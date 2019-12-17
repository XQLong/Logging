## win10启用网络发现

在网络共享中心的高级共享设置中启用共享。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/16/1576501190212-1576501190567.png)

并保证win10设备（可使用ipconfig指令查看ip）和linux(使用ifconfig指令)设备在同一局域网内，且设备间相互可以ping通，否则请检查防火墙设置或关闭防火墙。

## ubuntu安装samba

检查ubuntu是否安装samba,若安装会在在系统根目录下有etc/samba，直接使用指令
```
sudo service smbd restart
```
重启samba能否成功。若没有安装则使用如下指令安装;

- 更新当前软件
```
sudo apt-get upgrade 
sudo apt-get update 
sudo apt-get dist-upgrade
```
- 安装samba
```
sudo apt-get install samba samba-common
```

- 用户home目录下创建共享文件夹
创建file_share目录，右键属性对其进行共享并勾选其相应的权限。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/16/1576502595872-1576502595880.png)

- 给创建的目录设置权限

```
sudo chmod 777 /home/imhqq/share
```
- 添加用户
```sudo smbpasswd -a imhqq

```


## win10映射网络驱动器

通过ip+文件夹的形式，如“//192.168.1.113/share”

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/17/1576549431197-1576549431340.png)

