---
title: 【CUDA 基础】6.5 流回调
categories:
    - CUDA
    - Freshman
tags:
    - 流回调
toc: true
date: 2018-06-20 21:56:18
---

**Abstract:** 本文介绍流回调
**Keywords:** 流回调

<!--more-->
# 流回调
流回调是一种特别的技术，有点像是事件的函数，这个回调函数被放入流中，当其前面的任务都完成了，就会调用这个函数，但是比较特殊的是，在回调函数中，需要遵守下面的规则
- 回调函数中不可以调用CUDA的API
- 不可以执行同步

流函数有特殊的参数规格，必须写成下面形式参数的函数;
```c++
void CUDART_CB my_callback(cudaStream_t stream, cudaError_t status, void *data) {
    printf("callback from stream %d\n", *((int *)data));
}
```
然后使用：
```c++
cudaError_t cudaStreamAddCallback(cudaStream_t stream,cudaStreamCallback_t callback, void *userData, unsigned int flags);
```
加入流中。
本文完整的代码在github:[https://github.com/Tony-Tan/CUDA_Freshman](https://github.com/Tony-Tan/CUDA_Freshman)（欢迎随手star😝 ）
部分代码
```c++
//
//
//
void CUDART_CB my_callback(cudaStream_t stream,cudaError_t status,void * data)
{
    printf("call back from stream:%d\n",*((int *)data));
}
//
//
//
//
//
int main(int argc,char **argv)
{
    //
    //
    //

    //asynchronous calculation
    int iElem=nElem/N_SEGMENT;
    cudaStream_t stream[N_SEGMENT];
    for(int i=0;i<N_SEGMENT;i++)
    {
        CHECK(cudaStreamCreate(&stream[i]));
    }
    cudaEvent_t start,stop;
    cudaEventCreate(&start);
    cudaEventCreate(&stop);
    cudaEventRecord(start,0);
    for(int i=0;i<N_SEGMENT;i++)
    {
        int ioffset=i*iElem;
        CHECK(cudaMemcpyAsync(&a_d[ioffset],&a_h[ioffset],nByte/N_SEGMENT,cudaMemcpyHostToDevice,stream[i]));
        CHECK(cudaMemcpyAsync(&b_d[ioffset],&b_h[ioffset],nByte/N_SEGMENT,cudaMemcpyHostToDevice,stream[i]));
        sumArraysGPU<<<grid,block,0,stream[i]>>>(&a_d[ioffset],&b_d[ioffset],&res_d[ioffset],iElem);
        CHECK(cudaMemcpyAsync(&res_from_gpu_h[ioffset],&res_d[ioffset],nByte/N_SEGMENT,cudaMemcpyDeviceToHost,stream[i]));
        CHECK(cudaStreamAddCallback(stream[i],my_callback,(void *)(stream+i),0));
    }
    //timer
    CHECK(cudaEventRecord(stop, 0));
    int counter=0;
    while (cudaEventQuery(stop)==cudaErrorNotReady)
    {
        counter++;
    }

    //
    //
    //
    //
    //
    //
}

```

结果如下：
![re-6-5-2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-6-5-流回调/re-6-5-2.png)


## 总结
本文介绍了本系列的最后一个小功能，流回调，下面部分就是中级提高篇了，要好好练习前面的，不然后面会懵逼哦！





原文地址1：[https://www.face2ai.com/CUDA-F-6-5-流回调](https://www.face2ai.com/CUDA-F-6-5-流回调)转载请标明出处
