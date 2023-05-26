# Windows-Git 操作指南

[参考视频链接](https://www.bilibili.com/video/BV1pX4y1S7Dq/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=05f64983e307f6fee5b73d4e7077b69b)

## 1. Win安装Git

[下载Git](https://git-scm.com/download/win)

[Git安装教程](https://cloud.tencent.com/developer/article/2099150)



## 2. 基础配置

### 2.1 配置用户名和邮箱

仅作为说明性质。

```shell
git config --global user.name "loserlulin"
```

```shell
git config --global user,email loserlulin@163.com
```



### 2.2 创建仓库

从文件夹新建项目，从而使用Git功能：

```shell
git init
```

从本地克隆Github上的项目：

```shell
git clone <URL>
```



### 2.3 修改与提交文件

#### 2.3.1 跟踪文件或目录

跟踪某文件或目录/跟踪所有文件或目录：

```shell
git add <NAME>
git add .
```

一旦被跟踪则将被永久跟踪直至删除或修改跟踪权限。

删除文件：

```shell
git rm <NAME>
```

保留文件但使其不被跟踪：

```shell
git rm --cache <NAME>
```



#### 2.3.2 设置文件为缓存状态

修改文件后将文件的状态设置为缓存状态：

```shell
git add <NAME>
```

取消文件的缓存状态：

```shell
git reset HEAD <NAME>
```



#### 2.3.3 提交本次修改

提交此次修改并赋予修改内容提示：

```shell
git commit -m '修改内容提示'
```

取消本次提交（无法撤销第一次的提交）：

```shell
git reset head~ --soft
```

对未暂存的文件暂存并提交：

```shell
git commit -am '修改内容提示'
```



#### 2.3.4 查看文件/提交信息

查看文件状态：

```shell
git status
```

> 红色字体：已修改但未暂存
>
> 绿色字体：已暂存但未提交
>
> 无提示：已全部提交

查看具体修改信息：

```shell
git diff
```

查看历史提交：

```shell
git log
```

> 可通过以下形式美化提交信息：
>
> 如把每一次提交的信息变成一行：
>
> ```shell
> git log --pretty=oneline
> ```



## 3. 远程仓库

### 3.1 链接远程仓库

从Github上复制新建的远程仓库的链接URL并在本地链接远程仓库：

```shell
git remote add <NAME> <URL>
```

查看远程仓库：

```shell
git remote
```

重命名远程仓库：

```shell
git remote rename <OldName> <NewName>
```



### 3.2 推送本地分支

```shell
git push <RemoteName> <BranchName>
```

> **鉴别用户权限的两种方法：**
>
> 1. 通过Github token鉴别权限；
> 2. 通过ssh鉴别权限：



## 4. 分支

每一次提交的时候都会对提交对象生成一个Hash值的文件，分支本质就是包含Hash值的文件，可以简单理解为指向提交对象的指针。

### 4.1 查看分支

查看Hash值和当前分支：

```shell
git log
```

查看分支列表(*表示当前分支名称)：

```shell
git branch --list
```



### 4.2 创建/切换/拉取分支

创建分支：

```shell
git branch <BranchName>
```

切换分支：

```shell
git checkout <BranchName>
```

以图形式查看分支的提交记录：

```shell
git log --all --graph
```

拉取分支：

```shell
git fetch
```



### 4.3 合并分支

合并分支到Master分支上：

```shell
git merge <BranchName>
```



### 4.4 存储修改

存储当前为完成的文件修改内容：

```shell
git stash
```

恢复之前存储的内容：

```shell
git stash apply
```

查看存储列表：

```shell
git stash list
```

恢复到指定存储(如：list-1)：

```shell
git stash apply stash@{1}
```



### 4.5 重置

#### 4.5.1 使用重置操作

撤销当前提价：

```shell
git reset head~ --soft
```

> **head表示对于祖先的引用：**
>
> head：表示当前的提交；
>
> head~：表示上次提交；
>
> head~2：表示倒数第二次提交，以此类推。
>
> **soft表示仅撤销提交，但文件暂存状态仍然存在。可换位hard选项，表示撤销修改内容，直接回到上一次提交完之后的状态。**



#### 4.5.2 使用变基操作（慎用）

对于Master分支下同时裂开的A和B两个分支，希望把B分支的内容合并到A分支上：

```shell
git checkout B
git rebase A
```









