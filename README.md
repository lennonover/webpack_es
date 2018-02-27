## webpack_ES


## 需求列表 

1、html 文件引入的 js 文件，需要使用 es6、es7 的语法；

2、使用的语法里，除了常规的es6语法外，还包括例如 Promise、async 等特殊特性，要求可以转换。

注：babel 默认只转 js 语法，而不转换新的 API，例如 Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign）。所以我们必须引入额外的babel插件来解决这个问题

## 步骤

安装依赖

```
npm install
```

执行打包命令：

```
npm run test
```

查看打包后效果：

```
index.html
```

## 说明 


关于loader的写法很简单，唯一区别是在哪里设置 babel 的配置。

---

第一种办法：

写在 ``.babelrc`` 文件里，就像我们一般使用 babel 那样，文件内容如下。

```
{
  "presets": [
    "babel-preset-env"
  ],
  "plugins": [
    "transform-runtime"
  ]
}
```

此时 loader 写法如下（以上以下代码已省略）：

```
module: {
    // loader 放在 rules 这个数组里面
    rules: [
        {
            test: /\.js$/,
            // 这里表示忽略的文件夹，正则语法
            exclude: /node_modules/,
            loader: 'babel-loader'
        }
    ]
}
```

---

第二种写法，不使用 ``.babelrc`` 文件，而是直接写在 babel-loader 里。

此时loader写法如下（以上以下代码已省略）：

```
module: {
    // loader 放在 rules 这个数组里面
    rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            options: {
                presets: ['babel-preset-env'],
                plugins: ['transform-runtime']
            }
        }
    ]
}
```

---

第三种写法，和第二种写法类似，只不过细节有所区别。

```
module: {
    // loader放在rules这个数组里面
    rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            // 区别在这里
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['babel-preset-env'],
                    plugins: ['transform-runtime']
                }
            }
        }
    ]
}
```