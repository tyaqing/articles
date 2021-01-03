# 其他问题

## wordpress忘记密码

```sql
UPDATE wp_users SET user_pass = MD5( 'password' ) WHERE wp_users.user_login = 'admin' LIMIT 1
```

其中password为密码,admin为注册用户名

## 宝塔安装nginx-rtmp-module

参考[通过宝塔面板编译Nginx-rtmp-module模块搭建hls推流](https://www.bt.cn/bbs/forum.php?mod=viewthread&tid=51618&highlight=rtmp)

## nodejs 读行模块readline

[Readline](https://nodejs.org/api/readline.html)

