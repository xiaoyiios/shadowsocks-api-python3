Shadowsocks-mod
===========

UIM 配套的后端程序

### 关于 Python2
dbq, Python2 实在是太难支持了呜呜呜，请 Python2 用户使用分支 `py2` 克隆代码。

*猫猫注：0202年了，该换掉Python2了*

### Python2 Install
Debian / Ubuntu:
    
    apt update && apt install python-pip libffi-dev libssl-dev git
    git clone -b py2 https://github.com/Anankke/shadowsocks-mod.git
    cd shadowsocks-mod
	pip install -r requirements.txt

CentOS:

    yum install epel-release
    yum update
    yum install libffi libffi-devel openssl-devel python2-pip
    git clone -b py2 https://github.com/Anankke/shadowsocks-mod.git
    cd shadowsocks-mod
    pip install -r requirements.txt
    
### Python3 TurnKey Install
请参见 https://wiki.sspanel.host/#/turnkey-install-for-node

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



-------------------------------
建议在Debian9/10的环境下安装后端
### 安装
安装libsodium
```
wget -N --no-check-certificate https://raw.githubusercontent.com/aipeach/doubi/master/libsodium.sh && chmod +x libsodium.sh && bash libsodium.sh
```
Debian / Ubuntu:
```
apt update && apt install -y python3-pip libffi-dev libssl-dev git nano
```

下载后端源码
```
git clone -b manyuser https://github.com/Anankke/shadowsocks-mod.git /root/shadowsockscd shadowsocks
```

安装依赖
```
pip3 install -r requirements.txt
```

### 使用

创建配置文件
```
cp apiconfig.py userapiconfig.pycp config.json user-config.json
```

修改配置
```
nano userapiconfig.py
```

运行后端

调试运行
```
python3 server.py
```

后台运行
```
./run.sh
```

###  配置 systemd
```
cp -r /root/shadowsocks/ssr.service /etc/systemd/system/
```
如需启动多个后端只需复制一份 ssr.service 改成不同文件名并修改 WorkingDirectory= 即可

多开后端
如下例
```
cp -r /root/shadowsocks /root/aipeachcp -r /root/shadowsocks/ssr.service /etc/systemd/system/aipeach.service
```

启动程序并加入开机自启动
```
systemctl start ssrsystemctl enable ssrecho "sshd: ALL" > /etc/hosts.allow#防止 auto block 了自己无法连接 ssh
```

添加后端DNS解锁流媒体
```
echo -e "8.8.8.8\n" > /root/shadowsocks/dns.conf
```
填写DNS解锁的服务器IP即可解锁流媒体

### userapiconfig.py各项配置信息

```
# Config
#面板对应的节点id
NODE_ID = 0
# hour,set 0 to disable
#测速默认6小时
SPEEDTEST = 6
#自动上报与下载封禁IP
CLOUDSAFE = 1
#自动封禁SS密码和加密方式错误的 IP
ANTISSATTACK = 0
#是否接受上级下发的命令
AUTOEXEC = 0

#单端口多用户混淆参数后缀，可以随意修改，但请保持前后端一致
MU_SUFFIX = 'zhaoj.in'
#单端口多用户混淆参数表达式
MU_REGEX = '%5m%id.%suffix'

SERVER_PUB_ADDR = '127.0.0.1'  # mujson_mgr need this to generate ssr link
#对接方式，glzjinmod (数据库方式连接)，modwebapi (webapi)
API_INTERFACE = 'modwebapi'  # glzjinmod, modwebapi
# 面板站点地址，如果开启了 https 需要修改为https
WEBAPI_URL = 'https://zhaoj.in'
# .config.php里设置的对接口令
WEBAPI_TOKEN = 'glzjin'

# mudb
MUDB_FILE = 'mudb.json'

# Mysql
MYSQL_HOST = '127.0.0.1'
MYSQL_PORT = 3306
MYSQL_USER = 'ss'
MYSQL_PASS = 'ss'
MYSQL_DB = 'shadowsocks'

MYSQL_SSL_ENABLE = 0
MYSQL_SSL_CA = ''
MYSQL_SSL_CERT = ''
MYSQL_SSL_KEY = ''

# API
API_HOST = '127.0.0.1'
API_PORT = 80
API_PATH = '/mu/v2/'
API_TOKEN = 'abcdef'
API_UPDATE_TIME = 60

# Manager (ignore this)
MANAGE_PASS = 'ss233333333'
# if you want manage in other server you should set this value to global ip
MANAGE_BIND_IP = '127.0.0.1'
# make sure this port is idle
MANAGE_PORT = 23333
```
