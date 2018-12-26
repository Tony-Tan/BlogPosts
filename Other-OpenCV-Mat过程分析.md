---
title: 【OpenCV源代码分析】矩阵计算过程
categories:
  - Other
tags:
  - OpenCV
  - 矩阵计算
toc: true
date: 2017-12-29 10:54:33
---

**Abstract:** 本文通过分析opencv的Mat类的计算过程，来研究OpenCV的数据结构以及算法过程
**Keywords:** OpenCV，矩阵计算

<!--more-->
#  矩阵计算过程
2017年最后一篇博客，"2017年是不平凡的一年...."，今年下半年重新开始不定期更新博客，写了三四十篇，觉得还可以，求知欲可能是所有欲望里面最正面的，沉浸在求知的过程中的人内心更平静，因为当你知道周围的所有你生活的环境，人，事物，财富都将因时间化为云烟的时候，就跟他们没什么可以争论的了；留下点知识，或者思想，才是我们来到这个世界上的永不磨灭的印记，就像欧拉的《无穷分析引论》200年后依然影响了我，但是我已经不知道他们当时的首富是谁，他们的市长又是谁；
我这些烂博客肯定不能跟欧拉的著作相提并论，但是留下一些总比什么都没有好，而且如果我们继续努力，惊世神作也不是完全没可能的。
## OpenCV
OpenCV我2011年左右就开始了解到了，当时也是大学时候要做个小竞赛；那时之前看了钢铁侠，视觉心灵冲击大到影响了我的职业生涯，Jarves这种强人工智能暂且不说，《钢铁侠1》中的全息影像加手势交互给了我特别大的灵感，于是我做了个识别手指个数的小作品，参加了影响我一生的-西安电子科技大学生命科学技术学院星火杯科技竞赛，然后喜闻乐见的我得了个参与奖，当时用到opencv就是读取摄像头图像，自己编了一个算法在确定手指个数，现在看肯定naive但是当时完全是啥也不懂的情况下，没人指导，也没向谁请教，自己安装OpenCV，学51单片机和PC通信，焊电路板，然后还写了个MFC的界面，做到最后可以在竞赛现场演示，但是结果就是，参与奖，第一名是51单片机的光电对管的小应用，第二名是能吹灰的黑板擦（黑板擦加上一个小风扇）。。
跟高手过招，才能显示出身份，总跟下三路的对手pk，赢了没意义，输了更恶心。
后来尝试写图像处理库的时候，OpenCV更是帮我验证了很多算法，但是我一直有个疑惑OpenCV究竟是用什么办法处理一个矩阵中的不同数据类型的，典型的，我们有int ,char,float类型的数据，但是Mat类中指向数据的data 的指针是个uchar* 类型，也就是说Mat是个通用的，但是在算法实践中要把data转换成特定的类型，或者每种类型分别实现算法，那这样就工作量太大了，而且都是重复工作，而且我们并不知道OpenCV中矩阵这个类，和矩阵计算这些是怎么分块的，学习一下别人的分类模式可以帮助我们后面要做的PineNut提供很大的参考意义。
## Debug OpenCV
想要知道OpenCV是怎么计算的，最简单的方法就单步跟踪，如果不太明白单步追踪，下面的文章可以先不看，先去练习下写程序，但是单步调试的一个问题就是必须要有debug版本的OpenCV，OpenCV是开源的，cmake编译的时候设置成debug版本就可以了，debug版本与release版本的区别，这里也省略，因为跟我们关系也不是很大。
然后我们写了一小段应用调用OpenCV的加法：
```C++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace cv;

int main() {
    std::cout<<CV_VERSION<<std::endl;
    uchar A_8u[3][3] = { { 10, 5, 3 }, { 6, 4, 7 }, { 1, 0, 9 } };
    uchar B_8u[3][3] = { { 1, 3, 8 }, { 7, 5, 4 }, { 10, 6, 0 } };
    Mat A_m_8u(3, 3, CV_8S, A_8u);
    Mat B_m_8u(3, 3, CV_8S, B_8u);
    Mat C_m_8u;
    C_m_8u=A_m_8u + B_m_8u;
    std::cout<<C_m_8u<<std::endl;

    return 0;
}
```
这段程序主要是定义了两个Mat，然后使用+号对两个Mat进行相加，首先我们知道在C中是不能加两个结构体的，+一般只针对内置类型的数字字符等数据，结构体这种加法要自己定义函数，所以这个加号肯定是重载过的，也可以把它看做一个函数。
那么我们设置断点，进入加号

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Code-OpenCV-Mat过程分析/1-1.png)
果然根据我们的猜测，他进入了一个叫做MatExpr的类的加号运算符重载函数，并返回了一个MatExpr对象

```C++
MatExpr operator + (const Mat& a, const Mat& b)
{
    MatExpr e;
    MatOp_AddEx::makeExpr(e, a, b, 1, 1);
    return e;
}
```

接下来加法操作返回了，神奇的是这个加法操作没有执行任何循环或者加的动作，而是只返回了一个对象，也就是说这个对象中记载了要做的计算等到要执行的时候再执行，这样做有点类似tensorflow中的机制。
分析：重载的MatExpr符号操作（+）并没有执行任何计算操作，他只是记录了要做的的事，等到最后再做，这样做的好处可以减少一些不必要的计算，比如你这里有个A+B但是结果没有赋值给其他对象，那么这样的操作如果编译器没有处理掉的话是会浪费很多计算量，这种只记录不计算的方法就能避免，直到不得不计算的时候。
接着我们返回到main.cpp中，我们就得到了第一张图中序号①和②，先记录①的加法，然后在遇到②这种赋值符号的时候进行计算，但是要注意，这个赋值符号有讲究的，不是Mat赋值给Mat，而是MatExpr赋值给Mat 的时候，也就是说MatExpr我们把他叫做矩阵计算也好或者我们可以把它想象成矩阵的组合，里面有一张完整的计算图，当要把它赋值给Mat的时候进行计算，得到最终结果，这样在矩阵运算作为参数传递给函数的时候，这个方式也是可行的。
于是我们来到了MatExpr的赋值号（ \= ）重载函数：

```C++
inline
Mat& Mat::operator = (const MatExpr& e)
{
    e.op->assign(e, *this);
    return *this;
}
```
出现了一个新的对象op。这个对象的类是MatOp类
进入assign函数发现
```C++
void MatOp_AddEx::assign(const MatExpr& e, Mat& m, int _type) const
{
    Mat temp, &dst = _type == -1 || e.a.type() == _type ? m : temp;
    if( e.b.data )
    {
        if( e.s == Scalar() || !e.s.isReal() )
        {
            if( e.alpha == 1 )
            {
                if( e.beta == 1 )
                    cv::add(e.a, e.b, dst);//执行此函数
                else if( e.beta == -1 )
                    cv::subtract(e.a, e.b, dst);
                else
                    cv::scaleAdd(e.b, e.beta, e.a, dst);
            }
            else if( e.beta == 1 )
            {
                if( e.alpha == -1 )
                    cv::subtract(e.b, e.a, dst);
                else
                    cv::scaleAdd(e.a, e.alpha, e.b, dst);
            }
            else
                cv::addWeighted(e.a, e.alpha, e.b, e.beta, 0, dst);

            if( !e.s.isReal() )
                cv::add(dst, e.s, dst);
        }
        else
            cv::addWeighted(e.a, e.alpha, e.b, e.beta, e.s[0], dst);
    }
    else if( e.s.isReal() && (dst.data != m.data || fabs(e.alpha) != 1))
    {
        e.a.convertTo(m, _type, e.alpha, e.s[0]);
        return;
    }
    else if( e.alpha == 1 )
        cv::add(e.a, e.s, dst);
    else if( e.alpha == -1 )
        cv::subtract(e.s, e.a, dst);
    else
    {
        e.a.convertTo(dst, e.a.type(), e.alpha);
        cv::add(dst, e.s, dst);
    }

    if( dst.data != m.data )
        dst.convertTo(m, m.type());
}
```
op并不是MatOp类而是MatOp_AddEx类（应该是有继承关系的），所以这里使用了多态，那么这个类家族中应该还有：MatOp_Cmp，MatOp_T。。。等各种各样的继承（没错，这两个根本不是我猜的，是我去源代码里面扒出来的），这也就是说，矩阵运算（operation）下有很多具体的类，具体的实现。
我们继续走，发现会执行cv::add那句（加注释那句），所以我们继续跳转到这个函数：

```C++
void cv::add( InputArray src1, InputArray src2, OutputArray dst,
          InputArray mask, int dtype )
{
    CV_INSTRUMENT_REGION()

    arithm_op(src1, src2, dst, mask, dtype, getAddTab(), false, 0, OCL_OP_ADD );
}
```
于是我们来到了一个全局函数，这个函数的输入输出是一个新的类，InputArrary，OutputArray,有过opencv编程经验的人都应该知道，最好是写过C版本的opencv的同学都见过这个类，好多老函数都是用这个参数类型的，而且这个参数类型可以兼容CvMat，IplImage等好多类，所以这个输入输出类应该是一个兼容性设置，或者只读只写这类保护性作用的。然后接着一个干什么不知道的宏，然后进入到了arithm_op的函数，arithm_op有一个参数是个函数,
```C++
static BinaryFuncC* getAddTab()
{
    static BinaryFuncC addTab[] =
    {
        (BinaryFuncC)GET_OPTIMIZED(cv::hal::add8u), (BinaryFuncC)GET_OPTIMIZED(cv::hal::add8s),
        (BinaryFuncC)GET_OPTIMIZED(cv::hal::add16u), (BinaryFuncC)GET_OPTIMIZED(cv::hal::add16s),
        (BinaryFuncC)GET_OPTIMIZED(cv::hal::add32s),
        (BinaryFuncC)GET_OPTIMIZED(cv::hal::add32f), (BinaryFuncC)cv::hal::add64f,
        0
    };

    return addTab;
}
```
BinaryFuncC是个函数指针，这个函数定义一个数组，不同的元素对应着不同的数据类型·其中宏定义：
```C++
#define GET_OPTIMIZED(func) (func)
```
这种宏没有操作意义，但是可以增加代码的可读性，而且不影响源代码的编译质量，这样这个函数返回了一个函数指针，这个指针指向一个针对不同数据类型的函数族。
接下来进入了一个很长的，但是也是本过程最核心的部分：
```C++
static void arithm_op(InputArray _src1, InputArray _src2, OutputArray _dst,
                      InputArray _mask, int dtype, BinaryFuncC* tab, bool muldiv=false,
                      void* usrdata=0, int oclop=-1 )
{
    const _InputArray *psrc1 = &_src1, *psrc2 = &_src2;
    int kind1 = psrc1->kind(), kind2 = psrc2->kind();
    bool haveMask = !_mask.empty();
    bool reallocate = false;
    int type1 = psrc1->type(), depth1 = CV_MAT_DEPTH(type1), cn = CV_MAT_CN(type1);
    int type2 = psrc2->type(), depth2 = CV_MAT_DEPTH(type2), cn2 = CV_MAT_CN(type2);
    int wtype, dims1 = psrc1->dims(), dims2 = psrc2->dims();
    Size sz1 = dims1 <= 2 ? psrc1->size() : Size();
    Size sz2 = dims2 <= 2 ? psrc2->size() : Size();
#ifdef HAVE_OPENCL
    bool use_opencl = OCL_PERFORMANCE_CHECK(_dst.isUMat()) && dims1 <= 2 && dims2 <= 2;
#endif
    bool src1Scalar = checkScalar(*psrc1, type2, kind1, kind2);
    bool src2Scalar = checkScalar(*psrc2, type1, kind2, kind1);

    if( (kind1 == kind2 || cn == 1) && sz1 == sz2 && dims1 <= 2 && dims2 <= 2 && type1 == type2 &&
        !haveMask && ((!_dst.fixedType() && (dtype < 0 || CV_MAT_DEPTH(dtype) == depth1)) ||
                       (_dst.fixedType() && _dst.type() == type1)) &&
        ((src1Scalar && src2Scalar) || (!src1Scalar && !src2Scalar)) )
    {
        _dst.createSameSize(*psrc1, type1);
        CV_OCL_RUN(use_opencl,
            ocl_arithm_op(*psrc1, *psrc2, _dst, _mask,
                          (!usrdata ? type1 : std::max(depth1, CV_32F)),
                          usrdata, oclop, false))

        Mat src1 = psrc1->getMat(), src2 = psrc2->getMat(), dst = _dst.getMat();
        Size sz = getContinuousSize(src1, src2, dst, src1.channels());
        tab[depth1](src1.ptr(), src1.step, src2.ptr(), src2.step, dst.ptr(), dst.step, sz.width, sz.height, usrdata);
        return;
    }
...

```
我们只截取了我们要用到的部分，后面还有一段，但是我们没用到，这个函数进行了一些类类型，规模等的判断然后调用了tab数组（指向函数的数组）中的函数，并输入了正确的参数，其中prt()这个函数值得注意,然后我们进入了下一个函数：
```C++
void add8s( const schar* src1, size_t step1,
                   const schar* src2, size_t step2,
                   schar* dst, size_t step, int width, int height, void* )
{
    CALL_HAL(add8s, cv_hal_add8s, src1, step1, src2, step2, dst, step, width, height)
    vBinOp<schar, cv::OpAdd<schar>, IF_SIMD(VAdd<schar>)>(src1, step1, src2, step2, dst, step, width, height);
}
```
检查是否有HAL加速，发现没有，然后记者执行一个模板函数：
```C++
vBinOp<schar, cv::OpAdd<schar>, IF_SIMD(VAdd<schar>)>(src1, step1, src2, step2, dst, step, width, height);

```
进入这个函数：
```C++
template<typename T, class Op, class VOp>
void vBinOp(const T* src1, size_t step1, const T* src2, size_t step2, T* dst, size_t step, int width, int height)
{
#if CV_SSE2 || CV_NEON
    VOp vop;
#endif
    Op op;

    for( ; height--; src1 = (const T *)((const uchar *)src1 + step1),
                        src2 = (const T *)((const uchar *)src2 + step2),
                        dst = (T *)((uchar *)dst + step) )
    {
        int x = 0;

#if CV_NEON || CV_SSE2
#if CV_AVX2
        if( USE_AVX2 )
        {
            for( ; x <= width - 32/(int)sizeof(T); x += 32/sizeof(T) )
            {
                typename VLoadStore256<T>::reg_type r0 = VLoadStore256<T>::load(src1 + x);
                r0 = vop(r0, VLoadStore256<T>::load(src2 + x));
                VLoadStore256<T>::store(dst + x, r0);
            }
        }
#else
#if CV_SSE2
        if( USE_SSE2 )
        {
#endif // CV_SSE2
            for( ; x <= width - 32/(int)sizeof(T); x += 32/sizeof(T) )
            {
                typename VLoadStore128<T>::reg_type r0 = VLoadStore128<T>::load(src1 + x               );
                typename VLoadStore128<T>::reg_type r1 = VLoadStore128<T>::load(src1 + x + 16/sizeof(T));
                r0 = vop(r0, VLoadStore128<T>::load(src2 + x               ));
                r1 = vop(r1, VLoadStore128<T>::load(src2 + x + 16/sizeof(T)));
                VLoadStore128<T>::store(dst + x               , r0);
                VLoadStore128<T>::store(dst + x + 16/sizeof(T), r1);
            }
#if CV_SSE2
        }
#endif // CV_SSE2
#endif // CV_AVX2
#endif // CV_NEON || CV_SSE2

#if CV_AVX2
        // nothing
#elif CV_SSE2
        if( USE_SSE2 )
        {
            for( ; x <= width - 8/(int)sizeof(T); x += 8/sizeof(T) )
            {
                typename VLoadStore64<T>::reg_type r = VLoadStore64<T>::load(src1 + x);
                r = vop(r, VLoadStore64<T>::load(src2 + x));
                VLoadStore64<T>::store(dst + x, r);
            }
        }
#endif

#if CV_ENABLE_UNROLLED
        for( ; x <= width - 4; x += 4 )
        {
            T v0 = op(src1[x], src2[x]);
            T v1 = op(src1[x+1], src2[x+1]);
            dst[x] = v0; dst[x+1] = v1;
            v0 = op(src1[x+2], src2[x+2]);
            v1 = op(src1[x+3], src2[x+3]);
            dst[x+2] = v0; dst[x+3] = v1;
        }
#endif

        for( ; x < width; x++ )
            dst[x] = op(src1[x], src2[x]);
    }
}
```

哈哈哈哈，终于见到循环了没错这个就是最终的计算过程，为了找他我们也是煞费苦心了，最后这个函数里有很多加速的方式，SSE等这些指令集都是用来加速的，我们可以研究下怎么写，但还是优先优化算法，模板函数，这个就可以兼容所有的数据类型了，而且这个函数兼容所有的操作(op),循环里是展开写的，为了减少循环次数来减少其条件判断所带来的额外计算。

## 总结
这个就是简单的跟踪了下加法计算的全过程，下一篇就通过这些计算找出opencv 整个Mat计算所依靠的体系（各个类的作用，和设计目的）
我们通过下面这个表来看一下他们的调用关系，我们把MatExpr先暂时叫做计算图（类似于TensorFlow）


|     文件名      |     类      |   函数名   | 返回值  |作用|
|:---------------:|:-----------:|:----------:|:-------:|:-------:|
|    matop.cpp    |   MatExpr   | operator + | MatExpr |生成计算图|
|    matop.cpp    | MatOp_AddEx |  makeExpr  |  void   |MatOp的派生，为了完成不同的运算，MatOp派生了很多类|
|   mat.inl.hpp   |     Mat     | operator = |  Mat&   |重载，确定何时计算|
|    matop.cpp    | MatOp_AddEx |   assign   |  void   |执行计算图的计算|
|   arithm.cpp    |             |  cv::add   |  void   |通用加法计算，可以接受各种类型参数（Mat,CvMat,IplImage）|
|   arithm.cpp    |             | arithm_op  |  void   ||
|   arithm.cpp    |             |   add8s    |  void   |区别输入数据类型，调用vBinOp|
| arithm_core.hpp |             |   vBinOp   |  void   |模板函数，完成最终计算|



下一篇我们深入研究各个类





原文地址：[https://www.face2ai.com/Other-OpenCV-Mat过程分析](https://www.face2ai.com/Other-OpenCV-Mat过程分析)转载请标明出处
