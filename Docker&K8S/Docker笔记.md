# Docker笔记



## Docker基础命令

dockerfile



```shell
docker ps -a  #列出所有的正在运行的容器
docker pull 镜像名 #拉去镜像
dokcer run 镜像名  #运行容器，如果没有就先拉取
docker run -it ubuntu #以交互模式运行ubuntu
docker start -i id #以交互模式启动容器
```





## Linux命令

包管理

```shell
#ubuntu
apt install 包名;  #使用apt包管理工具安装软件
apt update #更新可用软件包列表
apt remove 包名 #卸载软件
```



常用命令

```shell
pwd  #print work directory
ls  #list 列出文件
cd  #change directory
mkdir #make directory 创建目录
MV #move 移动文件，或者重命名文件
rm -rf #remove 删除文件  -rf强制且递归删除
cat  #连接多个文件并打印到标准输出。
more #查看文件内容，建议大文件使用more，向下查看
less #查看文件内容，可以向上查看
head #查看头几行文件内容
tail #查看文件末尾的内容

>  #重定向符，会覆盖原来的文件
>> #重定向到文件，会在文件后面写入
ls -l /etc > files.txt  #将显示在屏幕上的/etc目录下面的文件重定向到files.txt中

grep #在文件中搜索文字
grep hello file1.txt #在file1.txt中搜索hello
grep -i -r hello .#在当前目录的所有文件中搜索hello


find #搜索文件和文件夹
find -type d #查找当前目录下的文件夹
find -type f #查找当前目录下的文件
find -type f -name hello.txt #查找当前目录下类型是文件，名字是hello.txt的文件
find / -type f -name "*.py" > py.txt

#一次执行多条命令中间用分号分割
mkdir test;cd test;echo haha #示例，注意当一条命令出现错误后面的命令仍然会执行
mkdir test && cd test && echo haha #注意：当一条命令出现错误后面的命令便不会执行
mkdir test || cd test ||  echo haha #注意：当一条命令执行成功后面的命令便不会执行

#管道，将上一条命令的输出作为下一条命令的输入
ls /bin | less 

#输入多行命令
mkdir hello;\
> cd hello;\
> touch 1.txt;\
> echo hello > 1.txt
```



环境变量

```shell
printenv #打印出环境
echo $PATH #打印出环境变量
export DB_USER=hanhan #设置当前会话的变量


source .bashrc #重新加载.文件
```





