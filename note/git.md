# Git

## 常用命令

### 查看提交文件

```
git log --name-only
```

### 分支

#### 本地分支

```bash
git branch -d zh #删除本地名为zh的分支
```

#### 远程分支

```bash
#创建分支，推送到远程分支
git branch test 
git push origin test

#删除远端分支
git push origin --delete test

git branch -r -d origin/zh #删除
git push origin :zh #sheng'xi
```



### repo sync

从远程仓库更新本地代码，如果远程仓库删除部分代码，使用该命令也会将本地的那部分代码删除.

repo sync -c --force-sync可以强制更新，但会将本地修改的代码删除.

| 参数  |                             作用                             |
| :---: | :----------------------------------------------------------: |
|  -c   |                    只从服务端获取当前分支                    |
|  -d   | 工作区项目进入分离头指针状态，并切换到 manifest 清单文件指定的提交。该参数对于编译构建时严格按照 manifest 清单文件检出提交，**丢弃工作区本地修改，非常有用** |
|  -f   |           即使某个项目同步失败，也继续同步其他项目           |
| -j<N> |                            并发数                            |
|  -n   | 只做网络端操作。即相当于只进行 `git fetch` 操作，不修改本地仓库的检出 |
|  -l   | 只做本地端操作。即相当于只进行 `git checkout` 操作，而不进行任何网络操作 |

**常用形式**：`repo sync -c -j10`

repo forall -c 'git lfs pull'

### untracked files

```bash
git clean -f #删除为跟踪文件

git clean -fd #删除为跟踪文件和目录

git clean -nfd #查看要删除的文件和目录，不会删除任何文件，仅查看
```

