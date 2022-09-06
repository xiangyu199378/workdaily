# Build Cleanflight-configuatar

1. remove 
   
sudo apt-get remove nodejs

sudo apt-get remove npm

然后转到/etc/apt/sources.list.d并删除所有节点列表（如果有）。然后做一个

sudo apt-get update

检查主文件夹中是否有任何.npm或.node文件夹，并将其删除。

如果您输入

which node

您可以看到节点的位置。尝试which nodejs和which npm太。


#apt-get 卸载
    sudo apt-get remove --purge npm
    sudo apt-get remove --purge nodejs
    sudo apt-get remove --purge nodejs-legacy
    sudo apt-get autoremove
​
    #手动删除 npm 相关目录
    rm -r /usr/local/bin/npm
    rm -r /usr/local/lib/node-moudels
    find / -name npm
    rm -r /tmp/npm* 

网上找了下，有说 执行这个的：yarn install --force，又说用 npm install 的，等，试过没作用。

sudo npm install -g npm@6.13.4
已解决。推出并关闭虚拟机，重启电脑，成功！虽然还是不知道为什么。本意是 5 天没关机了，重启下顺便等人回复，结果意外运行 yarn install --no-bin-links 成功了，有个警告不知道影不影响


