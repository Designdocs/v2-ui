# 新 [x-ui](https://github.com/sprov065/x-ui) 

## 一键安装&升级
```
bash <(curl -Ls https://blog.sprov.xyz/v2-ui.sh)
```

## 手动安装&升级
### 手动安装 xray
无需手动安装 v2ray，v2-ui 自带官方 xray 内核

### 手动安装 v2-ui
https://github.com/sprov065/v2-ui/releases

从该地址中下载最新的 v2-ui-linux.tar.gz 文件，并上传至 VPS 的 /root/ 目录下。若你上传至其它的目录，则将第一行命令的 cd /root/ 改为 cd (实际的目录)，不用包括文件名。
```
cd /root/
mv v2-ui-linux.tar.gz /usr/local/
cd /usr/local/
tar zxvf v2-ui-linux.tar.gz
rm v2-ui-linux.tar.gz -f
cd v2-ui
chmod +x v2-ui bin/xray-v2-ui
cp -f v2-ui.service /etc/systemd/system/
systemctl daemon-reload
systemctl enable v2-ui
systemctl restart v2-ui
 
curl -o /usr/bin/v2-ui -Ls https://raw.githubusercontent.com/sprov065/v2-ui/master/v2-ui.sh
chmod +x /usr/bin/v2-ui
```
安装完毕后，输入 v2-ui 命令，你会看到你想要的。

### 如何手动升级
重复做一遍手动安装的操作即可升级

# 面板其它操作
```
v2-ui                  # 显示管理菜单 (功能更多)
v2-ui start            # 启动 v2-ui 面板
v2-ui stop             # 停止 v2-ui 面板
v2-ui restart          # 重启 v2-ui 面板
v2-ui status           # 查看 v2-ui 状态
v2-ui enable           # 设置 v2-ui 开机自启
v2-ui disable          # 取消 v2-ui 开机自启
v2-ui log              # 查看 v2-ui 日志
v2-ui update           # 更新 v2-ui 面板
v2-ui install          # 安装 v2-ui 面板
v2-ui uninstall        # 卸载 v2-ui 面板
```

## 数据备份与迁移
面板所有数据包括账号信息等都存在 /etc/v2-ui/v2-ui.db 中，只要备份此文件即可。在新服务器安装了面板之后，先关闭面板，再将备份的文件覆盖新安装的，最后启动面板即可。

注意，若配置了面板 ssl 证书，确保新服务器的同样的路径下有相同的证书文件，否则将无法在新服务器启动面板。同样的，若配置了 v2ray 的 tls，并且使用了证书文件配置，也要确保新服务器有证书文件，否则将无法启动 v2ray，若使用证书内容配置，则无需关心。

## 卸载面板
执行以下命令即可完全卸载面板，如果还需要卸载 v2ray，请自行找相关教程。
```
systemctl stop v2-ui
systemctl disable v2-ui
rm /usr/local/v2-ui/ -rf
rm /etc/v2-ui/ -rf
rm /etc/systemd/system/v2-ui.service -f
systemctl daemon-reload
```


## 忘记用户名和密码
使用以下命令重置用户名和密码，默认都为 admin
```
/usr/local/v2-ui/v2-ui resetuser
```
## 面板设置修改错误导致面板无法启动
使用以下命令重置所有面板设置，默认面板端口修改为 65432，其它的也会重置为默认值，注意，这个命令不会重置用户名和密码。
```
/usr/local/v2-ui/v2-ui resetconfig
```

## 面板启动失败
### 出现：‘ascii’ codec can’t encode characters in position 0-6: ordinal not in range(128)
这是因为系统编码不支持中文的缘故，将系统编码设置为 UTF-8 即可，具体请自行搜索方法。


## 怎么让面板的账号 IP 显示为我的域名
 - 将域名解析到你的 VPS 的 IP
 - 使用域名访问面板，如：http://blog.sprov.xyz:65432 ，具体域名和端口号以你的实际域名和端口号为准
 - 如果面板设置里正确配置了域名证书和密钥，那么就使用：https://blog.sprov.xyz:65432 访问面板
>使用 CDN 的同志们注意了，CDN 通常只支持常见的 http 和 https 端口，所以使用 65432 是访问不了的，建议将面板端口设置为 CDN 商家支持的端口，肯定受支持的端口号是 80（http）和 443（https）

## 单端口多用户
设计之初并没有考虑到这个配置方式，目前再修改已经不太方便，所以之后大概率不会支持这个配置方式。

