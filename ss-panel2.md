# 搭建SS-Panel
准备一台VPS，系统选择 Ubuntu 18.04。然后登录你的VPS安装宝塔面板:
```
sudo apt install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
```
![1](https://i.imgur.com/cTrJfuL.png)
![2](https://i.imgur.com/Ro0naAm.png)

使用宝塔安装一个LNMP环境，注意PHP版本选择7.0，其他的默认即可。
![3](https://i.imgur.com/E8E6RCg.png)

环境安装好后，添加一个站点，绑定你的域名:
![4](https://i.imgur.com/ibGfZHG.png)

记住你的这个站点路径，回到VPS服务器中，进入到你的站点目录内:
```
cd /www/wwwroot/你的站点目录
```

下载 ss-panel 程序文件:
```
git clone https://github.com/sphard/ss-panel-v3-mod_UIChanges.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard
```

回到宝塔面板中，点击站点设置，添加伪静态规则:
```
location / {
    try_files $uri $uri/ /index.php$is_args$args;
}
```
![5](https://i.imgur.com/aTXDgxS.png)

接着点击网站目录，将运行目录改为 `/public`，如图:
![6](https://i.imgur.com/WsAxFcz.png)

现在在你的站点根目录下找到 `storage` 目录，点击如图按钮修改权限为 `777`，并把所有者改为 `www` 
![7](https://i.imgur.com/xRwdqcy.png)

现在下载 ss-panel 程序到本地电脑.<br>
项目地址: https://github.com/sphard/ss-panel-v3-mod_UIChanges

![8](https://i.imgur.com/TphdT6F.png)

此时打开宝塔面板内的 `phpmyadmin`，新建一个数据库, 命名为 `sspanel` :
![9](https://i.imgur.com/po7TZYP.png)
![10](https://i.imgur.com/drwUugA.png)

导入我们刚下载到本地的数据库文件，数据库文件的路径是:
```
ss-panel-v3-mod_Uim/sql/glzjin_all.sql
```
![11](https://i.imgur.com/VlPJ3YE.png)

回到宝塔面板中，进入 `config` 目录，编辑目录下的 `.config.php` 文件:
![12](https://i.imgur.com/xKThBSe.png)

填写你的站点名字、域名、随机安全码:
![13](https://i.imgur.com/XjxHfoy.png)

确定填写都是正确后，保存文件。

回到VPS服务器中，并在你的站点根目录内执行下面的命令开始安装依赖:
```
php composer.phar install
```

安装完成后如图所示:
![14](https://i.imgur.com/d56yvip.png)

添加计划任务:
```
crontab -e
```
输入如下内容:
```
30 22 * * * php /www/wwwroot/你的站点目录/xcat sendDiaryMail
*/1 * * * * php /www/wwwroot/你的站点目录/xcat synclogin
*/1 * * * * php /www/wwwroot/你的站点目录/xcat syncvpn
0 0 * * * php -n /www/wwwroot/你的站点目录/xcat dailyjob
*/1 * * * * php /www/wwwroot/你的站点目录/xcat checkjob    
*/1 * * * * php -n /www/wwwroot/你的站点目录/xcat syncnas
```
保存并退出

现在来创建面板的管理员账号:
```
php -n xcat createAdmin
```
一般输入这个命令后会有一些警告和错误信息，这里我们直接无视掉就好了，稍等一会儿就会提示让你输入管理员的邮箱之类的，照着填就行。

管理员账号创建完成后，现在来同步一下用户数据:
```
php xcat syncusers
```

回车即可同步完成

至此，该面板程序就部署完成了，可以打开浏览器输入你的域名看看长什么样子:
![15](https://i.imgur.com/5aYL17K.png)

<a href="https://www.vultr.com/?ref=7295225"><img src="https://www.vultr.com/media/banner_1.png" width="100%" height="90"></a>