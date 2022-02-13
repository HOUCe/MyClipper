# Python Hexo博文图片上传图床并自动替换链接的Python脚本

[blog.51cto.com](https://blog.51cto.com/u_11949039/2834780)

    import re
    import requests
    import json
    from qcloud_cos import CosConfig
    from qcloud_cos import CosS3Client
    import sys
    #import logging
    import argparse
    import os
    import time
    import shutil
    from urllib import parse
     
    def get_artical_path(art_tittle):
            '''
            合成文章路径
            '''
            try:
                    dir_path = os.getcwd()
                    #如果是在博客根目录放置脚本，请修改testdir\_posts路径为source\_posts或其他放置文章的目录。
                    article_path = os.path.join(dir_path,r"source\_posts",art_tittle)
                    return article_path
                    print(article_path)
            except BaseException as err:
                    print("error in get_artical_path\n{}".format(err))
     
    def change_pic_path(article_path,upld_method): 
            '''
            article_path: 文章的本地路径
            upld_method: 上传方法，sm.ms，腾讯cos，本地（github）。
            '''
            print("please wait a moment")
            try:
                    with open (article_path,'r',encoding = 'utf-8') as md:
                            article_content = md.read()
                            pic_block = re.findall(r'\!.*?\)',article_content)  #获取添加图片的Markdown文本
                            if upld_method == "smms":
                                    for i in range(len(pic_block)):
                                            pic_origin_url = re.findall(r'\((.*?)\)',pic_block[i]) #获取插入图片时图片的位置
                                            pic_new_url = smms(pic_origin_url[0])
                                            print("pic_new_url is {}".format(pic_new_url))
                                            article_content = article_content.replace(pic_origin_url[0],pic_new_url)
                            elif upld_method == "tx":
                                    for i in range(len(pic_block)):
                                            pic_origin_url = re.findall(r'\((.*?)\)',pic_block[i]) #获取插入图片时图片的位置
                                            #print("pic_origin_url is {}".format(pic_origin_url))
                                            pic_new_url = tx(pic_origin_url[0])
                                            print("pic_new_url is {}".format(pic_new_url))
                                            article_content = article_content.replace(pic_origin_url[0],pic_new_url)
                            elif upld_method == "local":
                                    for i in range(len(pic_block)):
                                            pic_origin_url = re.findall(r'\((.*?)\)',pic_block[i]) #获取插入图片时图片的位置
                                            pic_new_url = local(pic_origin_url[0])
                                            print("pic_new_url is {}".format(pic_new_url))
                                            article_content = article_content.replace(pic_origin_url[0],pic_new_url)
                            else:
                                    print("part of get_pic_path error")
     
                    with open (article_path,'w',encoding = 'utf-8') as md:
                            md.write(article_content)
                    print("job done")
     
            except BaseException as err:
                    print("error in change_pic_path\n{}".format(err))
     
    #上传至SM.MS
    def smms(pic_origin_url):
            try:
                    smms_url = 'https://sm.ms/api/upload'
                    #file_path = r"C:\Users\Root\AppData\Roaming\Typora\typora-user-images\1542107269017.png"   #for test
                    data = requests.post(
                            smms_url,
                            files={'smfile':open(pic_origin_url,'rb'),
                            'format':'json'}
                    )
                    pic_new_url = json.loads(data.text)
                    cloud_path = pic_new_url['data']['url']
                    return(cloud_path)
     
            except BaseException as err:
                    print("error in smms\n{}".format(err))
     
    def tx(pic_origin_url):
            '''
            上传至腾讯云
            '''
            try:
                    secret_id = ''          # 替换为用户的 secretId
                    secret_key = ''          # 替换为用户的 secretKey
                    region = ''         # 替换为用户的 Region
                    Bucket = '' #替换为用户的Bucket
                    token = None                                # 使用临时密钥需要传入 Token，默认为空，可不填
                    scheme = 'https'                        # 指定使用 http/https 协议来访问 COS，默认为 https，可不填
                    config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Token=token, Scheme=scheme)
                    client = CosS3Client(config)
     
                    pic_basename = os.path.basename(pic_origin_url)                #获取文件名及后缀
                    time_for_dir = time.strftime("%Y-%m-%d")                #获取当前日期用于命名文件夹
     
                    #默认上传后建立一个以当前日期命名的文件夹，如果不需要，注释掉这一行用下一行。
                    cloud_path = time_for_dir + r'/' + pic_basename               
                    #cloud_path = pic_basename
                    key = cloud_path
            # 2. 获取客户端对象
                    with open(pic_origin_url, 'rb') as fp:
                            response = client.put_object(
                                    Bucket=Bucket,
                                    Body=fp,
                                    Key=key,
                            #StorageClass='STANDARD',
                            #ContentType='text/html;
                            #charset='utf-8'
                            )
                    cloud_path = client._conf.uri(bucket=Bucket, path=key)
                    return cloud_path
            except BaseException as err:
                    print("error in tx\n{}".format(err))
     
    if __name__ == '__main__':
     
            parser = argparse.ArgumentParser()
            subparsers = parser.add_subparsers(help="commands", dest="command")
     
            smms_parser = subparsers.add_parser("smms",help="use smms")
            smms_parser.add_argument("art_tittle",help="input filename")
     
            tx_parser = subparsers.add_parser("tx",help="use tx cos")
            tx_parser.add_argument("art_tittle",help="input filename")
     
            local_parser = subparsers.add_parser("local",help="use local")
            local_parser.add_argument("art_tittle",help="input filename")
     
            args = parser.parse_args()
     
    if args.command == "smms":
            article_path = get_artical_path(args.art_tittle)
            change_pic_path(article_path,'smms')
     
    if args.command == "tx":
            print ('use tx,file name is {}'.format(args.art_tittle))
            article_path = get_artical_path(args.art_tittle)
            change_pic_path(article_path,'tx')

[查看原网页: blog.51cto.com](https://blog.51cto.com/u_11949039/2834780)