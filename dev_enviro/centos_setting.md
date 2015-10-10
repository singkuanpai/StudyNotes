
## centos 服务器部署

[TOC]

### 系统配置

#### View System Info

> (1).查看硬盘：df -l (byte)， df -lh (G)，fdisk -l
> (2).查看内存：free  -m ,free -g ，more /proc/meminfo
> (3).查看系统pci设备：lspci；
> (4).查看CPU信息：more /proc/cpuinfo；
> (5).查看网卡信息：ifconfig, 网卡配置cd /etc/sysconfig/network-scripts/
> (6).查看网络连接：ip a
> (7).查看yum源：cd /etc/yum.repos.d
> (8).查看用户: cat /etc/group ,cat /etc/passwd |cut -f 1 -d ,/etc/passwd

#### Network Config

```
cd /etc/sysconfig/network-scripts/ 
cp ifcfg-eth0{,.bak}   #.bak 为cp为 ifcfg-eth0

DEVICE=eth0     #接口名（设备,网卡）
BOOTPROTO= static   #IP的配置方法（static:固定IP， dhcpHCP， none:手动） 
HWADDR=08:00:27:A6:21:8F    #MAC地址 
ONBOOT=yes                     # 系统启动的时候网络接口是否有效（yes/no） 
IPADDR=192.168.1.2             #IP地址
NETMASK=255.255.255.0          #子网掩码 
GATEWAY=192.168.1.1            #网关地址 
IPV6INIT=no                    #IPV6是否有效（yes/no） 
TYPE=Ethemet                   #网络类型（通常是Ethemet） 

修改示例：
vim /etc/sysconfig/network-scripts/ifcfg-eth0 编辑配置以太网
DEVICE="eth0"
NM_CONTROLLED="yes"
ONBOOT="yes"
BOOTPROTO=none(不自动获取IP！想自动获取的话就等于DHCP)
IPADDR=192.168.0.1
NAME="System eth0"
DEFROUTE=yes
NETMASK=255.255.255.0
TYPE=Ethernet
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
UUID=5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03
HWADDR=00:0C:29:9D:35:59
PEERDNS=yes
PEERROUTES=yes

a.重新启动网络服务： service network restart 或者 /etc/init.d/network restart  或者 重启系统：reboot


b.centos中无法配置自动获取ip问题解决，关闭centos中的NetworkManager  进程即可。
ps -ef|grep NetworkManager
kill 1492
ps -ef|grep NetworkManager


c.yum "Couldn't resolve host 'mirrorlist.centos.org'" for CentOS 6

The solution to this was to simply place the 127.0.0.1 entry at the botton of the /etc/resolv.conf file, like so:
nameserver 8.8.8.8
nameserver 8.8.4.4
nameserver 127.0.0.1

d.域名无法ping通： unknow host

/etc/resolv.conf 这个空文件添加：nameserver 127.0.0.1
```

#### 更换repo源

> (1).cd /etc/yum.repos.d
 
> (2).wget http://mirrors.163.com/.help/CentOS6-Base-163.repo 
 
> (3).替换CentOS-Base.repos
 
> (4).yum update 即可。很简单


#### Basic Commands

```
yum -y update
yum -y install patch wget setuptool pip ntsysv expect
yum install gcc gcc-c++ autoconf vixie-cron crontabs system-config-network
 

yum -y install patch gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel png jpeg  gd freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers compat-libstdc++-33 pcre pcre-devel  

yum -y install vsftpd 

```

#### Firewall

```
vi /etc/sysconfig/iptables
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT #允许80端口通过防火墙
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT #允许3306端口通过防火墙
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3690 -j ACCEPT 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT 


-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3690  -j ACCEPT

-A INPUT -p tcp -m state --state NEW -m tcp --dport 11211  -j ACCEPT


#备注：添加到默认的22端口这条规则的下面
/etc/init.d/iptables restart #最后重启防火墙使配置生效
```
#### 关闭SELINUX

```
vi /etc/selinux/config
#SELINUX=enforcing #注释掉
#SELINUXTYPE=targeted #注释掉
SELINUX=disabled #增加
:wq #保存，关闭
shutdown -r now #重启系统
```
systemctl enable iptables.service  开机启动


#### 开机启动

```
chkconfig --list httpd 
chkconfig httpd on #设为开机启动 
chkconfig --levels 235 mysqld on
/etc/init.d/httpd restart #重启Apache service httpd start

systemctl enable 服务名
systemctl enable postfix.service #开机启动
systemctl disable httpd.service #开机不启动
systemctl is-enabled .service #查询服务是否开机启动
ls /etc/systemd/system/multi-user.target.wants/ :systemd查看开机自启动的程序,相当于chkconfig --list
systemctl #查看systemd单元加载及活动情况
systemctl --failed #显示启动失败的单元
systemctl list-unit-files:查看systemd管理的所有单元
```

### SSH
> 设置开机运行：chkconfig sshd on
> 查看ssh是否启动：chkconfig --list sshd

```
修改vi /etc/ssh/sshd_config，根据模板将要修改的参数注释去掉并修改参数值：
#Protocol 2,1　← 找到此行将行头“#”删除，再将行末的“,1”删除，只允许SSH2方式的连接
Protocol 2　← 修改后变为此状态，仅使用SSH2

#ServerKeyBits 768　← 找到这一行，将行首的“#”去掉，并将768改为1024
ServerKeyBits 1024　← 修改后变为此状态，将ServerKey强度改为1024比特

#PermitRootLogin yes 　← 找到这一行，将行首的“#”去掉，并将yes改为no
PermitRootLogin no 　← 修改后变为此状态，不允许用root进行登录

#PasswordAuthentication yes　← 找到这一行，将yes改为no
PasswordAuthentication no　← 修改后变为此状态，不允许密码方式的登录

#PermitEmptyPasswords no　 ← 找到此行将行头的“#”删除，不允许空密码登录
PermitEmptyPasswords no　 ← 修改后变为此状态，禁止空密码进行登录
```

### mysql & php & apache
```
yum -y install mysql mysql-server mysql-devel mariadb mariadb-server php php-devel  php-mysql php-cgi  php-gd php-fastcgi  libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt libmcrypt-devel mcrypt mhash libiconv libevent   libevent-devel httpd httpd-devel subversion mod_dav_svn mod_auth_mysql  python python-devel
```  

#### mysql 配置 my.cnf


从最新版本的linux系统开始，默认的是 Mariadb而不是mysql！

使用系统自带的repos安装很简单：

yum install mariadb mariadb-server

systemctl start mariadb ==> 启动mariadb
systemctl enable mariadb ==> 开机自启动
mysql_secure_installation ==> 设置 root密码等相关
mysql -uroot -p123456 ==> 测试登录！


```
为root账户设置密码
mysql_secure_installation
回车，根据提示输入Y
输入2次密码，回车
根据提示一路输入Y
最后出现：Thanks for using MySQL!

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
default-character-set=utf8
[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
[mysql]
default-character-set=utf8
```

#### php 配置 php.ini
```
date.timezone = PRC #在946行 把前面的分号去掉，改为date.timezone = PRC
expose_php = Off #在432行 禁止显示php版本的信息
magic_quotes_gpc = On #在745行 打开magic_quotes_gpc来防止SQL注入
short_open_tag = ON #在229行支持php短标签
```

#### apache 配置 httpd.conf

```
ServerTokens OS　 在44行 修改为：ServerTokens Prod （在出现错误页的时候不显示服务器操作系统的名称）
ServerSignature On　 在536行 修改为：ServerSignature Off （在错误页中不显示Apache的版本）
Options Indexes FollowSymLinks　 在331行 修改为：Options Includes ExecCGI FollowSymLinks（允许服务器执行CGI及SSI，禁止列出目录）
#AddHandler cgi-script .cgi　在796行 修改为：AddHandler cgi-script .cgi .pl （允许扩展名为.pl的CGI脚本运行）
AllowOverride None　 在338行 修改为：AllowOverride All （允许.htaccess）
Options Indexes MultiViews FollowSymLinks 在554行 修改为 Options MultiViews FollowSymLinks（不在浏览器上显示树状目录结构）
DirectoryIndex index.html index.html.var 在402行 修改为：DirectoryIndex index.html index.htm Default.html Default.htm
index.php Default.php index.html.var （设置默认首页文件，增加index.php）
KeepAlive Off 在76行 修改为：KeepAlive On （允许程序性联机）
MaxKeepAliveRequests 100 在83行 修改为：MaxKeepAliveRequests 1000 （增加同时连接数）

找到AddType application/x-gzip .gz .tgz在其下加以下内容
> AddType application/x-httpd-php .php
> AddType application/x-httpd-php-source .phps
```

#### Test

```
echo phpinfo();
<?php
$link = mysql_connect('localhost', 'mysql_user', 'mysql_password');
if (!$link) {
    die('Could not connect: ' . mysql_error());
}
echo 'Connected successfully';
mysql_close($link);
?> 
```

### python

#### Basic Install
> yum install gcc libffi-devel python python-devel python-pip  python-setuptools  openssl-devel   openblas-devel blas-devel lapack-devel


#### Packages
> pip list
> pip install  distribute BeautifulSoup4 chardet Cython lxml MySQL-python numpy pdfminer scipy simplejson six threadpool xlrd xlwt pyparsing matplotlib Scrapy queuelib Twisted 

### memcache

#### Install
```
yum install libmemcached memcached
chkconfig --add memcached  

wget -c http://pecl.php.net/get/memcache-3.0.8.tgz
tar -zxvf memcache-3.0.8.tgz
cd memcache-3.0.8
  # whereis phpize 由于是yum安装的php，可以使用find / -name php-config 查找php-config文件的位置
/usr/bin/phpize
./configure  --with-php-config=/usr/bin/php-config --enable-memcache --with-zlib-dir 
make
make install

vim /etc/php.ini
extension_dir ="/usr/lib64/php/modules/"    （这里的路径就是上面make install完后显示的路径）
extension=memcache.so
```


#### 启动memcached时候
```
 	cd /usr/local/bin //进入到该目录
  ./memcached -d -m 900 -u root -l 192.168.100.186 -p 11211 -c 256 -P /tmp/memcached.pid //启动memcached 启动参数说明： 

   ./memcached -d -m 900 -u root  -p 11211 -c 256 -P /tmp/memcached.pid

   启动参数说明：
   -d   选项是启动一个守护进程，
   -m  是分配给Memcache使用的内存数量，单位是MB，默认64MB

   -M  return error on memory exhausted (rather than removing items)
   -u  是运行Memcache的用户，如果当前为root 的话，需要使用此参数指定用户。
   -l   是监听的服务器IP地址，默认为所有网卡。
   -p  是设置Memcache的TCP监听的端口，最好是1024以上的端口
   -c  选项是最大运行的并发连接数，默认是1024
   -P  是设置保存Memcache的pid文件

   -f   factor  chunk size growth factor (default: 1.25)

   -I   Override the size of each slab page. Adjusts max item size(1.4.2版本新增)

  也可以启动多个守护进程，但是端口不能重复 
   
   停止Memcache进程：
   kill `cat /tmp/memcached.pid`
 ```

#### Test

> php -m
> ps -ef|grep memcached
> netstat -tln | grep 11211
> echo stats | nc IP 11211

```
<?php  
  
$mem = new Memcache;  
  
$mem->addServer("localhost",11211);  
  
echo "Version:".$mem->getVersion();  
  
  
class testClass{  
  
        private $str_attr;  
        private $int_attr;  
  
        public function __construct($str,$int)  
        {  
                $this->str_attr = $str;  
                $this->int_attr = $int;  
        }  
  
        public function set_str($str)  
        {  
                $this->str_attr = $str;  
        }  
        public function set_int($int)  
        {  
                $this->int_attr = $int;  
        }  
  
        public function get_str()  
        {  
                return $this->str_attr;  
        }  
        public function get_int()  
        {  
                return $this->int_attr;  
        }  
}  
  
$test = new testClass("hell world",1123);  
$mem->set("key",$test,false,1000);  
  
var_dump($mem->get("key"));  
  
$test_array = array("index_1"=>"hello","index_2"=>"world");  
  
$mem->set("array_test",$test_array,false);  
  
var_dump($mem->get("array_test"));  
  
?>  
```

### mcrypt扩展

yum install php-gd
yum install php-mcrypt
yum install php-mbstring

yum install libmcrypt libmcrypt-devel mcrypt mhash
安装php的mcrypt扩展(动态加载编译)
下载php下的mcrypt扩展或者直接下载php的完整安装包
http://cn.php.net/releases/ 网页下找到自己服务器的php版本，下载后tar解压
进入ext/mcrypt文件夹
phpize
 ./configure --with-php-config=/usr/bin/php-config
make && make install
给你的php.ini添加一条extension=mcrypt.so


注意mcrypt是PHP自带的Pecl扩展，所以只要去PHP的解压缩目录去找mcrypt包即可。
1.动态加载
使用php的常见问题是：编译php时忘记添加某扩展，后来想添加扩展，但是因为安装php后又装了一些东西如PEAR等，不想重装整个PHP，于是可以采用动态编译，使用phpize。需要注意的是要有与现有php完全相同的php压缩包。
```
#cd /usr/php-5.4.8/ext/mcrypt
#/usr/local/webserver/php/bin/phpize
#./configure --with-php-config=/usr/local/webserver/php/bin/php-config
#make && make install
```
给你的php.ini添加一条extension=mcrypt.so
重启apache
```
# /usr/local/apache2/bin/apachectl restart
```
查看phpinfo(),mcrypt以及安装好


### SSL
yum install mod_ssl

使用openssl手动创建证书
1、安装openssl

yum install openssl

2、生成服务器私钥

cd /etc/pki/tls
openssl genrsa -out server.key 1024

注意：server.key是私钥。

3、用私钥server.key文件生成证书请求文件csr

openssl req -new -key server.key -out server.csr

注：server.csr是证书请求文件。

此步骤需要输入一些证书信息：
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:beijing
Locality Name (eg, city) [Default City]:beijing
Organization Name (eg, company) [Default Company Ltd]:hongxin
Organizational Unit Name (eg, section) []:yansheng
Common Name (eg, your name or your server's hostname) []:www.example.com
Email Address []:a@a.com

输入国家、省份、城市、公司、部门、姓名或服务器名、电子邮箱，随后会要求输入一个challengepassword（密码），无需输入，后面一律直接回车即可。

4、生成数字签名crt文件（证书文件）

openssl x509 -days 365 -req -in server.csr -signkey server.key -outserver.crt

用私钥签名证书请求文件，证书的申请机构和颁发机构都是自己。

5、编辑apache的ssl配置文件

vim/etc/httpd/conf.d/ssl.conf

/etc/httpd/conf.d/ssl.conf文件配置具体如下：

<VirtualHost _default_:443>

DocumentRoot "/var/www/https"      //设置网页存放目录

ServerName *:443                  //服务器的端口

DirectoryIndex index.html index.html.var  //首页名称

SSLEngine on

SSLCertificateFile /etc/pki/tls/server.crt    //证书

SSLCertificateKeyFile /etc/pki/tls/server.key  //私钥

</VirtualHost> 
6、重启apache
开放443端口
service httpd restart
访问https://ip/，就能看到证书信息了。

由于不是第三方根证书颁发机构颁发的证书，而是自己颁发的证书，所以浏览器会提示安全证书不受信任。

!!!注意：首页index.html 的文件权限为755，否则将会出现如上提示：

Forbidden

Youdon't have permission to access /main.html on this server.

解决方法：修改首页index.html读写权限。

Chmod755  index.html

### SVN

yum install subversion
svnserve --version
需要建立SVN库。
```
#mkdir /opt/svn/repos
#svnadmin create /opt/svn/repos
```
执行上面的命令后，自动在repos下建立多个文件， 分别是conf, db,format,hooks, locks, README.txt。

先设置passwd
```  
[users]
# harry = harryssecret
# sally = sallyssecret
hello=123
```

这样我们就建立了hello用户， 123密码  

再设置权限authz

[/]
hello= rw

意思是hello用户对所有的目录有读写权限，当然也可以限定。
如果是自己用，就直接是读写吧。


最后设定snvserv.conf

anon-access = none # 使非授权用户无法访问
auth-access = write # 使授权用户有写权限
password-db = password
authz-db = authz   # 访问控制文件
realm = /opt/svn/repos # 认证命名空间，subversion会在认证提示里显示，并且作为凭证缓存的关键字。
采用默认配置. 以上语句都必须顶格写, 左侧不能留空格, 否则会出错.

连接

开放svn端口
默认是3690端口，你也可以用别的。已开启的跳过这一步
修改
iptables -I INPUT -p tcp --dport 3690 -j ACCEPT
保存
/etc/rc.d/init.d/iptables save
重启
service iptables restart
查看
/etc/init.d/iptables status

启动svn: svnserve -d -r /home/data/svn/

 killall svnserve    //停止 

ps -ef|grep svn|grep -v grep

如果已经有svn在运行，可以换一个端口运行
svnserve -d -r /opt/svn/repos --listen-port 3391

这样同一台服务器可以运行多个svnserver

好了，启动成功后,就可以使用了。
建议采用TortoiseSVN， 连接地址为: svn://your server address （如果指定端口需要添加端口  :端口号）

连接后可以上传本地的文件，有效的管理你的代码。


安装好的svn服务端，默认是不会开机自启动的，每次开机自己启动会很麻烦，我们可以把它设成开机启动
首先：编写一个启动脚本svn_startup.sh
```
#!/bin/bash
/usr/bin/svnserve -d -r /home/svn/
```
这里的svnserve路径保险起见，最好写绝对路径，因为启动的时候，环境变量也许没加载。
绝对路径怎么查？
which svnserve

创建执行脚本svn.sh(/root路径下)，其内容很简单，如下：
```
#!/bin/bash
/usr/bin/svnserve -d -r /home/data/svn/jubao
```
添加可执行权限:命令行运行#chmod ug+x /root/svn.sh
添加自动运行:打开(vi或gedit)/etc/rc.d/rc.local，在最后添加一行内容如下：/home/data/bashscript/svn_start.sh 保存退出。
检查:重启服务器，使用ps-ef看看svn进程是否启动。

/etc/rc.d/rc.local 要有执行权限， 
``` 	
chmod +x /etc/rc.d/rc.local
```