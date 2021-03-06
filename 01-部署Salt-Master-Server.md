#Salt-Master：
##1 安装操作系统
##2 配置CentOS 7.X yum源
###1) 导入GPG key
<pre>sudo rpm --import http://mirrors.ustc.edu.cn/centos/RPM-GPG-KEY-CentOS-7</pre>
###2) 修改repo文件添加CentOS 7.X的YUM源
<pre>
cd /etc/yum.repos.d/
vim rhel-source.repo
</pre>
内容如下<br>
<pre>
[base]
name=CentOS-$releasever-Base
baseurl=http://centos.ustc.edu.cn/centos/7/os/x86_64/
gpgcheck=1
gpgkey=http://mirrors.ustc.edu.cn/centos/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever-Updates
baseurl=http://centos.ustc.edu.cn/centos/7/os/x86_64/
gpgcheck=1
gpgkey=http://mirrors.ustc.edu.cn/centos/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-$releasever-Extras
baseurl=http://centos.ustc.edu.cn/centos/7/os/x86_64/
gpgcheck=1
gpgkey=http://mirrors.ustc.edu.cn/centos/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-$releasever-Plus
baseurl=http://centos.ustc.edu.cn/centos/7/os/x86_64/
gpgcheck=1
</pre>
###3) 清除缓存查看是否生效
<pre>
yum clean all
yum makecache
yum repolist
</pre>

###4) 安装yum-plugin-downloadonly插件
<pre>yum install -y yum-plugin-downloadonly
</pre>
##3 获取salt-stack安装包
###1) 创建salt-pkgs目录
<pre>mkdir /salt-pkgs</pre>
###2) 安装salt-latest.repo
<pre>
cd /salt-pkgs
wget https://repo.saltstack.com/yum/redhat/salt-repo-latest-1.el7.noarch.rpm
yum install salt-repo-latest-1.el7.noarch.rpm
</pre>
###3) 修改salt-latest.repo文件
<pre>vim /etc/yum.repo.d/salt-latest.repo</pre>
内容如下<br>
<pre>
[salt-latest]
name=SaltStack Latest Release Channel for RHEL/Centos 7
baseurl=https://repo.saltstack.com/yum/redhat/7/x86_64/latest
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/saltstack-signing-key
</pre>
###6) 创建salt-master目录
<pre>
mkdir -p /salt-pkgs/salt-master
</pre>
###7) 下载salt-master安装包及依赖
<pre>
yum install --downloadonly --downloaddir=/salt-pkgs/salt-master salt-master
</pre>
###8) 创建salt-master目录
<pre>
mkdir -p /salt-pkgs/salt-minion
</pre>
###9) 下载salt-master安装包及依赖
<pre>
yum install --downloadonly --downloaddir=/salt-pkgs/salt-minion salt-minion
</pre>
##4 依赖列表
<pre>
salt-master:
================================================================================
Package                Arch        Version              Repository        Size
================================================================================
Installing:
salt-master            noarch      2016.3.1-1.el7       salt-latest      1.4 M
Installing for dependencies:
PyYAML                 x86_64      3.11-1.el7           salt-latest      160 k
libyaml                x86_64      0.1.4-11.el7_0       base              55 k
python-babel           noarch      0.9.6-8.el7          base             1.4 M
python-crypto          x86_64      2.6.1-1.el7          salt-latest      469 k
python-futures         noarch      3.0.3-1.el7          salt-latest       26 k
python-jinja2          noarch      2.7.2-2.el7          base             515 k
python-markupsafe      x86_64      0.11-10.el7          base              25 k
python-msgpack         x86_64      0.4.6-1.el7          salt-latest       73 k
python-requests        noarch      2.6.0-1.el7_1        base              94 k
python-six             noarch      1.9.0-2.el7          base              29 k
python-tornado         x86_64      4.2.1-1.el7          salt-latest      636 k
python-urllib3         noarch      1.10.2-2.el7_1       base             100 k
python-zmq             x86_64      14.7.0-1.el7         salt-latest      482 k
salt                   noarch      2016.3.1-1.el7       salt-latest      6.1 M
zeromq                 x86_64      4.0.5-4.el7          salt-latest      583 k
Updating for dependencies:
python-chardet         noarch      2.2.1-1.el7_1        base             227 k

Transaction Summary
================================================================================
Install  1 Package  (+15 Dependent packages)
Upgrade             (  1 Dependent package)

salt-minion:
================================================================================
Package                Arch        Version              Repository        Size
================================================================================
Installing:
salt-minion            noarch      2016.3.1-1.el7       salt-latest       30 k
Installing for dependencies:
PyYAML                 x86_64      3.11-1.el7           salt-latest      160 k
libyaml                x86_64      0.1.4-11.el7_0       base              55 k
python-babel           noarch      0.9.6-8.el7          base             1.4 M
python-crypto          x86_64      2.6.1-1.el7          salt-latest      469 k
python-futures         noarch      3.0.3-1.el7          salt-latest       26 k
python-jinja2          noarch      2.7.2-2.el7          base             515 k
python-markupsafe      x86_64      0.11-10.el7          base              25 k
python-msgpack         x86_64      0.4.6-1.el7          salt-latest       73 k
python-requests        noarch      2.6.0-1.el7_1        base              94 k
python-six             noarch      1.9.0-2.el7          base              29 k
python-tornado         x86_64      4.2.1-1.el7          salt-latest      636 k
python-urllib3         noarch      1.10.2-2.el7_1       base             100 k
python-zmq             x86_64      14.7.0-1.el7         salt-latest      482 k
salt                   noarch      2016.3.1-1.el7       salt-latest      6.1 M
zeromq                 x86_64      4.0.5-4.el7          salt-latest      583 k
Updating for dependencies:
python-chardet         noarch      2.2.1-1.el7_1        base             227 k

Transaction Summary
================================================================================
Install  1 Package  (+15 Dependent packages)
Upgrade             (  1 Dependent package)
</pre>
##5 配置yum server
###1) 安装apache
<pre>yum install httpd*</pre>
注：如果提示yum lock，执行rm -f /var/run/yum.pid解决
###2) 编辑apache配置文件
<pre>
cd /etc/httpd/conf
cp httpd.conf httpd.conf.bak
</pre>
###3) 删除测试启动服务
<pre>
rm -f /etc/httpd/conf.d/welcome.conf
</pre>
###4) 拷贝文件到目录或者修改httpd.conf中的DocumentRoot
<pre>
cp -r /salt-pkgs/salt-master /var/www/html
cd /var/www/html
chmod -R 755 salt-master/
</pre>
###5) 安装createrepo
<pre>
yum -y install createrepo
</pre>
###6) 创建repodata
<pre>
createrepo -p -d -o /var/www/html/salt-master/ /var/www/html/salt-master/
createrepo -p -d -o /var/www/html/salt-minion/ /var/www/html/salt-minion/
</pre>
###7) 重启apache服务 并设置服务自启动
<pre>
systemctl restart httpd
systemctl enable httpd
</pre>
###8) 关闭防火墙
<pre>
systemctl stop firewalld
systemctl disable firewalld
</pre>
