[TOC]

# UI组件

## 1 输入框

### 1.1 绘制带icon的输入框

我们要使用的icon，可以先看官方提供的：https://element.eleme.cn/2.5/#/zh-CN/component/icon

但是官方提供的十分有限，那么我们可以使用阿里提供的。**至于如何离线调用阿里巴巴提供的Icon，[请点击这里参加自己写的博客]()**

如何显示图标，可以参考黑马的教程,[点击这里](D:\WHU\JavaEE\17-21 Vue.js项目实战开发\20-21vue电商\3.vue-项目实战day1\素材\fonts\demo_fontclass.html)

我们首先将项目第一天的素材中的fonts文件夹导入到我们项目的assets文件夹中，然后导入`iconfonts.css`文件，之后我们要使用第三方的icon，只需要修改`prefix-icon`属性中的值。

```html
prefix-icon="iconfont icon-user"
```

```html
<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
```

### 1.2 密码输入框隐藏输入

要让输入框输入的内容加密显示，很简单，只需要在`输入框控件`后加一个`type`属性，并取值为password，则会成为一个密码输入框，对于输入的内容会加密显示。

```vue
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
```



## 2 表单

### 2.1 表单数据绑定 model属性

首先我们将表单控件得数据通过==model==指定绑定到的对象。然后我们通过==v-model==指定要绑定到的对象的具体属性。

至于对象的定义，我们则写在==script标签==内

如代码所示：我们的表单控件的数据来源是loginForm对象，而输入框input的内容则绑定到对象的username一级password属性上。

```vue
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
```



```vue
<script>
export default {
  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      }
    }
  }
}
```

### 2.2 表单验证，添加校验规则

用于在防止用户犯错的前提下，尽可能让用户更早地发现并纠正错误。

通过表单验证，我可以验证用户输入的内容是否符合我们的`规则`，我们在表单通过`rules`属性传入规则对象，在表单的具体控件中通过`prop`指定具体的验证规则。

==我们依然将rules写在data()区域。==

![image-20201125160217153](images/image-20201125160217153.png)

常用的rules如下代码所示：

- required：表示是否为必填项
- message：表示验证不通过时触发的消息
- trigger：表示何时验证，blur表示在鼠标失去焦点的时候进行验证
- min：表示最小的内容长度
- max：表示最长的内容长度
- type：表示数据类型

```vue
rules: {
          name: [
            { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          region: [
            { required: true, message: '请选择活动区域', trigger: 'change' }
          ],
          date1: [
            { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
          ],
          date2: [
            { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
          ],
          type: [
            { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
          ],
          resource: [
            { required: true, message: '请选择活动资源', trigger: 'change' }
          ],
          desc: [
            { required: true, message: '请填写活动形式', trigger: 'blur' }
          ]
        }
```



### 2.3 表单的重置功能

实现点击重置按钮时，重置表单的数据。

每个Form类表单对象都可以执行Form Methods，那么我们如何获取表单对象呢？

我们只需要给表单添加一个引用属性`ref`，给它取一个引用名，然后就可以直接通过该引用名取到该表单对象。我们接下来要给重置的`button`添加一个绑定的槽函数。

==所有的槽函数都写在script标签中的methods的{}中==

比如我们这里的槽函数写法如下:

```vue
<script>
export default {
  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm () {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>
```

按钮绑定函数的代码如下，通过`@click`属性进行函数的绑定:

```vue
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
```

这杨我们点击重置按钮之后执行resetLoginForm的方法，就会执行表单数据的重置。

![image-20201125161520235](images/image-20201125161520235.png)



### 2.4 实现登陆前表单数据的预验证

调用Form的validate方法。我们将写一个函数绑定到登录按钮上，当用户点击按钮以后就直接执行validate方法。

方法依旧写在methods的{}中，函数如下:

- valid表示验证的结果，验证成功为true，失败则为false

```vue
    login () {
      this.$refs.loginFormRef.validate(async valid => {
		console.log(valid)
      })
    }
```



### 2.5 配置axios发起登录请求

但是为了能够在app中使用axios，我们首先要在main.js中进行全局的配置

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import './plugins/element.js'
import './assets/css/global.css'
import './assets/fonts/iconfont.css'
import axios from 'axios'
// 配置请求的根路径
axios.defaults.baseURL = 'http://127.0.0.1:8888/api/private/v1/'
// http请求都通过axios发起
Vue.prototype.$http = axios
Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')

```



登录方法如下:

在未使用异步`async`之前，返回值是一个promise对象，当返回值是一个promise对象的时候，我们可以用await以及async的修饰来简化获取结果的过程。

```
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
        window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/home')
      })
    }
```

$http.post方法的返回值如下，而事实上我们用得到的知识data中的内容而已，为了能够从返回的结果中解析出data部分，在这里我们使用=={data: res}的写法，即取出键为data的部分并且重命名为res==：

![image-20201125165126103](images/image-20201125165126103.png)

## 3 弹框组件

### 3.1 环境配置

我们通过console.log()的方式在控制台中展示数据结果用户是看不到的，为了更加友好的app交互，我们需要配置弹框提示用户结果。

我们在这使用的是message消息提示控件，[官方文档点击这里](https://element.eleme.cn/#/zh-CN/component/message)

![image-20201125165634796](images/image-20201125165634796.png)

此外，每次使用一个新的组件我们都需要在`elemetn.js`中导入

Message的组件和之前的不同，需要进行`全局挂载`，目前导入了组件之后我们的`element.js`如下所示:

- 其中`$message`表示我们自定义的一个全局属性，命名自己取，只需要合法就可以。但是赋值必须是我们导入的Message，这样我们就将Message组件挂载到了Vue的原型对象上，这样，我们就可以通过`this.$message`访问到Message组件。

```javascript
import Vue from 'vue'
import { Button, Form, FormItem, Input, Message } from 'element-ui'

Vue.use(Button)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)
Vue.prototype.$message = Message

```



### 3.2 使用弹框组件提示登录成功

Message提供了error，success等多种提示框，详情见官网示例，这样我们就可以根据情况调用不同的提示框了。

```
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
      })
    }
```





# Vue实战项目：电商管理系统

# 11.16

# 一 项目功能

![image-20201111220241756](images/image-20201111220241756.png)



本项目采用前后端分离的开发模式，其中前端是基于Vue技术栈的SPA项目。



# 二 前端项目初始化

## 1 内容总览

![image-20201116191637360](images/image-20201116191637360.png)

## 2 安装Vue脚手架

参加博客:https://www.jianshu.com/p/1fcc8d55e44b



## 3 创建Vue工程

### 3.1 启动Vue UI

①打开cmd，使用`vue ui`命令打开创建Vue工程的图形界面：

————————————————————

**踩坑**：输入vue ui命令没有任何反应，这是因为低于3.0版本的vue没有ui，我们需要安装更新版本的vue。解决方法参见[点击这里](https://blog.csdn.net/qq_43571415/article/details/103781460)

我们依次在powershell中输入以下命令，即可完成旧版本的卸载以及新版本的安装。

```
npm uninstall vue-cli -g
npm install @vue/cli -g
```

**迷惑：**如何启动一个已经存在的vue项目？

网上说使用`npm run dev`，可是会报错，实际上我们的package.json中就没有dev，我们应该用`npm run serve`来启动项目，如果我们要重新开发项目，则应该继续使用`vue ui`命令，来到图形界面进行开发。

![image-20201116203335006](images/image-20201116203335006.png)

————————————————————



命令运行成功以后，我们进入到图示界面：

![image-20201116194113687](images/image-20201116194113687.png)

### 3.2 选择项目路径

②我们点击创建，然后选择，我们的项目存放路径：

![image-20201116194934928](images/image-20201116194934928.png)

### 3.3 输入项目名称，初始化git

![image-20201116195143379](images/image-20201116195143379.png)

### 3.4 选择预设面板

如果我们之前使用过vue创建过项目，我们可以使用以前预设的配置，但是我们这里演示`手动`配置。

我们一定要勾选上`Babel`，`router`，`Linter`，`使用配置文件`

之后我们进入到下一个界面，我们一定取消勾选使用历史模式的路由，因为哈希模式的路由使用更加方便，我们不使用历史模式。

至于Linter的配置，我们使用标准模式。

![image-20201116195614906](images/image-20201116195614906.png)

之后会询问我们是否要将该项目配置保存为一个预设，需要的话就保存。

![image-20201116195656634](images/image-20201116195656634.png)

## 4 配置Element-UI组件库

在插件中，查询`vue-cli-plugin-element`

![image-20201116195957457](images/image-20201116195957457.png)

记住它的图标，我们安装的版本是1.0.1

![image-20201116200121265](images/image-20201116200121265.png)

安装好了以后，我们要配置插件，不能选择完全导入Element，因为那样会使得我们的项目结构臃肿，我们修改成按需导入。然后点击完成安装等待安装完成即可。

![image-20201116200302551](images/image-20201116200302551.png)

## 5 配置axios库

我们要配置axios，以支持我们发起网络请求。

我们在`依赖`中搜索安装`axios`，注意我们现在安装的版本是`0.21.0`，且一定要安装到`运行依赖`

![image-20201116200415174](images/image-20201116200415174.png)

## 6 将代码托管到gitee

### 6.1 生成ssh公钥

参加gitee教程：https://gitee.com/help/articles/4181#article-header0

生成以后的公钥存放路径保存在如图所示文件中：

![image-20201116201536177](images/image-20201116201536177.png)

之后将公钥复制粘贴到码云中添加即可：

![image-20201116201607662](images/image-20201116201607662.png)

之后就可以创建仓库，并将项目Push到仓库上。

# 三 后端项目初始化

## 1 配置mysql

我们在mysql终端执行以下[文件夹中的sql脚本](D:\BaiduNetdiskDownload\17-21 Vue.js项目实战开发\20-21vue电商\3.vue-项目实战day1\素材\vue_api_server\db)

执行时间比较久，等待一段时间后就可以了。



## 2 安装后台项目依赖

我们今天的后台项目在[该文件夹下](:\BaiduNetdiskDownload\17-21 Vue.js项目实战开发\20-21vue电商\3.vue-项目实战day1\素材\vue_api_server)

我们在项目文件夹下直接打开一个powershell然后输入：

```
npm install
```

之后回车就会自动安装项目需要的所有依赖项了。之后用`node .\app.js`运行项目

————————————————————

**踩坑：**运行项目失败，错误原因是因为mysql的版本过低。解决方法：将pakage.json中的mysql版本更换成最新的2.18.1，然后再次运行`npm install`

![image-20201116204908883](images/image-20201116204908883.png)

然后即使我们安装了最新的mysql，依旧无法解决问题，正确的解决方法参照博客：

https://waylau.com/node.js-mysql-client-does-not-support-authentication-protocol/

将mysql的密码验证权限修改，就可以正常地运行项目了。

————————————————————



# 11.17

# 一 登录/退出功能

## 1 技术选择

记录状态技术的选择：

如果前后端之间不存在跨域的问题，则使用cookie和session记录状态。

如果前后端存在跨域问题，则使用token维持状态。

由于我们这里存在端口号的跨域问题，所以选择使用token。

[什么是跨域问题？点击这里](https://blog.csdn.net/lambert310/article/details/51683775)

![image-20201117150954193](images/image-20201117150954193.png)

## 2 token原理分析

![image-20201117151309427](images/image-20201117151309427.png)

## 3 实现登录界面布局(vue)

在这里我们创建一个新的分支然后继续我们前端项目的开发。

————————————————————

**踩坑：**Vue项目启动以后控件不显示，这是因为我们在创建项目的时候勾选了选择vue的版本。请不要勾选。

![image-20201117160550870](images/image-20201117160550870.png)

————————————————————



### 3.1 编写Login Component

我们在components文件夹下编写一个Login.vue

```vue
<template>
    <div>
        登录组件
    </div>
</template>

<script>
export default {
    
}
</script>
<style lang="less" scoped>

</style>

```

这里值得说明的是scoped指令，如果不在style使用它，那么我们在这里定义的风格style就会全局生效，如果使用了scoped，那么只会对标签内部的配置生效。



### 3.2 定义路由

然后我们在路由中导入Login组件，并且为它配置路由

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Login from "./components/Login.vue"

Vue.use(VueRouter)

const routes = [{
  path:'/login',component:Login
}
]

const router = new VueRouter({
  routes
})

export default router

```



### 3.3 在app根组件中放一个路由的占位符

在template的div标签中添加路由占位符即可。

```vue
<template>
  <div id="app">
    <!--路由占位符-->
    <router-view> </router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
</style>

```



### 3.4 重定向到login

————————————————————

**踩坑：**报错，代码不符合eslint规则。

为了在vs code中解决这个麻烦的问题。

首先，我们在vs code中安装Eslint插件：

![image-20201117170546744](images/image-20201117170546744.png)

安装完成以后，我们在File->Preference->settings中找到extensions，然后在里面找到Eslint，然后在其打开`settings.json`，在其中增加一行。

![image-20201117171952184](images/image-20201117171952184.png)

```json
    "editor.codeActionsOnSave": {
        "source.fixAll": true
    }
```

增加之后的`settings.json`内容如下：

![image-20201117172040073](images/image-20201117172040073.png)

————————————————————



### 3.5 背景色的设置

**依赖安装：安装`less-loader`和`less`到开发依赖**

然后，我们在login.vue代码中新增一个类，配置背景颜色，并且在div标签中通过class属性使用它。

```vue
<template>
    <div class="login_container">
        登录组件
    </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
.login_container{
    background-color: #2b4b6b;
}
</style>

```

此时我们发现显示的页面如图，是颜色是没有填充整个背景的：

![image-20201118203144472](images/image-20201118203144472.png)

### 3.6 通过css让背景色填充整个背景

- 在assets文件夹下新建一个名为css的文件夹用于存法css文件，然后新建一个global.css文件，用于配置全局css样式。

- 编写css代码让html，body以及#app布局高度占满界面。

```css
html,body,#app{
    height: 100%;
    margin: 0;
    padding: 0;
    
}
```

![image-20201118203708904](images/image-20201118203708904.png)

- 同时，设置背景色元素的高度为100%。

```vue
<template>
    <div class="login_container">
        登录组件
    </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
.login_container{
    background-color: #2b4b6b;
    height: 100%;
}
</style>

```

- 在主程序入口导入css配置。直接增加Import即可。

```javascript
import './assets/css/global.css'
```



### 3.7 使用element-ui中的组件

Element-UI的官网：https://element.eleme.cn/2.5/#/zh-CN/guide/design

如果我们在界面开发中使用到了Element-UI中的组件，那么我们需要在Plugins下的element.js文件中import它们，并且使用`Vue.use()`方法将组件注册为全局可用。

```vue
import Vue from 'vue'
import { Button, Form, FormItem, Input } from 'element-ui'

Vue.use(Button)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)

```



## 4 获取token令牌

在之后的会话中，我们都用token来唯一识别是否为同一用户。

token存在于登录post请求结果的data部分的data中。

在我们的工程中可以通过`res.data.token`的方式将之获取，之后我们将其放到SessionStorage中存储，然后跳转页面。

```javascript
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
        window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/home')
      })
    }
```



## 5 路由导航守卫控制页面访问权限

需求：在我们没有使用路由导航守卫时，即使我们没有登录，用户也可以通过直接输入URL访问到home界面，这是不行的。**配置路由导航守卫的目的就是让用户只有在登录状态下才能访问到一些页面**

### 5.1 router.beforeEach

这个方法通过[回调函数](https://www.zhihu.com/question/19801131)进行调用

路由守卫的配置代码如下，在index.js文件中对router对象进行配置

- to表示将要访问的页面路径
- from表示我们从哪个页面路径跳转而来的
- next表示一个放行的函数

```javascript
// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
  // to 将要访问的路径
  // from 代表从哪个路径跳转而来
  // next 是一个函数，表示放行
  //     next()  放行    next('/login')  强制跳转

  if (to.path === '/login') return next()
  // 获取token
  const tokenStr = window.sessionStorage.getItem('token')
  // 如果用户没有token则跳转到登录界面
  if (!tokenStr) return next('/login')
  next()
})
```



## 6 退出

![image-20201201200949789](images/image-20201201200949789.png)

### 6.1 清空token的代码

我们将清空token的代码通过@clickb绑定到单击“退出”按钮的槽函数上

```vue
<template>
  <div>
    <el-button type="info" @click="logout">退出</el-button>
  </div>
</template>

<script>
export default {
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="less" scoped>
</style>

```

## 7 语法处理

### 7.1 处理ESLint的语法警告

让代码自动修改代码语法曼居ESLint规则。我的做法如下：

————————————————————

**踩坑：**报错，代码不符合eslint规则。

为了在vs code中解决这个麻烦的问题。

首先，我们在vs code中安装Eslint插件：

![image-20201117170546744](images/image-20201117170546744.png)

安装完成以后，我们在File->Preference->settings中找到extensions，然后在里面找到Eslint，然后在其打开`settings.json`，在其中增加一行。

![image-20201117171952184](images/image-20201117171952184.png)

```json
    "editor.codeActionsOnSave": {
        "source.fixAll": true
    }
```

增加之后的`settings.json`内容如下：

![image-20201117172040073](images/image-20201117172040073.png)

————————————————————

黑马的做法是在项目的根路径中创建一个名为`.prettierrc`的文件，手动配置语法，写文件内容如下：

这样代码就可以自动去除分号，并且将双引号更改为单引号。

```
{
    "semi":false,
    "singleQuote":true
}
```

并且将小括号前必须要有空格的语法规则禁用，修改项目根路径下的`.eslintrc.js`文件，在rules中添加对`space-before-fuction-paren`这条语法规则的禁用。

```javascript
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: [
    'plugin:vue/essential',
    '@vue/standard'
  ],
  parserOptions: {
    parser: 'babel-eslint'
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off'，
    'space-before-fuction-paren': 0
  }
}

```

