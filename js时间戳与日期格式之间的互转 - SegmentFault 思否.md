# [js时间戳与日期格式之间的互转 - SegmentFault 思否](https://segmentfault.com/a/1190000000481753)

### 1\. 将时间戳转换成日期格式

var date = new Date(时间戳); 


date.getFullYear();  
date.getMonth();  
date.getDate();  
date.getTime();  
date.getHours();  
date.getMinutes();  
date.getSeconds();  

### 例子

var date = new Date(1398250549490);
Y = date.getFullYear() + '-';
M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
D = date.getDate() + ' ';
h = date.getHours() + ':';
m = date.getMinutes() + ':';
s = date.getSeconds(); 
console.log(Y+M+D+h+m+s); 

___

### 2\. 将日期格式转换成时间戳

var strtime = '2014-04-23 18:55:49:123';
var date = new Date(strtime); 

var date = new Date(strtime.replace(/-/g, '/'));


time1 = date.getTime();
time2 = date.valueOf();
time3 = Date.parse(date);

___

### 3\. _Date()参数形式有7种_

1.  _new Date("month dd,yyyy hh:mm:ss");_
    
2.  _new Date("month dd,yyyy");_
    
3.  _new Date("yyyy/MM/dd hh:mm:ss");_
    
4.  _new Date("yyyy/MM/dd");_
    
5.  _new Date(yyyy,mth,dd,hh,mm,ss);_
    
6.  _new Date(yyyy,mth,dd);_
    
7.  _new Date(ms);_
    

比如：

1.  new Date("September 16,2016 14:15:05");
    
2.  new Date("September 16,2016");
    
3.  new Date("2016/09/16 14:15:05");
    
4.  new Date("2016/09/16");
    
5.  new Date(2016,8,16,14,15,5); // 月份从0～11
    
6.  new Date(2016,8,16);
    
7.  new Date(1474006780);
