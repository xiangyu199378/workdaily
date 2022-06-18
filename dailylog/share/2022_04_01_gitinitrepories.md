- git服务器空仓库
```
mkdir example.git
cd example.git
git init --bare
git clone git@ip:example.git
```

1、输入命令mkdir  example.git 创建仓库目录
2、输入命令 cd  example.git进入example.git目录下
3、输入命令 git init --bare  初始化仓库
4、初始化完毕，输入ls -l命令可看到生成了git所需要的必要目录
5、在客户端上启动git bash工具
6、在打开的git Bash终端上执行克隆命令 git clone git@IP地址:仓库名字，按下回车
7、在test目录下克隆example仓库成功，说明git服务器上创建的空仓库可以使用了。到此在git服务器上创建空仓库的方法就介绍完了。