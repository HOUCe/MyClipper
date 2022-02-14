# [为网站启用 HTTP Basic Auth | Vigorous Pro](https://www.wevg.org/archives/nginx-http-basic-auth/)

本文将介绍如何为网站启用HTTP基本认证，来保护我们的一些网站页面。

> `.htpasswd` 是用于创建和更新用于存储用户名和密码的文件，以便对 HTTP 用户进行基本身份验证。

也就是说进行用户认证的信息都将会存储在 `.htpasswd` 文件中(使用 Nginx 时，虽可以将文件保存成其他名称，但文件的本质时相同的)。  
创建这个文件可以通过下方的两种方法进行。

## 方式一：通过 openssl 生成

1  

```
echo "username:$(openssl passwd -crypt 'password')" >> /home/.htpasswd
```

> **CRYPT**  
> 仅限 Unix 。使用传统的`Unix crypt(3)`函数，随机生成32位盐（仅使用12位）和密码的前8个字符(即：密码最多仅支持8位，超过八位将仅保留前八位)。不安全。

1  

```
printf "${username}:`openssl passwd -apr1`\n" >> .htpasswd
```

> **MD5**  
> `$ apr1 $` +特定于 Apache 的算法的结果，使用随机32位盐和密码的各种组合的迭代（1,000次）MD5摘要。

## 方式二：使用Apache 工具生成

此方法需要安装 Apache 工具 Debian/Ubuntu 用户需要安装 `apache2-utils` ，RHEL/CentOS/Oracle Linux 系统则需要安装`httpd-tools`

1  
2  
3  
4  
5  

```
//For Debian/Ubuntuapt install apache2-utils//For RHEL/CentOS/Oracle Linuxyum install httpd-tools
```

创建密码文件和第一个用户。

1  

```
htpasswd -c /home/.htpasswd user1
```

通过 `htpasswd -c` 命令来生成 .htpasswd 文件， `/home/.htpasswd` 为用户认证文件路径， `user1` 则为认证用户的用户名，点击回车后将会要求您为用户设置密码。

通过下方的命令来添加新用户，需要确保文件路径相同。

1  

```
htpasswd /home/.htpasswd user2
```

确认该文件中包含用户名和加密密码( 猫一下(￣y▽,￣)╭

1  
2  
3  
4  

```
cat /home/.htpasswduser1:$apr1$/woC1jnP$KAh0SsVn5qeSMjTtn0E9Q0user2:$apr1$QdR8fNLT$vbCEEzDj7LyqCMyNpSoBh/user3:$apr1$Mr5A0e.U$0j39Hp5FfxRkneklXaMrr/
```

正常情况下，不会出现问题。下面我们来配置 Nginx 。

在 conf 文件中加入此配置，即可使用基本身份验证限制对整个网站的访问。

1  
2  

```
auth_basic "Administrator’s Area";auth_basic_user_file /home/.htpasswd;
```

但是如果我想要保持某个部分可以直接访问呢？在这种情况下，通过添加 `auth_basic off` 参数来指定取消继承自上级配置级别的基本身份验证限制：

1  
2  
3  
4  
5  
6  
7  
8  
9  

```
server {    ...    auth_basic           "Administrator’s Area";    auth_basic_user_file conf/htpasswd;    location /public/ {        auth_basic off;    }}
```

此时， public 目录即可直接访问，而其他目录则需要验证。  
也可以反过来，仅对特定目录使用基本身份验证

1  
2  
3  
4  

```
location /api {    auth_basic           “Administrator’s Area”;    auth_basic_user_file /etc/apache2/.htpasswd; }
```

甚至可以更进一步，限制/仅允许特定的ip进行访问

1  
2  
3  
4  
5  
6  
7  

```
location /api {    #...    deny  192.168.1.2;    allow 192.168.1.1/24;    allow 127.0.0.1;    deny  all;}
```

配置完成后，重新加载 Nginx 配置文件

1  

```
nginx -s reload
```

现在我们便成功的为网站启用了 HTTP 基本认证。

![](https://i.yecdn.com/images/2019/05/23/212f54907f1962e6441816628a0a70a2.png)

当用户输入了错误的用户名或密码时，Nginx会返回401 Authorization Required禁止访问。

## 参考文章

-   [Nginx启用HTTP Basic Auth](https://www.iwch.me/archives/658.html)
-   [Create htpasswd file for nginx (without apache)](https://coderwall.com/p/zvvgna/create-htpasswd-file-for-nginx-without-apache)
-   [Password Formats - Apache HTTP Server Version 2.4](https://httpd.apache.org/docs/2.4/misc/password_encryptions.html)
-   [Generate salted password with OpenSSL example](https://socketloop.com/tutorials/generate-salted-password-with-openssl-example)
-   [James Doyle | OpenSSL Passwd Without Prompt](https://ohdoylerules.com/tricks/openssl-passwd-without-prompt/)
-   [How To Set Up Basic HTTP Authentication With Nginx on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-basic-http-authentication-with-nginx-on-ubuntu-14-04)
-   [Restricting Access with HTTP Basic Authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)
