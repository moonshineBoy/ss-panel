# 使用SS-Panel
现在来配置节点，首先使用我们的管理员账号登录到后台，填写节点信息:
![1](https://i.imgur.com/spagsOx.png)
![2](https://i.imgur.com/g4UE8cb.png)
注意节点名称这里，一定要按照如下的格式来填写:
```
美国 VIP节点1 - 10G带宽
香港 普通节点1 - 100M带宽
```

诸如此类的，你可以自行发挥，但总体的格式不能有变化。

现在回到VPS服务器中，安装SSR后端:
```
wget https://github.com/jedisct1/libsodium/releases/download/1.0.16/libsodium-1.0.16.tar.gz
tar xf libsodium-1.0.16.tar.gz && cd libsodium-1.0.16
./configure && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
```

```
cd /root
apt -y install python-setuptools
easy_install pip
git clone -b manyuser https://github.com/glzjin/shadowsocks.git
cd shadowsocks
pip install -r requirements.txt
cp apiconfig.py userapiconfig.py
cp config.json user-config.json
```
以上命令一个个直接复制粘贴就行。

关闭 Ubuntu18.04 的防火墙:
```
ufw disable
```
编辑后端配置文件，填写你的节点对应ID和数据库信息:
```
vi userapiconfig.py
```
按如图填写:
![3](https://i.imgur.com/ku33Ayn.png)

节点ID在哪里？如图:
![4](https://i.imgur.com/BqJe9Gc.png)

确定都填写正确后，用调试模式先启动后端:
```
python server.py
```
看到如下图能够回显用户的连接信息就说明配置正常:
![5](https://i.imgur.com/Syu0NqO.png)
否则有问题，就自己根据报错信息来找原因。

确定没问题后，`Ctrl+C` 退出来，输入如下命令将程序放到后台运行:
```
./run.sh
```
此时我们回到面板的节点列表这里，可以看到节点是在线状态:
![6](https://i.imgur.com/NWf3MjX.png)
使用肯定也是没有问题的。至此，这个面板就基本算是调教完毕了。有关此面板的更多设置和使用方法请自行研究。



<a href="https://www.vultr.com/?ref=7295225"><img src="https://www.vultr.com/media/banner_1.png" width="100%" height="90"></a>