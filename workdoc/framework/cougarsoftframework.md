

# <center>Cougar 软件架构分析
&emsp;&emsp;cougar lite 工程第一级目录如下，其中cougar_mkd是关联的子git库.cougar下还有子github仓库，git clone 最好递归调用.
```
Git命令 --recursive 会递归克隆fast-rcnn项目下面的所有git项目
```
&emsp;&emsp;
<div align=center>
<img src="../images/da20a364daefdec35e77e4dc94c262729d858589e9d75315d292cf3604123453.png"
     alt="这里放图片显示不出的时候出现的文字"
     style="zoom:70%"/>
<center><p>cougar lite文件结构</p></center>
</div>

&emsp;&emsp;工程管理文件为makefile，手写而不是使用automake工具生成。包含子目录.mk文件，编译子仓库git调用sdk_build.sh 。同时usbloader也会调用cougar_mdk目录下的文件。这里的文件夹之间存在相互引用的问题。需要去耦合。




