# [使用Python去除Chrome重复、无效的书签链接_shenghaomail的专栏-CSDN博客](https://blog.csdn.net/shenghaomail/article/details/88561302)

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[shenghaomail](https://blog.csdn.net/shenghaomail "垃圾中文技术性网站") 2019-03-14 21:35:24 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 945 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏  1 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

长时间使用浏览器，没有了新鲜感，伴随着不爽与忍耐，尽管通过各种方式反馈给官方，然并卵。当忍耐到了尽头，就百度“浏览器”，然后找下一个评分最高的，又再次开始了不爽与忍耐。

现在的浏览器或本身具备、或安装插件的方式，都提供了比较强大的扩展功能，我最喜欢的变异鼠标手势与保存密码。换浏览器的时候不仅仅是安装浏览器这么简单，还需要转移书签，还有安装符合习惯的插件以及转移网站密码。

对于插件和网站密码，在这里不做讨论，毕竟没有这么高的技术能力。在此先来讨论一下换浏览器之后的**书签**。

在这里介绍一下我换了Chrome浏览器之后，对书签做的瘦身操作。

## 目标

1.  将书签里的链接，按照URL进行去重，然后把重复的、目录较深的书签路径加书签名打印出来
2.  测试去重后剩下来的链接，判断当初收藏的网站是否还在线。

## 本机环境

`系统: Windows 10`  
`Python版本: Python 3.7`  
`Chrome版本: 72.0.3626.121`

## 步骤

1.  解析Chrome书签文件，获取书签名、书签URL以及书签目录
2.  依次检查书签名和书签URL，判断是否与之前的重复
3.  若重复，则判断两个书签哪一个目录层级比较深，若相同，打印第二个
4.  测试去重后的所有书签URL，打印失效链接

## 不足之处

1.  Chrome所有书签保存在一个bookmarks文件中，并且有一定的校验算法，以我目前的能力，无法直接在bookmarks文件中直接删除重复的书签，只能打印出目录及书签名，然后在浏览器中手动删除(**大写尴尬**)。
2.  测试URL的有效性时，因为某种无上伟大的迷之大法，只能校验墙内的链接。可自行“卍解”后测试。

## 脚本

```
# -*- coding: utf-8 -*-
import os
import json
import requests


def read_file():
    """读取书签文件内容"""
    # 书签路径
    google_fav_file_path = 'C:/Users/sheng/AppData/Local/Google/Chrome/User Data/Default/Bookmarks'
    if os.path.exists(google_fav_file_path):
        with open(google_fav_file_path, 'rb') as r:
            goog_fav = r.read().decode('utf-8')
    return goog_fav


def get_bookmark_total(bookstr):
    """将书签文件转换成Python字典"""
    if len(book_str):
        book_json = json.loads(book_str)

    # 得到主要内容
    children_bar = book_json['roots']['bookmark_bar']['children']
    return children_bar


def get_detail(children: list, name, index):
    """递归解析书签信息"""
    if len(children) > 0:
        for child in children:
            if 'children' in child and child.get('type') == 'folder':
                # 书签文件夹，进行递归解析
                get_detail(child.get('children'), f'{name} -- {child.get("name")}', index+1)

            if child.get('type') == 'url':
                # 书签URL，进行去重判断
                print_complex_url(index, name, child.get('name'), child.get('url'))


total_fav = dict()
def print_complex_url(index, direct, name, url):
    """按照书签名检查是否重复"""
    if f'{name}' in total_fav:
        # 重复
        if index >= total_fav.get(f'{name}')[0]:
            print(direct, name)
        else:
            print(total_fav.get(f'{name}')[1], name)
            total_fav[f'{name}'] = [index, direct, name, url]
    else:
        # 不重复
        total_fav[f'{name}'] = [index, direct, name, url]


def is_bookmarks_active():
    """检查去重后的书签是否有效"""
    for key in total_fav:
        url = total_fav.get(key)[3]
        is_ok = is_url_ok(url)
        if is_ok in [-1, -2, -3]:
            print(total_fav.get(key)[1], key)


def is_url_ok(url):
    """判断URL是否有效"""
    try:
        resu = requests.get(url, timeout=5)
        if resu.status_code == 200:
            # 正常
            return 0
        else:
            return -3
    except requests.exceptions.ConnectionError as e:
        return -1
    except Exception as e:
        return -2


if __name__ == '__main__':
    book_str = read_file()
    children_bar = get_bookmark_total(book_str)
    # print(type(children_bar))
    get_detail(children_bar, 'root', 0)
    print('检查书签重复完成')
    is_bookmarks_active()


```
