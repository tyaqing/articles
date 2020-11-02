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

