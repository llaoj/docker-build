## 介绍
 - 1 这个镜像是用来打包php-cli-swoole的基础镜像
 - 2 预装了 supervisor  php-cli with swoole redis 
 - 3 supervisor 为主进程 管理 子进程
 - 5 supervisor配置文件放置地址:`/etc/supervisor/conf.d/`
 - 6 workdir=`/var/www/html/`,项目文件拷贝到该目录下即可

## 使用说明
在你的项目根目录下创建一个deploy文件夹
目录结构这样的
```
deploy
  conf
    supervisor
      *.conf:需要管理的守护进程,supervisor配置文件,比如队列\定时任务等...
    app
      ...:应用配置文件,比如:.env.testing\.env.master...
  Dockerfile
  start.sh
  ...
```
按照这样的组织方式就可以将代码打包进image

#### 配置文件发布生效说明
为了发布我们的网站和守护进程(supervisor),需要在Dockerfile中将这些文件拷贝进相应的目录,然后前台执行supervisord服务.

`Dockerfile`示例

```
FROM llaoj/php-swoole:tag

WORKDIR /var/www/html
COPY . .
RUN chmod -R 777 /var/www/html

CMD ["./deploy/start.sh"]
```

`start.sh`示例

```
#!/bin/bash

cp $PWD/deploy/conf/supervisor/* /etc/supervisor/conf.d/

...

sleep 30

exec supervisord -n
```

在项目根目录下,执行
```
docker build -f ./deploy/Dockerfile -t tagname .
```
进行打包

### 进程启动用户
默认都是`root`
所以,自己配置守护进程(supervisor)时记得指定启动用户:`user=root`

supervisor进程配置文件,示例
```
[program:abc]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/html/bin/abc start
autostart=true
autorestart=true
user=root
numprocs=1
stdout_events_enabled=true
stderr_events_enabled=true
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
stopsignal=QUIT
```
