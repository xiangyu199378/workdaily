<font color='red'> #include "Monday , March 29 ,2022 yingguang work summary "  </font>

在实际使用ubuntu时候，经常要碰到需要查看文件以及文件夹大小的情况。

有时候，自己创建压缩文件，可以使用 ls -hl

查看文件大小。参数-h 表示Human-Readable，使用GB,MB等易读的格式方式显示。

klein@klein-ubuntu:~/source$ ll -h
总用量 18G
drwxrwxr-x  3 klein klein 4.0K 7月   6 19:42 ./
drwxr-xr-x 33 klein klein 4.0K 7月   6 18:09 ../
drwxrwxr-x  3 klein klein 4.0K 6月  11 10:49 j01/
-rw-rw-r--  1 klein klein  18G 7月   6 19:25 j01.tar.bz
klein@klein-ubuntu:~/source$ 

对于文件夹的大小，ll -h 显示只有4k。

那么如何来查看文件夹的大小呢？

使用du命令查看文件或文件夹的磁盘使用空间

--max-depth 用于指定深入目录的层数。

 

如要查看当前目录已经使用总大小及当前目录下一级文件或文件夹各自使用的总空间大小，

输入du -h --max-depth=1即可。

klein@klein-ubuntu:~/source$ du -h --max-depth=1
44G    ./j01
62G    .
klein@klein-ubuntu:~/source$


如要查看当前目录已使用总大小可输入：du -h --max-depth=0

klein@klein-ubuntu:~/source$ du -h --max-depth=0
62G    .
klein@klein-ubuntu:~/source$

