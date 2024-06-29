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
3. 为DNS解析中A记录的DNS地址添加SSL证书 WWW websites->Manage SSL certificates->选取A记录DNS地址后的Manage->Add certificate->随机生成一个SSL证书
*若生成失败，可到[FreeSSL](https://freessl.cn/)自行申请
4. 开启应用权限  Additional services->Status->Enabled


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