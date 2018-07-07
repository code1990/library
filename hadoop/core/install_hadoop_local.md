##hadoop 配置教程

##（一）关闭防火墙

*  service iptables stop 关闭防火墙 
*  chkconfig iptables off 永久关闭防火墙
*  service iptables status 查看防火墙状态
##（二）关闭SELinux

* vim /etc/selinux/config 编辑 SELinux 配置文件
* SELINUX=disabled 改状态

##（三）卸载OpenJDK

* rpm -qa|grep java 检查java安装情况
* rpm -e --nodeps 文件名 卸载openJDK

##（四）修改Hosts
   
* vim /etc/hosts 编辑hosts 
##（五）配置ssh免登陆

* ssh-keygen 生成key: 
* ssh-copy-id B 复制从A复制到B上:
* ssh localhost/exit，ps -e|grep ssh 验证：
* ssh A  #在B中执行
##（六）检查yum 修改yum源
* rpm -qa|grep yum
* cd /etc/yum.repos.d/ 进入目录
* ls -al 列表
* vim CentOS-Base.repo
* baseurl=http://ftp.sjtu.edu.cn/centos/releasever/os/basearch/
##（七）H30时间服务器安装 　配置ntp.conf 　　启动ntp服务
yum install ntp
vim /etc/ntp.conf
server 127.127.1.0
fudge 127.127.1.0 stratum 10

service ntpd start 启动

chkconfig ntpd on 开机启动

service ntpd status 状态
##（八）安装httpd服务
yum install httpd

service httpd start
chkconfig httpd on

##（九）安装createrepo
yum install createrepo

##（十）安装yum-utils
yum install yum-utils
##（十一）将文件copy到/var/www/html/hdp目录,如果没有该目录，创建。
HDP-2.3.0.0-centos6-rpm.tar和HDP-UTILS-1.1.0.20-centos6.tar要拷贝进来
tar zxvf HDP-2.3.0.0-centos6-rpm.tar.gz 

tar zxvf HDP-UTILS-1.1.0.20-centos6.tar.gz


  http://public-repo-1.hortonworks ... -centos6-rpm.tar.gz

  http://public-repo-1.hortonworks ... 0.20-centos6.tar.gz


http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.4.0.0/HDP-2.4.0.0-centos6-rpm.tar.gz

HDP-UTILS
http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.20/repos/centos6/HDP-UTILS-1.1.0.20-centos6.tar.gz 
cd /var/www/html
ls -al
#创建hdp目录
mkdir hdp
tar zxvf HDP-2.3.0.0-centos6-rpm.tar.gz 

tar zxvf HDP-UTILS-1.1.0.20-centos6.tar.gz
##（十二）创建基于html的创建源
createrepo hdp
##（十三）下载Ambari的yum仓库资源描述文件
cd /etc/yum.repos.d/

wget http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.1.1/ambari.repo
##（十四）开启yum的在线安装缓存资源备份
vim /etc/yum.conf 

　　修改成：keepcache=1
##（十五）

##（十六）

##（十七）ssh免密登录key位置
/root/.ssh/id_rsa
baseurl=http://192.168.33.61/hdp/amberi-2.1.1/centos6/
gpgkey=http://192.168.33.61/hdp/amberi-2.1.1/centos6/
##（十八）

##（十九）

##（二十）
#baseurl=http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.1.1
baseurl=http://mini1/ambari/centos6/2.x/updates/2.1.1
gpgcheck=1
#gpgkey=http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
gpgkey=http://mini1/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins

