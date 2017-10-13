# 开发环境常用配置
1. IDEA
- 常用插件：Key Promoter/Markdown Support/Maven Helper/FindBugs/GsonFormat/Free Mybatis 
- 初始启动界面修改，System Setting
- 文件编码修改：Encoding
- 自动编译：Compiler-Build project automatically
2. MySql
Centos：
- 1.安装：
```
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server 
```
- 2.启动：
```
service mysqld restart
```
- 3.初始没有密码，设置密码
```
set password for 'root'@'localhost' =password('password');
```
4.编码配置
- 编辑mysql配置文件为/etc/my.cnf，在文档最后加上编码配置
```mysql
[mysql]
default-character-set =utf8
```
5.远程链接设置
- 把在所有数据库的所有表的所有权限赋值给位于所有IP地址的root用户。
```mysql
grant all privileges on *.* to root@'%'identified by 'password';
```
- 如果是新用户而不是root，则要先新建用户
```
mysql>create user 'username'@'%' identified by 'password';  
```
此时就可以进行远程连接了。
参考：https://www.cnblogs.com/starof/p/4680083.html
3. TOMCAT

4. Shadowsocks
1.安装Shadowsocks客户端
```
yum install python-pip
pip install shadowsocks
```

2. 创建/etc/shadowsocks.json 文件，以下为作者配置，使用正常。网上有更多配置，自行搜索。
```json
{
    "server":"your_server_ip",      #ss服务器IP
    "server_port":your_server_port, #端口
    "local_address": "127.0.0.1",   #本地ip
    "local_port":1080,              #本地端口
    "password":"your_server_passwd",#连接ss密码
    "timeout":300,                  #等待超时
    "method":"aes-256-cfb",             #加密方式
    "fast_open": false,             # true 或 false。
}
```
3.启动命令
在终端输入，这里作者未配置后台启动，如需配置可查看下面参考链接。
```
sudo sslocal -c /etc/shadowsocks.json -d start
```
参考：http://blog.csdn.net/yanzi1225627/article/details/51121507

如果你使用的是Chrome，配合Shadowsocks，我们需要安装SwitchOmega插件，方便访问国内外网站，Help Yourself~ 
5. Hexo
//TODO