GitLab克隆代码时出现Permission denied (publickey,gssapi-keyex,gssapi-with-mic,passwo
2019年11月17日 阅读数：1039
这篇文章主要向大家介绍GitLab克隆代码时出现Permission denied (publickey,gssapi-keyex,gssapi-with-mic,passwo,主要内容包括基础应用、实用技巧、原理机制等方面，希望对大家有所帮助。
标签：
html
git
github
bash
ssh
ide
gitlab
ui
加密
spa
第一种，直接使用Http的连接clone代码

若是使用Http连接仍是出Permission denied或者密码错误等异常状况，能够直接在Http连接中添加密码，以下：

git clone http://xuxiaolong:xxxxxxxx@gitlab.xxx.com
复制代码

第二种，使用SSH的方式clone代码。此方式须要在GitLab上配置ssh key
首先，打开本地.ssh文件夹

cd ~/.ssh
复制代码

查看此文件夹下面是否存在id_rsa和id_rsa.pub两个文件，若是存在，代表以前有生成过ssh key，若是不存在，能够经过如下命令直接生成ssh key

ssh-keygen -t rsa -C 'xxx@xxx.com' (‘’中的参数就是你的邮箱地址)
复制代码

终端输入此命令，一直回车,直到出现下图的结果，表示ssh key生成完成

此时.ssh问价夹下面会出现test_rsa和test_rsa.pub两个文件

打开test_rsa.pub文件，将里面的全部内容复制到GitLab上对应位置

此时，ssh key配置完成。
若是同时使用GitLab和GitHub，那么就须要配置多个ssh key
一、使用以下命令，指定文件名生成一个ssh key

ssh-keygen -t rsa -C 'xxx@xxx.com' -f ~/.ssh/gitlab_rsa
复制代码

此时在.ssh文件夹下面会生成gitlab_rsa和gitlab_rsa.pub两个文件
二、在~/.ssh文件夹下面新建名为config的文件，用来配置多个ssh key

# gitlab
Host gitlab.bitautotech.com
    HostName gitlab.bitautotech.com
    User xuxiaolong3
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitlab_rsa
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa

# 配置文件参数
# Host : Host能够看做是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件
# HostName : 要登陆主机的主机名
# User : 登陆名,不填写的话，默认使用邮箱做为登陆名
# IdentityFile : 指明上面User对应的identityFile路径
复制代码

此时再次去终端进行clone，第一次会让你输入密码，输入完成以后就能够正常clone了。htm