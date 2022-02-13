# [Java格式化数字（1格式为01）_A_Runner的博客-CSDN博客](https://blog.csdn.net/A_Runner/article/details/82425484)

**“睡起秋声无觅处，满街梧桐月明处”**  
今天的需求是要给另外一个服务器传文件，文件名中有个编号自增的，例如：01，02，03……。  
其中的核心代码如下：

```
public static void main(String[] args) {
        //String.format（）方法的作用就是格式化输出参数. “%02d”是指定输出格式，%作先导标记，0表示自动补0, 2的意思是最小长度为2（如果用4，则1输出0001），d表示整数。
       System.out.println(String.format("%02d",1));
       System.out.println(String.format("%02d",5));
       System.out.println(String.format("%02d",25));
 }
```

输出结果为：  
![这里写图片描述](https://img-blog.csdn.net/20180905161010439?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FfUnVubmVy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
