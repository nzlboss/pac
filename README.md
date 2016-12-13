[PAC](http://pac.itzmx.com/)
=======
本项目主要介绍如何利用国外VPS搭建多协议代理服务。

GFW 封锁了 HTTP/Socks5 代理，HTTP 代理是关键词过滤，Socks5 代理则是封锁协议。不过某些特殊的低端口并没有这么处理，已知的有 21，25。

20端口已经被封杀，21端口目前会被限速400Kbps，换算后约合50KB/S，建议使用25端口，不限速。

[这里](http://pac.itzmx.com/) 提供了我在 [vultr](http://pac.itzmx.com/abc.pac) 上搭建的公共代理。


搭建代理服务器
==============
在 25 端口搭建 http/https 代理。


Ubuntu（需要一行一行复制安装）:
-------
apt-get -y install squid

curl http://9vg.net/vpn/pac/ubuntu-pac.conf > /etc/squid/squid.conf

mkdir -p /var/cache/squid

chmod -R 777 /var/cache/squid

service squid stop

squid -z

service squid restart





CentOS 6.7 x64（推荐用此系统）:
-------
setenforce 0

ulimit -n 800000

echo "* soft nofile 800000" >> /etc/security/limits.conf

echo "* hard nofile 800000" >> /etc/security/limits.conf

echo "alias net-pf-10 off" >> /etc/modprobe.d/dist.conf

echo "alias ipv6 off" >> /etc/modprobe.d/dist.conf

killall sendmail

/etc/init.d/postfix stop

chkconfig --level 2345 postfix off

chkconfig --level 2345 sendmail off

yum -y install squid wget

wget http://9vg.net/vpn/pac/centos-pac.conf -O /etc/squid/squid.conf

mkdir -p /var/cache/squid

chmod -R 777 /var/cache/squid

squid -z

service squid restart

chkconfig --level 2345 squid on

iptables -t nat -F

iptables -t nat -X

iptables -t nat -P PREROUTING ACCEPT

iptables -t nat -P POSTROUTING ACCEPT

iptables -t nat -P OUTPUT ACCEPT

iptables -t mangle -F

iptables -t mangle -X

iptables -t mangle -P PREROUTING ACCEPT

iptables -t mangle -P INPUT ACCEPT

iptables -t mangle -P FORWARD ACCEPT

iptables -t mangle -P OUTPUT ACCEPT

iptables -t mangle -P POSTROUTING ACCEPT

iptables -F

iptables -X

iptables -P FORWARD ACCEPT

iptables -P INPUT ACCEPT

iptables -P OUTPUT ACCEPT

iptables -t raw -F

iptables -t raw -X

iptables -t raw -P PREROUTING ACCEPT

iptables -t raw -P OUTPUT ACCEPT

service iptables save



设置帐号密码
echo "root:X0VfZDMyzCKQA" >> /etc/squid/passwd （这里是帐号密码：root\root)

装完后记得reboot重启下服务器确保生效。


转载注明出处：http://bbs.itzmx.com/thread-8815-1-1.html


Ubuntu 可以直接使用，centos 现在只需要清理系统防火墙规则即可使用。

捐赠：http://pac.itzmx.com/donate/index.html
