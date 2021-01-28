# MoinMoin 轻量级python wiki系统 

MoinMoin安装在python2下面，我的系统里是用virtualenv的方式启动的，但遗憾的是archlinux里已经找不到uwsgi-plugin-python2的插件了，可以用python插件启动，但是开始的时候要送入一个python virtualenv的路径，并且用python2 virtualenv里的uwsgi来启动。

```ini
[Unit]
Description=uWSGI service unit
After=syslog.target

[Service]
Environment="PATH='/home/moin-1.9.9/virt/bin':${PATH}"
ExecStart=/home/moin-1.9.9/virt/bin/uwsgi --ini /etc/uwsgi/%I.ini

ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/bin/kill -INT $MAINPID
Restart=always
Type=notify
NotifyAccess=all
KillSignal=SIGQUIT

[Install]
WantedBy=multi-user.target
```

```ini
[uwsgi]
uid = http 
gid = http
# daemonize = /var/log/moin000.log
logto = /var/log/moin.log

pidfile = /run/uwsgi/moin.pid
emperor = /etc/uwsgi.d
stats = /run/uwsgi/stats.sock
chmod-socket = 660
emperor-tyrant = true
cap = setgid,setuid

socket = /run/uwsgi/moin.sock
plugins = python

virtualenv = /home/moin-1.9.9/virt

chdir = /home/moin-1.9.9/wiki/server
wsgi-file = moin.wsgi

master

workers = 2
max-requests = 10
harakiri = 60
dir-on-term
```