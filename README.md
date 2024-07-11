# frp0.5版本以上内网穿透教程
>关于frp0.5版本以上使用.toml文件进行内网穿透得详细配置教程【纯新手保姆教程】。本内网穿透主要讲通过http协议进行访问。
***
## 前期准备【纯新手】：
* 需要一台虚拟服务器【腾讯云、阿里云均可】，我在教程中使用的是vultr。
* 需要申请对应的域名，并把域名解析到服务器，我在教程中使用的是namesilo。	
* 一个ssh工具，我在教程中使用的是FinalShell。
* 一个解析到你服务器的宝塔界面【主要用作端口放行】。
***
## 参考链接:
* [frp原作者](https://github.com/fatedier/frp)  
* [frp各个客户端下载](https://github.com/fatedier/frp/releases)
* [frp服务端一键配置脚本](https://github.com/mvscode/frps-onekey "脚本默认获取frp最新版本")
* [frp老版本内网穿透教程](https://www.bilibili.com/video/BV1tL4y1p7qA/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f "借鉴服务端配置教程")
* [frp宝塔端使用设置](https://www.bilibili.com/video/BV1PY4y1F7cb/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f)
***
## 正式步骤：
### 1、先在vultr购买一个服务器【看经济能力任意即可】，购买后会获得服务器的ip和账号还有密码。
* [参考图片](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/1.png)
### 2、在namesilo中购买域名并解析到服务器中。
* [购买域名](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/2.png)
* [购买后选择Domain Manager](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/3.png)
* [点击最右边的蓝色球](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/4.png)
* [点击A创建需要解析的域名地址【根据穿透的重点不同，建议地址也不同】一半解析时间大概要15分钟左右](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/5.png)
* [快到时间可以去chinaz.com中ping一下自己的地址](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/6.png)
### 3、用FinalShell链接服务器后，输入命令一键安装。  
#### Gitee
```Bash
wget https://gitee.com/mvscode/frps-onekey/raw/master/install-frps.sh -O ./install-frps.sh
chmod 700 ./install-frps.sh
./install-frps.sh install
```
#### Github
```Bash
wget https://raw.githubusercontent.com/mvscode/frps-onekey/master/install-frps.sh -O ./install-frps.sh
chmod 700 ./install-frps.sh
./install-frps.sh install
```

### 其他你可能会用到的命令
#### Uninstall（卸载）
```Bash
./install-frps.sh uninstall
```
#### Update（更新）
```Bash
./install-frps.sh update
```
#### Server management（服务管理器）
```Bash
Usage: /etc/init.d/frps {start|stop|restart|status|config|version}
```
#### 开启frps
```Bash
frps start
```
#### 停止frps
```Bash
frps stop
```
#### 重启frps
```Bash
frps restart
```
***
### 注意事项
frps在安装的时候一定要把**bind port(端口)放行，要不然会没有办法进入frp的管理后台**【我这里是用宝塔进入服务器后进行的端口放行】  
frps在配置好后会有包含ip等内容的总结数据，建议把该数据截图留用。

安装好frps后输入:  
>http://[你的服务器ip]:[配置frps的bind port]
***
### 4、去[frp官方路径下载对应的客户端](https://github.com/fatedier/frp/releases)，并解压到本地后开始配置frpc.toml
>*注意：老版本的ini文件后续会逐步被toml替代，官方也不再建议使用ini*。

## frpc.toml的配置文件建议如下【更多配置内容可参考官方的填写建议】：  
```Bash
serverAddr = "127.1.0.1" [此处是你的服务器ip]
serverPort = 7000 [此处是你frps的端口]
auth.method = "token" [此处告诉frpc这里是token]
auth.token = "123445564"[此处填写frps中的token]


[[proxies]]
name = "123"[此处给这段配置取个名，方便后续维护追踪到]
type = "http"[选择服务，我这边以http为例]
localIP = "123.123.123.1"[这里填写你需要穿透设备的内网ip]
localPort = 80[这里填写你需要穿透设备的内网端口号]
customDomains = ["router.something.com"] [这里双括号要留，填写你给穿透设备配置的域名（域名要完成解析）]

你需要多少个[[proxies]]就按照上方标准配置多少个[[proxies]]
```
配置好后保存这个toml文件
### 5、在frp文件目录中，创建一个启动bat
```Bash
@echo off
:home
frpc.exe -c frpc.toml
goto home
```
## 把bat设置成开机自启
* 打开windwos开始菜单，输入 “任务计划程序” 将会自动搜索，接着打开它。
* 点击右侧的 “创建任务”，名称随意填写，安全选项选择 “不管用户是否登录都要运行”，当然你也可以选择 “只在用户登录时运行”。
* 选择 “不管用户是否登录都要运行” 可以让你的电脑在断电自动启动后自动运行 frp，你就可以远程桌面连接电脑了。
* 
