# tyaqing的博客

## 同源和同站

在此前,

https://www.4sus2.com

### 同源(same-origin)/跨域(corss-origin)

同源,我们经常和CORS跨域一起讨论,一般说一个ajax请求跨域指的就是**被请求**的链接与**请求**的链接不同源 .

跨域的域Origin指的是scheme+hostname+port

比如一个接口:https://www.4sus2.com:80/api/getUser?id=xxx

协议scheme为https,主机名hostname是www.4sus.com ,端口port是80

如果2个接口的协议,主机名,端口一致,我们称为`相同来源`,只要有一个不一样,我们就视为`跨域`.

### 同站(same-site)/跨站(cross-site)

同站/跨站的概念与同源/跨域的概念是不同的,同站的匹配规则更加宽松,只需要`eTLD+1`相同即可,无需匹配端口port和协议scheme.

eTLD指的就是有效顶级域名,比如baidu.com,taobao.com,qq.com

eTLD+1指的是相同顶级域名下紧接其后的域名,比如video.qq.com,music.qq.com他们的顶级域名相同,所以这两个网站我们可以称之为`同站`,而www.baodu.com和www.qq.com则为跨站

### 跨站的影响

在Chrome80之前,网站中的点击链接都会发送cookie,chrome80后,默认屏蔽了第三方的 Cookie.

#### SameSite属性

1. **Strict** 仅允许一方请求携带 Cookie，即浏览器将只发送相同站点请求的 Cookie，即当前网页 URL 与请求目标 URL 完全一致。
2. **Lax** 允许部分第三方请求携带 Cookie
3. **None** 无论是否跨站都会发送 Cookie

![img](https://camo.githubusercontent.com/feff36574753ce4c04ddfe9769e623ad671a539ac8792a40f8aff34909ee8114/68747470733a2f2f67772e616c6963646e2e636f6d2f7466732f54423172473448794b4832674b306a535a4645585863714d7058612d313430302d3532382e706e67)

2 月份谷歌发布的 Chrome 80 版本中,

### 解决方案

解决方案就是设置 SameSite 为 none。



**[参考]**

- [同站和同源你理解清楚了么](https://cloud.tencent.com/developer/article/1651506)
- [浏览器系列之 Cookie 和 SameSite 属性 · Issue \#157 · mqyqingfeng/Blog](https://github.com/mqyqingfeng/Blog/issues/157)

## 原型与原型链

参考

- [JavaScript深入之从原型到原型链 · Issue \#2 · mqyqingfeng/Blog](https://github.com/mqyqingfeng/Blog/issues/2)



## js基本脚手架

### pakage.json配置

```js
{
  "name": "js-learn",
  "version": "1.0.0",
  "description": "no description",
  "main": "index.js",
  "repository": "git@codeup.aliyun.com:5fc306172f8cc15c287b6197/default/js-learn.git",
  "author": "tyaqing <626019610@qq.com>",
  "license": "MIT",
  "scripts": {
    "start": "webpack serve",
    "lint": "eslint src/*.js --fix",
    "build": "webpack"
  },
  "devDependencies": {
    "clean-webpack-plugin": "^3.0.0",
    "eslint": "^7.17.0",
    "eslint-config-airbnb-base": "^14.2.1",
    "eslint-config-prettier": "^7.1.0",
    "eslint-plugin-import": "^2.22.1",
    "eslint-plugin-prettier": "^3.3.0",
    "html-webpack-plugin": "^4.5.1",
    "prettier": "^2.2.1",
    "webpack": "^5.11.1",
    "webpack-cli": "^4.3.1",
    "webpack-dev-server": "^3.11.1"
  },
  "dependencies": {}
}

```

### eslint配置

```js
module.exports = {
  env: {
    es6: true,
    browser: true
  },
  extends: ['airbnb-base', 'prettier'],
  parserOptions: {
    ecmaVersion: 10,
    sourceType: 'module'
  },
  rules: {
    'no-console': 0,
    'no-useless-escape': 0,
    'no-multiple-empty-lines': [0],
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        semi: false,
        trailingComma: 'none',
        bracketSpacing: true,
        jsxBracketSameLine: true,
        endOfLine: 'auto'
      }
    ]
  },
  plugins: ['prettier']
}
```

### webpack配置

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  entry: {
    app: './src/index.js'
  },
  mode: 'development',
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devServer: {
    contentBase: './dist'
  },
  devtool: 'inline-source-map',
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'Development'
    })
  ]
}

```



