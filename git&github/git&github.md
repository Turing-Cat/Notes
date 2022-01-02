# Git

## 1、初始化仓库

```bash
git init
```



## 2、检出仓库

```bash
git clone  本地地址或者远程URL
```



## 3、工作流

工作目录(working dir)       暂存区(stage)      本地仓库(HEAD)

![image-20220102164024423](git&github.assets/image-20220102164024423.png)



## 4、添加和提交

添加到暂存区

```bash
git add <filename>
```

提交到本地仓库,但是此时并没有提交到远程仓库

```bash
git commit -m "代码提交信息"
```



## 5、推送改动

main是默认分支，可换成任意分支

```bash
git push oirgin  main
```

将本地仓库连接到某个远程服务器

```bash
git remote add origin <server>
```



## 6、分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*main* 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

![image-20220102164543344](git&github.assets/image-20220102164543344.png)



- 创建分支

```bash
git checkout -b 分支名称
```

- 切换回主分支

```bash
git checkout main
```

- 把新建的分支删掉

```bash
git branch -d 分支名
```

- 将该分支推送到远程仓库

```bash
git push origin <branch>
```



删除远程仓库

```bash
git remote rm 远程仓库名
```

删除远程仓库分支

```bash
git push origin --delete 分支名
```





## 7、更新和合并

更新本地仓库至最新改动,以在你的工作目录中 *获取（fetch）* 并 *合并（merge）* 远端的改动。

```bash
git pull
```

要合并其他分支到你的当前分支（例如 master)。

```bash
git merge <branch>
```

执行如下命令以将它们标记为合并成功

```bash
git add <filename>
```

在合并改动之前，你可以使用如下命令预览差异：

```bash
git diff <source_branch> <target_branch>
```





## 8、标签

标签是为软件发布创建标签是推荐的。

```bash
git tag 1.0.0 1b2e1d63ff
```

*1b2e1d63ff* 是你想要标记的提交 ID 的前 10 位字符。



获取提交ID

```bash
git log
```







## 9、log

查看本地仓库的历史记录

```bash
git log
```





## 10、替换本地改动

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：

```bash
git checkout -- <filename>
```

此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。



假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：

```bash
git fetch origin
git reset --hard origin/mai
```

