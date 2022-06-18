
2021,12,03



hi 荧光，

在新电脑来之前，你可以：
1. 注册github 、redmine等账号
2.在虚拟机上模拟搭建一下ubuntu环境 
3.我们VM服务器上只有source insight 和 VScode ，熟悉一下软件；
4.在192.168.20.206/sharefolder/0_project/movidus-开发/ 下面有些开发资料
5.熟悉git 操作
6. 找个模组，熟悉一下常用的Tools （guvcview  、 xslam-view）
7. 在ubuntu 烧录固件： sudo ./yunupdateimg framework.img 
8.向厂库领取 jtag 、 typeC 线


常用git 指令如下

git config user.name 查看当前用户
git config user.email
 
git config --global user.name  "username"   修改用户 
git config --global user.email "email"       
 
git clone git@172.31.1.233:embedded/cougar_lite.git 下载地址
git checkout develop_skyblue
git pull –recurse-submodules  
git pull --recurse-sumbodules
git submodule update --init
 
git branch
 
git log 查看Log信息
 
git branch -D skyblue_rdc200a 删除branch
 
git checkout -- . 将所有修改点删除
 
git mergetool 解决冲突
 
git difftool version1 version2 用compare比较两 个版本的差异
 
git reset --hard c34ecce2..... 退回到之前的某个提 交，最好新建一个临 时branch进行操作
 
        git reset file                                                                      将git add .后变绿  的文件再变红      
                                                      
git checkout -b本地分支名　origin/服务器分支名　　　　　从服务器拉取一个本 　地没有的branch

git pull 更新代码

Hi Dery,

你需要搭建的环境如下：
1.安装ubuntu18系统，和windows10 虚拟机；
2.按照“InitialSetupGuideline.odt”第二章节在ubuntu上安装工具；
3.按照如下方法安装sdk工具；
-To do once :
sudo apt-get install -y software-properties-common
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9DAD6A4F
sudo add-apt-repository -y http://devel
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install -y xslam-sdk
sudo apt-get install -y xslam-viewer
sudo apt-get -f install

-For the next update:
sudo apt-get update
sudo apt-get upgrade
or to avoid updating all the system packages:
sudo apt-get install --only-upgrade xslam-sdk
sudo apt-get install --only-upgrade xslam-viewer
------------

-Run the viewer:
xslam_viewer

-Compile the SDK samples:
cp -r /usr/share/xslamsdk/examples $HOME/xslam-examples
cd $HOME/xslam-examples
mkdir build; cd build
cmake ..
make
./visual_odometry

4.安装vpn工具
5.安装vmrc工具









QUESTION1  cmake plango: 
Could not find a package configuration file provided by "Eigen3" with any
  of the following names:

    Eigen3Config.cmake
    eigen3-config.cmake

SOLVE:
sudo apt-get install libeigen3-dev


QUESTION2  make plango: 
15: fatal error: any: No such file or directory
 #include <any>
              
mount -t cifs -o username=guest,password=P@ssXvisio -l //192.168.20.206 /mnt


ubuntu monut smb 
https://blog.csdn.net/faxiang1230/article/details/95978242


git clone git@@172.31.1.233:embedded/cougar_mdk.git
git clone git@@172.31.1.233:embedded/cougar_lite.git

install 
Gtk-Message: Failed to load module "canberra-gtk-module

sudo apt-get install libcanberra-gtk-module


