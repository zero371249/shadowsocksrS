shadowsocks
===========

[![PyPI version]][PyPI]
[![Build Status]][Travis CI]
[![Coverage Status]][Coverage]

A fast tunnel proxy that helps you bypass firewalls.

Server
------

### Install

Debian / Ubuntu:

    apt-get install python-pip
    pip install shadowsocks

CentOS:

    yum install python-setuptools && easy_install pip
    pip install shadowsocks

Windows:

See [Install Server on Windows]

### Usage

    ssserver -p 443 -k password -m aes-256-cfb

To run in the background:

    sudo ssserver -p 443 -k password -m aes-256-cfb --user nobody -d start

To stop:

    sudo ssserver -d stop

To check the log:

    sudo less /var/log/shadowsocks.log

Check all the options via `-h`. You can also use a [Configuration] file
instead.

Client
------

* [Windows] / [OS X]
* [Android] / [iOS]
* [OpenWRT]

Use GUI clients on your local PC/phones. Check the README of your client
for more information.

Documentation
-------------

You can find all the documentation in the [Wiki].

License
-------

Copyright 2015 clowwindy

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations
under the License.

Bugs and Issues
----------------

* [Troubleshooting]
* [Issue Tracker]
* [Mailing list]



[Android]:           https://github.com/shadowsocks/shadowsocks-android
[Build Status]:      https://img.shields.io/travis/shadowsocks/shadowsocks/master.svg?style=flat
[Configuration]:     https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File
[Coverage Status]:   https://jenkins.shadowvpn.org/result/shadowsocks
[Coverage]:          https://jenkins.shadowvpn.org/job/Shadowsocks/ws/PYENV/py34/label/linux/htmlcov/index.html
[Debian sid]:        https://packages.debian.org/unstable/python/shadowsocks
[iOS]:               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help
[Issue Tracker]:     https://github.com/shadowsocks/shadowsocks/issues?state=open
[Install Server on Windows]: https://github.com/shadowsocks/shadowsocks/wiki/Install-Shadowsocks-Server-on-Windows
[Mailing list]:      https://groups.google.com/group/shadowsocks
[OpenWRT]:           https://github.com/shadowsocks/openwrt-shadowsocks
[OS X]:              https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help
[PyPI]:              https://pypi.python.org/pypi/shadowsocks
[PyPI version]:      https://img.shields.io/pypi/v/shadowsocks.svg?style=flat
[Travis CI]:         https://travis-ci.org/shadowsocks/shadowsocks
[Troubleshooting]:   https://github.com/shadowsocks/shadowsocks/wiki/Troubleshooting
[Wiki]:              https://github.com/shadowsocks/shadowsocks/wiki
[Windows]:           https://github.com/shadowsocks/shadowsocks-csharp
准备另外一台服务器：centos7+，cpu和内存最好大于512MB的。



连接服务器。复制以下代码

Bash
cd ~
yum -y groupinstall "Development Tools"
yum -y install wget
wget https://github.com/jedisct1/libsodium/releases/download/1.0.16/libsodium-1.0.16.tar.gz
tar xf libsodium-1.0.16.tar.gz && cd libsodium-1.0.16
./configure && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig && cd ~
git clone https://github.com/miseryCN/shadowsocksr.git
cd shadowsocksr
./initcfg.sh
最后要按下回车键。



服务器执行完代码之后，来到SSRPanel后台管理面板—节点管理—节点列表—右上角添加节点

 

我们输入节点名称，IPv4地址（就是节点服务器的IP地址）。其它的先默认即可！然后点击提交！

 

回到服务器shadowsocksr目录下



Bash
vim usermysql.json


进入之后按键盘英文状态下“i”键修改

Bash
{
    "host": "数据库IP地址",
    "port": 3306,
    "user": "数据库用户名",
    "password": "数据库密码",
    "db": "数据库名",
    "node_id": 面板对应ID,
    "transfer_mul": 1.0,
    "ssl_enable": 0,
    "ssl_ca": "",
    "ssl_cert": "",
    "ssl_key": ""
}
 

修改完成之后先按键盘ESC键，再按shift+冒号键，输入wq，然后回车退出。

 

然后再执行

Bash
python server.py
显示正常能连接到数据库就行了。

 CTRL+C退出，再执行

Bash
./run.sh
./logrun.sh


大约1分钟左右，后端状态就会由离线变成数字！这个时候就说明可以了！

 

然后测试节点是否生效！

 

如果连接上节点，还是打不开谷歌，YouTube的网站。尝试一下关闭防火墙！

Centos7+关闭防火墙



Bash
systemctl stop firewalld.service
systemctl disable firewalld.service


优化一下，SSR后端会莫名其妙的挂掉。所以设置一下定时任务。



服务器开机自启SSR任务！

Bash
cd ~
chmod +x /etc/rc.d/rc.local
vi /etc/rc.d/rc.local
插入以下内容

Bash
bash /root/shadowsocksr/run.sh
bash /root/shadowsocksr/logrun.sh


服务器每天定时5.01自动重启！

Bash
vi /etc/crontab
插入以下内容

Bash
1 05 * * * root reboot


 

最后我们再优化一下速度，使用BBR加速！

Bash
cd ~
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
选择安装以下 BBR plus,安装完成重启服务器！

Bash
./tcp.sh
开启加速即可！
