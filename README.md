# fcp0.5版本以上内网穿透教程
>关于fcp0.5版本以上使用.toml文件进行内网穿透得详细配置教程【纯新手保姆教程】。本内网穿透主要讲通过http协议进行访问。
***
## 前期准备【纯新手】：
* 需要一台虚拟服务器【腾讯云、阿里云均可】，我在教程中使用的是vultr。
* 需要申请对应的域名，并把域名解析到服务器，我在教程中使用的是namesilo。	
* 一个ssh工具，我在教程中使用的是FinalShell。
* 一个解析到你服务器的宝塔界面【主要用作端口放行】。
***
## 参考链接:
* [frp原作者](https://github.com/fatedier/frp)  
* [fcp各个客户端下载](https://github.com/fatedier/frp/releases)
* [fcp服务端一键配置脚本](https://github.com/mvscode/frps-onekey "脚本默认获取frp最新版本")
* [fcp老版本内网穿透教程](https://www.bilibili.com/video/BV1tL4y1p7qA/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f "借鉴服务端配置教程")
* [fcp宝塔端使用设置](https://www.bilibili.com/video/BV1PY4y1F7cb/?spm_id_from=333.880.my_history.page.click&vd_source=e24c2664016fb1213df78e216a6dd35f)
***
## 正式步骤：
### 1、现在vultr购买一个服务器【看经济能力任意即可】
