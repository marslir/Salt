#Salt-Master:
##1 安装salt-ssh
###1) 安装salt-ssh
<pre>yum install salt-ssh
</pre>
###2) 修改/etc/salt/roster文件
<pre>vim /etc/salt/roster内容如下
minion01:
  host: 172.20.10.6
  user: root
  passwd: salttest
  sudo: False
</pre>
###3) 测试salt-ssh
<pre>salt-ssh -i '*' test.ping</pre>
###4) 准备配置文件

###5) 编写sls文件
<pre>mkdir -p /srv/salt/salt-minion/repo
mkdir -p /srv/salt/salt-minion/conf
mkdir -p /srv/salt/salt-minion-install
vim /srv/salt/salt-minion-install/init.sls内容如下
repo-install:
  file.managed:
    - name: /etc/yum.repos.d/httpd.repo
    - source: salt://salt-minion-install/repo/httpd.repo
    - user: root
    - group: root
    - mode: 644
  cmd.run:
    - name: yum clean all;yum makecache
salt-minion-install:
  cmd.run:
    - name: yum install salt-minion
    - require:
      - cmd: repo-install
salt-minion-config:
  file.managed:
    - name: /etc/salt/minion
    - source: salt://minions/conf/minion
    - user: root
    - group: root
    - mode: 640
    - template: jinja
    - defaults:
      minion_id: {{ grains['nodename'] }}
    - require:
      - cmd: salt-minion-install
