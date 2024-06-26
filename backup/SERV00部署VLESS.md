#     一  注册SERV00
  注册地址：[https://www.serv00.com/](https://www.serv00.com/)
  *会有IP纯净度的检测


#    二  注册Cloudns域名
  注册地址：[https://www.cloudns.net/](https://www.cloudns.net/)
  *会有IP纯净度的检测


#    三  将域名托管至SERV00

1. 打开Cloudns控制面板，删除Cloudns中域名所有的DNS记录
2. 打开SERV00的DevilWEB 网络面板（在邮件里面找面板的链接，用账号密码登录）
DNS zones->Add new zone，填入注册的域名，并获取SERV00的DNS记录
3. 将获取的SERV00的DNS记录再填回Cloudns中


#    四  SERV00设置

1. 添加端口  Port reservation->Add port->Random
2. 添加域名  WWW websites->add new website->复制Cloudns中申请到的域名->Advanced settings->将Webiste type更改位Proxy->Proxy port中选择上一步添加的端口
3. 开启应用权限  Additional services->Status->Enabled


#    五  搭建VLESS

- ####  SSH连接至SERV00

- ####  安装Vless

1. 进入项目操作目录，以下命令行中`<Your-domain>`换成你的域名

```shell
cd ~/domains/<Your-domain>
``` 

2. 克隆SERV00项目

```shell
git clone https://github.com/qwer-search/serv00-vless && mv -f serv00-vless vless && cd vless && rm -f README.md
``` 

3. 修改VLESS配置

```shell
nano app.js
``` 
需要修改的项目：const uuid和const port，修改完成后Ctrl+X保存退出

- ####  安装PM2

**PM2**是一款非常优秀的node.js进程管理工具。可以通SSH用下面的指令一键安装。

```
bash <(curl -s https://raw.githubusercontent.com/k0baya/alist_repl/main/serv00/install-pm2.sh)
``` 

- ####  安装依赖

```shell
npm install
``` 

- ####  使用PM2启动并守护vless进程

```shell
pm2 start app.js --name vless
``` 


- ####  接着去你的代理客户端软件中手动添加vless配置即可：

| Key | Value |
|--------|--------|
| 地址 | Panel 中 WWW Websites 选项卡里的你的 Domain name |
| 端口 | 你放行的端口 |
| 用户 ID | 你修改的UUID |
| 传输协议 | ws |
| 伪装域名 | 同地址 |
| ws path | / | 

也可以复制以下vless节点信息，修改`<域名地址>`为Panel 中 WWW Websites 选项卡里的你的 Domain name，`<端口>`为你放行的端口即可

```shell
vless://37a0bd7c-8b9f-4693-8916-bd1e2da0a817@<域名地址>:<端口>?flow=&security=none&encryption=none&type=ws&host=<域名地址>&path=/&sni=&fp=&pbk=&sid=SERV00-VLESS
``` 

#    六  自动化

SERV00每三个月内必须要有一次登录面板或者SSH连接，不然会删号，也可以通过一个脚本解决问题

- ####  自动续期脚本制作

1. 新建`opt`目录，并进入目录

```shell
mkdir ~/opt
``` 

2. 创建自动续期脚本，把其中的`密码`、`用户名`、SSH的`地址`修改为你自己的。

```shell
cat > auto-renew.sh << EOF
#!/bin/bash

while true; do
  sshpass -p '密码' ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -tt 用户名@地址 "exit" &
  sleep 259200  #30天为259200秒
done
EOF
``` 

3. 给 auto-renew.sh添加可执行权限：

```shell
chmod +x auto-renew.sh
``` 

4. 启动保号脚本：

```shell
./auto-renew.sh
``` 

- ####  保存PM2快照

```shell
pm2 save
``` 

- ####  设置自动启动任务

在SERV00的管理页面上找到Cron jobs选项卡，使用Add cron job功能添加任务，Specify time选择After reboot，即为重启后运行。Form type选择Advanced，Command写：

1. 

```shell
/home/用户名/.npm-global/bin/pm2 resurrect
``` 

2. 

```shell
/home/用户名/opt/.auto-renew.sh
```

这样每次SERV00不定时重启任务时，都能自动调用PM2读取保存的任务列表快照，恢复任务列表。如果在保存了任务列表快照后又改变了任务PM2的任务列表，需要重新执行保存快照以更新任务列表。


#    *七  PM2可视化页面监控进程

1. 进入[https://app.pm2.io](https://app.pm2.io)PM2账号并创建新的Bucket：VLESS
4. 根据web页面提示复制连接服务器命令在SSH中链接服务器，并输入你的注册信息
5. 在SSH中开启页面监控

```shell
pm2 monitor
``` 


————————————————
版权声明：本文为「一休's Blog」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://yixiu.icu/posts/gratis/freevpsandvless/#2%e5%ae%89%e8%a3%85pm2


根据以上原文整理了顺序
附YOUTUBE视频链接：[https://www.youtube.com/watch?v=lXQxEh8Awaw&t=1s](https://www.youtube.com/watch?v=lXQxEh8Awaw&t=1s)