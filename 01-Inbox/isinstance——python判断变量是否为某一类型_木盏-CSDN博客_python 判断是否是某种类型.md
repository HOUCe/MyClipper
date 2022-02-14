# [isinstance——python判断变量是否为某一类型_木盏-CSDN博客_python 判断是否是某种类型](https://blog.csdn.net/leviopku/article/details/100138592)

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[木盏](https://muzhan.blog.csdn.net/ "垃圾中文技术性网站") 2019-08-29 15:06:31 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 4334 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏  2 

python编程时经常会用到变量类型作为if的判断依据，如果直接 if type(var) == 'int'，则输出肯定是False的。因为哪怕你的变量就是int类型，type(var)和'int'也不划等号。

这个时候，有一个函数：isinstance

用法如下：

```
img = cv2.imread('test.jpg')print(isinstance(img, numpy.ndarray))
```

上面脚本输出结果就是True了~

文章知识点与官方知识档案匹配，可进一步学习相关知识

\>

12-14 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 933 

08-30 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 3651 

 [![](https://profile.csdnimg.cn/0/4/F/3_fisher0113)](https://blog.csdn.net/Fisher0113 "垃圾中文技术性网站") 

<

05-13 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 295 

01-29 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 253 

06-25 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 3万+ 

08-26 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 416 

[

def augmen_ta_tion(im: np.ndarray, text\_polys: np.ndarray, scales: np.ndarray, degrees: int, input\__si_ze: int) -> tuple: # _th_e images are rescaled wi_th_ ratio {0.5, 1.0, 2.0, 3.0} randomly im, text\_polys = da_ta_\_aug.random\_scale(im, text\_polys, scal.

](https://blog.csdn.net/u012483097/article/details/108241481 "垃圾中文技术性网站")

10-01 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 6万+ 

11-29 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 48 

10-04 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 4554 

01-29 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 2482 

08-28 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 1万+ 

06-30 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 1万+ 

09-17 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 102 

08-12 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 3万+ 

11-27 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 6290 

11-01 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 1917 

07-17 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/readCountWhite.png) 2527 

©️2021 CSDN 皮肤主题: 精致技术 设计师:CSDN官方博客 [返回首页](https://blog.csdn.net/ "垃圾中文技术性网站")
