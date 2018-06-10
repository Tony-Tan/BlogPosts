---
title: 【CUDA FRESHMAN】5.4 合并的全局内存访问
categories:
    - CUDA
    - Freshman
tags:
    - 合并
    - 转置
toc: true
date: 2018-06-04 21:34:22
---

**Abstract:** 本文介绍使用共享内存进行矩阵转置以减少内存的交叉访问
**Keywords:** 合并，转置

<!--more-->
## 开篇废话
没废话，看以前的废话感觉自己像个傻瓜。。就像以后看我正在写的文字一样。
还记得我们矩阵转置的例子么，在全局内存部分介绍的：[4.4核函数可达到的带宽](https://face2ai.com/CUDA-F-4-4-核函数可达到的带宽/)
在4.4中我们当时只有共享内存这一种工具可以使用，为了达到最高效率，我们要配合一级缓存，二级缓存进行编程，来提高转置的效率，因为转置只能在行读取列写入或者列读取行写入之间选择一个，这样就必然会引发非合并的访问，虽然我们利用一级缓存的性质可以提高性能，但是我们今天会介绍我们的新工具共享内存，在共享内存中完成转置后写入全局内存，这样就可以避免交叉访问了。

## 基准转置内核
在介绍我们的神奇共享内存之前，我们最好先研究出来一下我们的问题的极限在哪，换句话说，我们需要清楚的知道我们最慢的情况（最简单的方式能达到的速度）以及最快的理论速度，理论速度可能会达不到，但是可以接近，最慢速度肯定可以超越，你永远可以写出更慢的程序，所以我们用最简单的方法作为下界，而用正行读取，然后不经变换的写入来作为上限，这一招我们在前面使用过，就是在4.4中，那次我们突破极限了（哈哈，很有可能是计时有问题），但是正常来讲，极限是最好的参考值。
完整的代码在github:[https://github.com/Tony-Tan/CUDA_Freshman](https://github.com/Tony-Tan/CUDA_Freshman)（欢迎随手star😝 ）
上限：
```c++
__global__ void copyRow(float * in,float * out,int nx,int ny)
{
    int ix=threadIdx.x+blockDim.x*blockIdx.x;
    int iy=threadIdx.y+blockDim.y*blockIdx.y;
    int idx=ix+iy*nx;
    if (ix<nx && iy<ny)
    {
      out[idx]=in[idx];
    }
}
```
下限是我们的too young too naive版本，就是最常规的方法：
```c++
__global__ void transformNaiveRow(float * in,float * out,int nx,int ny)
{
    int ix=threadIdx.x+blockDim.x*blockIdx.x;
    int iy=threadIdx.y+blockDim.y*blockIdx.y;
    int idx_row=ix+iy*nx;
    int idx_col=ix*ny+iy;
    if (ix<nx && iy<ny)
    {
      out[idx_col]=in[idx_row];
    }
}
```
这两段代码中第一段并没有转置的功能，只是为了测试上限，第二段是naive的转置，前面也讲过，这里就直接贴结果了
copyRow的cpu计时和nvprof结果：
![re-1](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-1.png)
transformNaiveRow的cpu计时和nvprof结果：

![re-2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-2.png)

我们可以得到下表：

|      核函数       |  CPU计时   | nvprof计时 |
|:-----------------:|:----------:|:----------:|
|      copyRow      | 0.001442 s | 1.4859 ms  |
| transformNaiveRow | 0.003964 s | 3.9640 ms  |

可以看出计cpu计时还是比较准的，在数据量比较大情况下，我们现在的矩阵大小是 $2^{12}\times 2^{12}$ 的大小。
然后是加载和存储全局内存请求的平均事务数（越少越好）
copyRow:
![re-3](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-3.png)

transformNaiveRow:
![re-4](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-4.png)

接着我们就开始用共享内存进行操作了。

## 使用共享内存的矩阵转置
为了避免交叉访问，我们可以使用二维共享内存缓存原始矩阵数据，然后从共享内存中读取一列存储到全局内存中，因为共享内存按列读取不会导致交叉访问那么严重的延迟，所以这种想法是可以提高效率的，但是前面一篇我们说这种案列访问共享内存会造成冲突，所以我们先来按照最简单的方式来使用共享内存，来测试下看看有多少性能提高：

![re-5](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-5.png)
上面这个图就是我们的转置过程
- 从全局内存读取数据（按行）写入共享内存（按行）
- 从共享内存读取一列写入全局内存的一行

如果从串行的角度看，我们有一个二维矩阵其存储在内存的时候是一维的，一般我们是把二维矩阵按照逐行的方式放入一维内存中，转置的过程可以理解为把逐行从上到下数据改成逐列，从左到右的数据。
由于过程很简单，我们直接看代码，这里我们可以使用正方形的共享内存也可以使用矩形的共享内存，我们按照一般情况下的情况使用矩形的共享内存。
```c++
__global__ void transformSmem(float * in,float* out,int nx,int ny)
{
	__shared__ float tile[BDIMY][BDIMX];
	unsigned int ix,iy,transform_in_idx,transform_out_idx;
// 1
	ix=threadIdx.x+blockDim.x*blockIdx.x;
    iy=threadIdx.y+blockDim.y*blockIdx.y;
	transform_in_idx=iy*nx+ix;
// 2
	unsigned int bidx,irow,icol;
	bidx=threadIdx.y*blockDim.x+threadIdx.x;
	irow=bidx/blockDim.y;
	icol=bidx%blockDim.y;
// 3
	ix=blockIdx.y*blockDim.y+icol;
	iy=blockIdx.x*blockDim.x+irow;
// 4
	transform_out_idx=iy*ny+ix;
	if(ix<nx&& iy<ny)
	{
		tile[threadIdx.y][threadIdx.x]=in[transform_in_idx];
		__syncthreads();
		out[transform_out_idx]=tile[icol][irow];

	}
}
```
结合下图，看一下我们的代码过程：

![1-2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/1-2.png)

解释代码，这段代码是通用代码，并不需要矩阵为正方形（结合上面的图，过程会更清晰）：
1. 计算当前块中的线程的全局坐标（相对于整个网格），计算对应的一维线性内存的位置
2. bidx表示block idx也就是在这个快中的线程的坐标的线性位置（把块中的二维线程位置按照逐行排布的原则，转换成一维的），然后进行转置，也就是改成逐列排布的方式，计算出新的二维坐标，逐行到逐列排布的映射就是转置的映射，这只完成了很多块中的一块，而关键的是我们把这块放回到哪
3. 计算出转置后的二维全局线程的目标坐标，注意这里的转置前的行位置是计算出来的是转置后的列的位置，这就是转置的第二步。
4. 计算出转置后的二维坐标对应的全局内存的一维位置
5. 读取全局内存，写入共享内存，然后按照转置后的位置写入

上面5个过程可以理解为两步
- 把共享内存内的矩阵进行转置，如果你傻乎乎的真的分配两块共享内存，然后转置，就有点傻了，当然也不赞成你按照行读取全局内存然后按列写入（我开始就是这么想的），而是建立一种转置的映射关系，完成分块转置中的某一个块的转置。
- 找到上面转置完了的块的新位置，然后写入全局内存。

看一下cpu计时

![re-5-5](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-5-5.png)

nvprof结果;

![re-6](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-6.png)

从0.003964 s到0.001803 s 看起来还不错。速度提升了一倍。
然后看一下平均内存事务:

![re-7](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-7.png)

stroe过程的事务数从32降低到4，还是很显著的。我们看一下共享内存的事务数，来看看是否有冲突：

![re-8](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-8.png)

毫不意外，16路冲突，store没冲突，主要来自load，解决这个问题，我们使用的方法只有填充


## 使用填充共享内存的矩阵转置
解决共享内存冲突的最好办法就是填充，我们来填充下我们的共享内存；
```c++
__global__ void transformSmemPad(float * in,float* out,int nx,int ny)
{
	__shared__ float tile[BDIMY][BDIMX+IPAD];
	unsigned int ix,iy,transform_in_idx,transform_out_idx;
	ix=threadIdx.x+blockDim.x*blockIdx.x;
    iy=threadIdx.y+blockDim.y*blockIdx.y;
	transform_in_idx=iy*nx+ix;

	unsigned int bidx,irow,icol;
	bidx=threadIdx.y*blockDim.x+threadIdx.x;
	irow=bidx/blockDim.y;
	icol=bidx%blockDim.y;


	ix=blockIdx.y*blockDim.y+icol;
	iy=blockIdx.x*blockDim.x+irow;


	transform_out_idx=iy*ny+ix;

	if(ix<nx&& iy<ny)
	{
		tile[threadIdx.y][threadIdx.x]=in[transform_in_idx];
		__syncthreads();
		out[transform_out_idx]=tile[icol][irow];

	}

}
```
为一的不同就是IPAD，我们主要看指标：
运行结果：

![re-8-5](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-8-5.png)
可见执行速度从0.0018降低到0.0015
nvprof结果：
![re-9](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-9.png)
我们主要关注共享内存事务,全局事务没有改变。
![re-11](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-11.png)
还有冲突，我们改变IPAD试试（从1到2）

![re-12](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-12.png)
哈哈，冲突不见了，执行CPU计时：

![re-13](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-13.png)
冲突解决了但是似乎速度没有显著提升。
## 使用展开的矩阵转置
在共享内存进行填充消除冲突后，我们还想继续提高性能，那么我们使用前面提到了另一个大杀器，展开循环，节省大量的线程块，提高带宽利用率：
废话不多说，直接上代码，然后再解释：

```c++
__global__ void transformSmemUnrollPad(float * in,float* out,int nx,int ny)
{
	__shared__ float tile[BDIMY*(BDIMX*2+IPAD)];
//1.
	unsigned int ix,iy,transform_in_idx,transform_out_idx;
	ix=threadIdx.x+blockDim.x*blockIdx.x*2;
    iy=threadIdx.y+blockDim.y*blockIdx.y;
	transform_in_idx=iy*nx+ix;
//2.
	unsigned int bidx,irow,icol;
	bidx=threadIdx.y*blockDim.x+threadIdx.x;
	irow=bidx/blockDim.y;
	icol=bidx%blockDim.y;
//3.
	unsigned int ix2=blockIdx.y*blockDim.y+icol;
	unsigned int iy2=blockIdx.x*blockDim.x*2+irow;
//4.
	transform_out_idx=iy2*ny+ix2;
	if(ix+blockDim.x<nx&& iy<ny)
	{
		unsigned int row_idx=threadIdx.y*(blockDim.x*2+IPAD)+threadIdx.x;
		tile[row_idx]=in[transform_in_idx];
		tile[row_idx+BDIMX]=in[transform_in_idx+BDIMX];
//5
		__syncthreads();
		unsigned int col_idx=icol*(blockDim.x*2+IPAD)+irow;
        out[transform_out_idx]=tile[col_idx];
		out[transform_out_idx+ny*BDIMX]=tile[col_idx+BDIMX];

	}
}

```
没错，一个线程干两个线程的事，或者说一个块干两个块的事，其实结合前面的共享内存转置的详细说明，这个就很好理解了，但是我还是再写一遍，以免有遗漏，或者解释不清楚。
当然，看图更清楚，注意与上面的代码进行对比：

![1-3](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/1-3.png)

1. 计算当前块中的线程的全局坐标——一维线性内存的位置
2. 在共享内存内进行转置
3. 计算出转置后的二维全局线程的目标坐标，注意这里的转置前的行位置是计算出来的是转置后的列的位置，这就是转置的第二步。
4. 计算出转置后的二维坐标对应的全局内存的一维位置，注意这里不是一次计算一个块，而是计算两个块，换个理解方法，我们把原来的块x方向扩大一倍，然后再对这个大块分成两个小块（，B），每个小块中的对应位置差BDIMX，然后对其中A，B中数据按行写入共享内存。
5. 将4中读取到共享内存中的数据，按照转置后的位置写入全局内存。

这个过程看起来不麻烦，写起来有点麻烦，但是有一个技巧，就是画图，你画个图就基本明白了。
我们看一下nvprof的结果，以及全局内存的事务。
nvprof：

![re-1-13-5](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-1-13-5.png)

全局内存事务：
![re-14](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-14.png)

可以看到在我们当前的环境下，展开并没有提速，我们可以展开更多试试，这里就不做试验了。


## 增大并行性
增大并行性的办法是调整核函数的配置，我们调整成 $16\times 16$ 的块，可以得到下面的结果：

![re-15](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-15.png)

调整成 $8\times 8$ 的块的结果如下
![re-16](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-4-合并的全局内存访问/re-16.png)

这个要考大家不停的试才能找到当前设备最优的。

## 总结
本文使用共享内存避免交叉的全局内存访问，优化了矩阵转置的速度。
