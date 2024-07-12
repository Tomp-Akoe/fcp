# frp0.5版本以上内网穿透教程
>关于frp0.5版本以上使用.toml文件进行内网穿透得详细配置教程【纯新手保姆教程】。本内网穿透主要讲通过http协议进行访问。
***
## 🤖前期准备【纯新手】：
* 需要一台虚拟服务器【腾讯云、阿里云均可】，我在教程中使用的是vultr。
* 需要申请对应的域名，并把域名解析到服务器，我在教程中使用的是namesilo。	
* 一个ssh工具，我在教程中使用的是FinalShell。
* 一个解析到你服务器的宝塔界面【主要用作端口放行】。
***
## 🔗参考链接:
* [frp原作者](https://github.com/fatedier/frp)  
* [frp各个客户端下载](https://github.com/fatedier/frp/releases)
* [frp服务端一键配置脚本](https://github.com/mvscode/frps-onekey "脚本默认获取frp最新版本")
* [frp老版本内网穿透教程](https://www.bilibili.com/video/BV1tL4y1p7qA/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f "借鉴服务端配置教程")
* [frp宝塔端使用设置](https://www.bilibili.com/video/BV1PY4y1F7cb/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f)
* [frpc在windows端自启动](https://blog.csdn.net/gdali/article/details/108864769#:~:text=%E6%89%93%E5%BC%80%E5%BC%80%E5%A7%8B%E8%8F%9C%E5%8D%95%EF%BC%8C%E8%BE%93%E5%85%A5%20%E2%80%9C%E4%BB%BB%E5%8A%A1%E8%AE%A1%E5%88%92%E7%A8%8B%E5%BA%8F%E2%80%9D%20%E5%B0%86%E4%BC%9A%E8%87%AA%E5%8A%A8%E6%90%9C%E7%B4%A2%EF%BC%8C%E6%8E%A5%E7%9D%80%E6%89%93%E5%BC%80%E5%AE%83%E3%80%82%20%E7%82%B9%E5%87%BB%E5%8F%B3%E4%BE%A7%E7%9A%84%20%E2%80%9C%E5%88%9B%E5%BB%BA%E4%BB%BB%E5%8A%A1%E2%80%9D%EF%BC%8C%E5%90%8D%E7%A7%B0%E9%9A%8F%E6%84%8F%E5%A1%AB%E5%86%99%EF%BC%8C%E5%AE%89%E5%85%A8%E9%80%89%E9%A1%B9%E9%80%89%E6%8B%A9,%E2%80%9C%E4%B8%8D%E7%AE%A1%E7%94%A8%E6%88%B7%E6%98%AF%E5%90%A6%E7%99%BB%E5%BD%95%E9%83%BD%E8%A6%81%E8%BF%90%E8%A1%8C%E2%80%9D%EF%BC%8C%E5%BD%93%E7%84%B6%E4%BD%A0%E4%B9%9F%E5%8F%AF%E4%BB%A5%E9%80%89%E6%8B%A9%20%E2%80%9C%E5%8F%AA%E5%9C%A8%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95%E6%97%B6%E8%BF%90%E8%A1%8C%E2%80%9D%E3%80%82%20%E9%80%89%E6%8B%A9%20%E2%80%9C%E4%B8%8D%E7%AE%A1%E7%94%A8%E6%88%B7%E6%98%AF%E5%90%A6%E7%99%BB%E5%BD%95%E9%83%BD%E8%A6%81%E8%BF%90%E8%A1%8C%E2%80%9D%20%E5%8F%AF%E4%BB%A5%E8%AE%A9%E4%BD%A0%E7%9A%84%E7%94%B5%E8%84%91%E5%9C%A8%E6%96%AD%E7%94%B5%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8%E5%90%8E%E8%87%AA%E5%8A%A8%E8%BF%90%E8%A1%8C%20frp%EF%BC%8C%E4%BD%A0%E5%B0%B1%E5%8F%AF%E4%BB%A5%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E8%BF%9E%E6%8E%A5%E7%94%B5%E8%84%91%E4%BA%86%E3%80%82)
## ⬇️正式步骤：
### 1、先在vultr购买一个服务器【看经济能力任意即可】，购买后会获得服务器的ip和账号还有密码。
* [参考图片](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/1.png)
### 2、在namesilo中购买域名并解析到服务器中。
* [购买域名](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/2.png)
* [购买后选择Domain Manager](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/3.png)
* [点击最右边的蓝色球](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/4.png)
* [点击A创建需要解析的域名地址【根据穿透的重点不同，建议地址也不同】一半解析时间大概要15分钟左右](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/5.png)
* [快到时间可以去chinaz.com中ping一下自己的地址](https://github.com/Tomp-Akoe/fcp/blob/41b5f26b579f82885a871b304bc6de8fe605cec7/photo/6.png)
### 3、用FinalShell连接服务器后，输入命令一键安装。  
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

### 📍其他你可能会用到的命令
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
### ❗注意事项
frps在安装的时候一定要把**bind port(端口)放行，其他配置的端口也要放行，要不然会没有办法进入frp的管理后台**【我这里是用宝塔进入服务器后进行的端口放行】  
frps在配置好后会有包含ip等内容的总结数据，建议把该数据截图留用。

安装好frps后输入:
```Bash
http://[你的服务器ip]:[配置frps的bind port]
```
可以进入你自己的frp控制台。
### 4、去[frp官方路径下载对应的客户端](https://github.com/fatedier/frp/releases)，并解压到本地后开始配置frpc.toml
>*注意：老版本的ini文件后续会逐步被toml替代，官方也不再建议使用ini*。

#### frpc.toml的配置文件建议如下【更多配置内容可参考官方的填写建议】：  
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
#### 把bat设置成开机自启
##### 创建任务
* 打开windwos开始菜单，输入 “任务计划程序” 将会自动搜索，接着打开它。
* 点击右侧的 “创建任务”，名称随意填写，安全选项选择 “不管用户是否登录都要运行”，当然你也可以选择 “只在用户登录时运行”。
* 选择 “不管用户是否登录都要运行” 可以让你的电脑在断电自动启动后自动运行 frp，你就可以远程桌面连接电脑了。
* “使用最高权限运行” 也是可选的，根据个人需要可以选上。
* 最后勾选 “隐藏”，就不会在启动时弹出命令行窗口了。
##### 设置触发器
* 转到 “触发器” 页，点击新建，选择 “启动时”。
##### 设置操作页
* 转到 “操作” 页，点击新建，选择 “启动程序”。
* 在程序或脚本一栏选择第一步创建的 start.bat，下面的 “起始于” 填写 start.bat 的路径（不要包含 start.bat）
* 例如你的 start.bat 在 E:\frp\start.bat，那么你只需要在 “起始于” 填写 E:\frp\
***
## 🔅至此，穿透完成。
