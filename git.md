# Git 一些问题





## 常见问题

**github不允许上传超过100m的文件,但是已经commit了**

恢复到修改的版本,然后在commit

```ba
git reset HEAD~
```

**github怎么提交多行commit内容**

```bash
git commit -m "这行字会在commit中作为标题 (空格)
1.做了某某修改   (空格)
2.做了xx修改 " (写最后一个"  然后空格结束)
```

这样写出来的注释类似于这样

![截屏2020-11-02 下午5.19.12](https://pic.4sus2.com/uPic/16043087554493vBRze.png)

## git stash暂存

多人开发,经常遇到开发某一个分支时,需要处理其他事情,这时就可以暂存手头的工作,进行其他工作,完事后再恢复,,继续工作.

### 1. 暂存操作

```
#查看当前状态
git status 
#如果有修改,添加修改文件
git add .
#暂存操作
git stash save '本次暂存的标识名字'
123456
```

### 2. 查看当前暂存的记录

```
#查看记录
git stash list
12
```

### 3. 恢复暂存的工作

‘pop命令恢复,恢复后,暂存区域会删除当前的记录’

```
#恢复指定的暂存工作, 暂存记录保存在list内,需要通过list索引index取出恢复
git stash pop stash@{index}
12
```

‘apply命令恢复,恢复后,暂存区域会保留当前的记录’

```
#恢复指定的暂存工作, 暂存记录保存在list内,需要通过list索引index取出恢复
git stash apply stash@{index}
12
```

### 4. 删除暂存

```
#删除某个暂存, 暂存记录保存在list内,需要通过list索引index取出恢复
git stash drop stash@{index}
#删除全部暂存
git stash clear
```