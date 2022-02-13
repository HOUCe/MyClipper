# python 获取微信公众号的文章（持续更新）\_低调城主的博客-程序员ITS404

[www.its404.com](https://www.its404.com/article/robert789654/109843461)

需求场景：关注很多的微信公众号，有时候看到很好的文章，过段时间再想查看下，发现找不到历史的文章记录了，也没有一个根据文章名称检索的地方。现在利用python爬虫爬取微信公众号的文章，数据存入到数据库中。可以定时读取微信公众号的最新文章，方便日后的读取和查询。

实现思路：通过微信公众号登录获取想要的微信公众好的fakeid，token和cookie（token和cookie是每天更新的，这个目前还没有实现自动获取，后续更新会继续）。现将步骤和代码奉上

步骤一：登录微信公众号获取token和cookie，fakeid

登录微信公众--创作管理--图文素材

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131459177.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131552976.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131617439.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131650194.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131745784.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131845774.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120131916134.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

步骤二：通过代码获取文章列表把数据存入mysql

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    # @File  : articleCollection.py
    # datetime:2020/11/5 10:58
    
    
    import time
    import random
    import requests
    import json
    import pandas as pd
    import mysql
    from cookie import getCookie, getFakeId
    
    
    # 毫秒数转日期
    def getDate(times):
        # print(times)
        timearr = time.localtime(times)
        date = time.strftime("%Y-%m-%d %H:%M:%S", timearr)
        return date
    
    
    def listAllArticle(fakeId):
        # with open('cookie.txt', 'r', encoding='utf-8') as f:
        #     cookie = f.read()
        # cookies = json.loads(cookie)
        # 目标url
        url = "https://mp.weixin.qq.com/cgi-bin/appmsg"
        # 使用Cookie，跳过登陆操作
        headers = {
            "Cookie": "appmsglist_action_3253322613=card; pgv_pvi=1927635968; RK=qb4RrLzDY3; ptcz=8e89a2e6f2a59b1a3f8a0f31ddac1c9377a330495aa98d6782318128d951fbde; pgv_pvid=3216855634; o_cookie=876860131; pac_uid=1_876860131; sd_userid=48741598419620498; sd_cookie_crttime=1598419620498; ua_id=HyCPg850LnH9CM3IAAAAAERZlptrn4Ugx5qlWDhDiUk=; mm_lang=zh_CN; openid2ticket_oFz_G00ma08fbv-hIlBEH6O6q6fE=; noticeLoginFlag=1; openid2ticket_oYj8c00Jx-5q4pqWDv11SI2X5ofc=; openid2ticket_oiYgP0Uqf2UZrNA391GmMDwJGcCE=; openid2ticket_ojB5L5CfumKlgHCgLAklgd5KEx4Y=; wxuin=03948109754498; remember_acct=463451476%40qq.com; ts_uid=611546807; openid2ticket_oVOWI5PW7Zm4a3wtuO1qNGNvoz-c=; luin=o0876860131; lskey=000100000766fb94649b295f405b49b4fccf586695f86fce7607f4d8ae57d26bbcb0301dbbf27c6270924611; openid2ticket_ooIDA4uHEZzqbrxbGu9AbKC12u-Q=; pgv_si=s1613571072; uuid=2e283eb009db08eaf4670f9242d7552f; rand_info=CAESIALz/RAVT8BuYAVfLpMNUKBAqgZZl6Edk1INJYS1j5AJ; slave_bizuin=3253322613; data_bizuin=3236413511; bizuin=3253322613; data_ticket=gU6uFdhksZuwfq6BMxYWp1PTkqrSZbknRl67RSBCVqcR65/ypmY41ldD5fsQ5/F0; slave_sid=Q3lsZHBsZ0JaQTdwV1gwMlpVSkU4dTBRRng2aVZJUXdPbDJrc0FNUWFXZ011VnRqVlZ1Z28xektlZzdfZGc2Wlo3VDV4R1h3NjY5dWJKZDRZTmNRV1NRQzdJNUR6UDBHQWVRVGRZcWIyTExCbkdDSURHYUdFT0FaVE9mazBPNDB0MUpGdkJFZHlUeUpNWFBs; slave_user=gh_142dc657372c; xid=6a58b8adab44b39b8aa19a7fb675f9c9; openid2ticket_oR8DnwJPMyNDbpsKFzdTxkqtKcmk=aXPZY0GRnsCCNvQJDcRjByPJv4/HgJg1agmjuL8MlQk=",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36",
        }
        data = {
            "token": "379017573",
            "lang": "zh_CN",
            "f": "json",
            "ajax": "1",
            "action": "list_ex",
            "begin": "0",
            "count": "5",
            "query": "",
            "fakeid": fakeId,
            "type": "9",
        }
        for i in range(2):
            data["begin"] = i * 5
            # 生成3-10的随机整数，方便下面程序间隔时间
            sleepTime = random.randint(3, 10)
            print(sleepTime)
            time.sleep(sleepTime)
            content_json = requests.get(url, headers=headers, params=data).json()
            for item in content_json["app_msg_list"]:
                # 提取每页文章的标题及对应的url
                items = [item["title"], item["link"], item["cover"], getDate(item["create_time"]), item["digest"],
                         item["item_show_type"], getDate(item["update_time"]), ''.join(fakeId)]
                print(items)
                mysql.saveWeChatArticle(items)
    
    
    if __name__ == '__main__':
        listFakeId = mysql.listWeChatFilId()
        for i in listFakeId:
            print(''.join(i))
            listAllArticle(i)
    

从mysql中查询faikid和把文章列表存入mysql

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    # @File  : mysql.py
    # datetime:2020/11/5 10:44
    import pymysql
    
    from dbutils.pooled_db import PooledDB
    
    POOL = PooledDB(
        creator=pymysql,  # 使用链接数据库的模块
        maxconnections=6,  # 连接池允许的最大连接数，0和None表示不限制连接数
        mincached=2,  # 初始化时，链接池中至少创建的空闲的链接，0表示不创建
        maxcached=5,  # 链接池中最多闲置的链接，0和None不限制
        maxshared=3,
        # 链接池中最多共享的链接数量，0和None表示全部共享。PS: 无用，因为pymysql和MySQLdb等模块的 threadsafety都为1，所有值无论设置为多少，_maxcached永远为0，所以永远是所有链接都共享。
        blocking=True,  # 连接池中如果没有可用连接后，是否阻塞等待。True，等待；False，不等待然后报错
        maxusage=None,  # 一个链接最多被重复使用的次数，None表示无限制
        setsession=[],  # 开始会话前执行的命令列表。
        ping=0,
        # ping MySQL服务端，检查是否服务可用。
        host='10.63.4.53',
        port=20318,
        user='root',
        password='f608a3fd',
        database='sy_jzfp',
        charset='utf8'
    )
    
    
    def func():
        # 检测当前正在运行连接数的是否小于最大链接数，如果不小于则：等待或报raise TooManyConnections异常
        # 否则 则优先去初始化时创建的链接中获取链接 SteadyDBConnection。
        # 然后将SteadyDBConnection对象封装到PooledDedicatedDBConnection中并返回。
        # 如果最开始创建的链接没有链接，则去创建一个SteadyDBConnection对象，再封装到PooledDedicatedDBConnection中并返回。
        # 一旦关闭链接后，连接就返回到连接池让后续线程继续使用。
        conn = POOL.connection()
    
        # print(th, '链接被拿走了', conn1._con)
        # print(th, '池子里目前有', pool._idle_cache, '\r\n')
    
        cursor = conn.cursor()
        cursor.execute('select * from two_clour_two')
        result = cursor.fetchall()
        for i in result:
            print(i)
        conn.close()
    
    
    # 数据库插入操作
    def saveDouBan(dict):
        conn = POOL.connection()
        cursor = conn.cursor()
        sql = "insert into two_clour_two (`code`, `red_1`, `red_2`, `red_3`, `red_4`, `red_5`, `red_6`, `blue`,`sum`,`std`,`var`,`create_date`) values(\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\")" % (
            str(dict[0]), str(dict[1]), str(dict[2]), str(dict[3]), str(dict[4]), str(dict[5]), str(dict[6]),
            str(dict[7]), str(dict[8]), str(dict[9]), str(dict[10]), str(dict[11]))
        cursor.execute(sql)
        print('Successful')
        conn.commit()
        cursor.close()
    
    
    # 数据库插入操作
    def saveWeChatArticle(dict):
        # name = ['title', 'link', 'cover', 'create_time', 'digest', 'item_show_type', 'update_time', 'fakeid']
        conn = POOL.connection()
        cursor = conn.cursor()
        sql = "insert into we_chat_article (`title`, `link`,`cover`, `create_time`, `digest`, `item_show_type`, `update_time`, `fakeid`) values(\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\")" % (
            pymysql.escape_string(dict[0]), str(dict[1]), str(dict[2]),
            str(dict[3]), str(dict[4]), str(dict[5]),
            str(dict[6]), str(dict[7]))
        cursor.execute(sql)
        # print('Successful')
        conn.commit()
        cursor.close()
    
    
    def listWeChatFilId():
        conn = POOL.connection()
        cursor = conn.cursor()
        cursor.execute("select fake_id from wechat_public where del_flag ='0' ")
        result = cursor.fetchall()
        # for i in result:
        #     print(i[0])
        conn.close()
        return result
    
    
    if __name__ == '__main__':
        listWeChatFilId()
    

执行效果和结果如下

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120132619747.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120132657999.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

后记：改方法目前存在问题如如下：

1、不能实现自动的获取token和faikid；每天需要手动获取；

2、定时任务还没有加，获取的文章有一些重复的（目前的做法是取前两页列表的数据，重复的没有判断）

3、获取条数过多会被腾讯封IP；

4、后面会把数据接入微信小程序中。

这个是自己目前开发的![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120133328521.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)一个app还不成熟![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201120133315253.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvYmVydDc4OTY1NA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70)

[查看原网页: www.its404.com](https://www.its404.com/article/robert789654/109843461)