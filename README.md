# vue_messagebox 简介

该项目是剥离mint-ui的MessageBox，并添加了toast弹框，以及支持全局配置(如this.$MessageBox.alert('Test'))功能。

因为一些前端项目需要用到全局弹窗，但用不到整个UI框架。考虑到引入一整个UI框架会增加不少体积，所以就剥离了mint-ui的弹窗。

使用的时候，只需要拷贝 src/components/MessageBox 文件夹到你项目的组件文件夹下，之后参考以下步骤

``` bash
# 页面单独引用
参考views/index.vue的代码

```

``` bash
# 全局引用
a. 在main.js中添加

    import MessageBox from './components/MessageBox/index'

    Vue.use(MessageBox)

b. 参考views/global.vue的代码

```

# mint-ui 链接

配置请参考：http://mint-ui.github.io/docs/#/en2/message-box

# 备注

用户MessageBox用到了scss，所以需要额外安装node-sass和sass-loader


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
