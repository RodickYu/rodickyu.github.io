#     一  注册SERV00
  注册地址：[https://www.serv00.com/](https://www.serv00.com/)
  *会有IP纯净度的检测


#    二  注册Cloudns域名
  注册地址：[https://www.cloudns.net/](https://www.cloudns.net/)
  *会有IP纯净度的检测


#    三  将域名托管至SERV00

1. 打开Cloudns控制面板，删除Cloudns中域名所有的DNS记录
2. 打开SERV00的DevilWEB 网络面板（在邮件里面找面板的链接，用账号密码登录）
DNS zones->Add new zone，填入注册的Cloudns域名，并获取到SERV00的DNS记录
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
需要修改的项目：const uuid（建议修改）和const port（为面板Port reservation放行的端口），修改完成后按Ctrl+X，按y选择保存，回车退出
** [UUID生成](https://1024tools.com/uuid)

- ####  安装PM2

**PM2**是一款非常优秀的node.js进程管理工具。可以通SSH用下面的指令一键安装。

```
bash <(curl -s https://raw.githubusercontent.com/k0baya/alist_repl/main/serv00/install-pm2.sh)
``` 

- ####  安装依赖

```shell
npm install
``` 

**在安装完成后建议在SSH窗口中EXIT后重新登录一次以刷新npm安装状态

- ####  使用PM2启动并守护vless进程

1. 重新登录SSH后进入Vless目录

```shell
cd ~/domains/<Your-domain>/vless
``` 

2. PM2启动进程

```shell
pm2 start app.js --name vless
``` 


- ####  至此，VLESS节点已搭建完成，接着去你的代理客户端软件中手动添加vless配置即可：

| Key | Value |
|--------|--------|
| 地址 | Panel 中 WWW Websites 选项卡里的你的 Domain name |
| 端口 | 你放行的端口 |
| 用户 ID | 你修改的UUID |
| 传输协议 | ws |
| 伪装域名 | 同地址 |
| ws path | / | 

也可以复制以下vless节点信息，修改`<域名地址>`为Panel 中 WWW Websites 选项卡里的你的 Domain name，`<端口>`为你放行的端口，然后重新复制修改好自己信息的vless节点信息从剪切板导入

```shell
vless://37a0bd7c-8b9f-4693-8916-bd1e2da0a817@<域名地址>:<端口>?flow=&security=none&encryption=none&type=ws&host=<域名地址>&path=/&sni=&fp=&pbk=&sid=SERV00-VLESS
``` 

#    六  自动化

- ####  保存PM2快照

**确认你vless节点正常可连接后，保存当前pm2快照

```shell
pm2 save
``` 

- ####  设置自动启动任务

在SERV00的管理页面上找到Cron jobs选项卡，使用Add cron job功能添加任务，Specify time选择After reboot，即为重启后运行。Form type选择Advanced，Command写：

```shell
/home/用户名/.npm-global/bin/pm2 resurrect
``` 

这样每次SERV00服务器重启任务时，都能自动调用PM2读取保存的任务列表快照，恢复任务列表。如果在保存了任务列表快照后又改变了任务PM2的任务列表，需要重新执行保存快照以更新任务列表。


#    *七  PM2可视化页面监控进程（非必要程序）

1. 进入[https://app.pm2.io](https://app.pm2.io)PM2账号并创建新的Bucket：VLESS

2. 链接PM2账号

![微信截图_20240703043237](https://github.com/RodickYu/rodickyu.github.io/assets/92588826/145d099c-6a40-4a09-90e8-5f005c1fbc7d)

根据信息在SSH中复制图片中第2条命令进行链接，中间可能需要输入你的PM2监测注册信息（账号，密码等）

3. 在SSH中开启页面监控

```shell
pm2 monitor
``` 


————————————————

## 版权声明：本文为「一休's Blog」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。 原文链接：https://yixiu.icu/posts/gratis/freevpsandvless/#2%e5%ae%89%e8%a3%85pm2


1. 根据以上原文修改整理了顺序以方便实际操作，删除了原文中对搭建VLESS节点无关的SSL证书操作和关于SERV00保号脚本的操作

- 附YOUTUBE视频链接：[https://www.youtube.com/watch?v=lXQxEh8Awaw&t=1s](https://www.youtube.com/watch?v=lXQxEh8Awaw&t=1s)

2.  SERV00保号操作，请参考github项目[serv00-login](https://github.com/yixiu001/serv00-login)

- YOUTUBE视频链接：[https://www.youtube.com/watch?v=ApJXnjjdFK8](https://www.youtube.com/watch?v=ApJXnjjdFK8)
