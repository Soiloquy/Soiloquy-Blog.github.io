---
title: Vue2组件和脚手架
date: 2023-04-09 17:59:25
tags:
---

# 非单文件组件

**Vue使用组件的三大步骤：**

1. 定义组件（创建组件）
1. 注册组件
1. 使用组件（写组件标签）

**如何定义一个组件：**

==使用Vue.extend(options)创建，其中options和 new Vue（options）时传入的那个options几乎一样，但也有点区别：==

1. el不用写 ---- 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器
1. data必须写成函数 ---- 避免组件被复用时，数据存在引用关系

备注：使用template可以配置组件结构

### 注册组件：

1. 局部注册：靠new Vue的时候传入components选项
1. 全局注册：靠Vue.component（'组件名',组件）

```js
<body>
    <div id="root">
        <!-- 编写组件标签 -->
        <school></school>
        <student></student>
    </div>
</body>
<script>
    // 第一步：创建school组件
    const school = Vue.extend({
        template: `
        <div>
            <h2>学校名称：{{schoolName}}</h2>
            <h2>学校地址：{{address}}</h2>
        </div>
        `,
        data() {
            return {
                schoolName: 'XXXX',
                address: 'MMMMM'
            }
        },
    })
    // 第一步：创建student组件
    const student = Vue.extend({
        template: `
        <div>
            <h2>学生姓名：{{studentName}}</h2>
            <h2>学生年龄：{{age}}</h2>
        </div>
        `,
        data() {
            return {
                studentName: 'Tom',
                age: '19'
            }
        },
    })
    // 第二步：注册组件（全局注册）
    Vue.component('student', student);
    // 创建vm
    new Vue({
        el: '#root',
        // 第二步：注册组件（局部注册）
        components: {
            school
        }
    })
</script>
```

### 注意点：

1. 关于组件名：
  2. 由一个单词组成：(1) 首字母小写   (2) 首字母大写
  3. 多个单词组成：(1) kebab-case命名：my-school   (2) CamelCase命名：Myschool **(需要Vue脚手架)**
  4. 备注:**可使用name配置项指定组件在开发者工具中呈现的名字**
5. 关于组件标签：
  6. <school></school>
  7. <school/>(不使用脚手架时，<school/>会导致后续组件不能渲染)
8. 一个简写方式：`const school=Vue.extend(options)==>const school = options`

### VueComponent构造函数

1. school 组件本质是一个名为VueComponent的构造函数,且不是程序员定义的，是Vue.extend 生成的
2. 在写<school></school>或<school/>时，Vue解析时会帮我们创建school组件的实例对象，**即执行：VueComponent（options）**
3. **特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！**
4. 关于this的指向：
  5. 组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是【VueComponent实例对象】
  6. new  Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是【Vue实例对象】
7. VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）

**==重要的内置关系：==**`VueComponent.prototype.__proto__===Vue.prototype`

![图解](D:\桌面\Note\vm实例对象.png)

**作用：让组件实例对象可以访问到Vue原型上的属性、方法**

# 单文件组件：

### 脚手架安装：

第一步（仅第一次执行）：全局安装@vue/cli：`npm install -g @vue/cli `

第二步：切换到你要创建项目的目录，然后使用命令创建项目 `vue create xxxx `

第三步：启动项目 `npm run serve `

==**备注： **==

1. 如出现下载缓慢请配置 npm 淘宝镜像：npm config set registry https://registry.npm.taobao.or
2. Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpakc 配置， 请执行：`vue inspect > output.js`



不同版本的Vue：

1. vue.js与vun.runtime.xxx.js的区别:
  2. vue.js是完整版的Vue，包含：核心功能 和 模板解析器
  3. vun.runtime.xxx.js是运行版的Vue，只包含：核心功能
4. 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要render函数接收的createElement函数去指定具体的内容

### 模板项目的结构:

```js
├── node_modules 
├── public 
│ ├── favicon.ico: 页签图标 
│ └── index.html: 主页面 
├── src 
│ ├── assets: 存放静态资源 
│ │ └── logo.png 
│ │── component: 存放组件 
│ │ └── HelloWorld.vue 
│ │── App.vue: 汇总所有组件 
│ │── main.js: 入口文件 
├── .gitignore: git 版本管制忽略的配置 
├── babel.config.js: babel 的配置文件 
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件 
├── package-lock.json：包版本控制文件
```

### ref属性：

1. 被用来给元素或子组件注册引用消息（id的替代者）
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
3. 使用方式：
  4. 打标识：`<h1 ref="xxx">....</h1>`或`<School ref="xxx"></School>`
  5. 获取：`this.$refs.xxx`

### 配置项props：

**功能：让组件接收外部传过来的数据**

 1. 传递数据：`<Demo name="xxx"/>`

 2. 接收数据：第一种方式（只接收）：`props：['name']`   第二种方式：(限制类型)：`props:{name:String}`

    第三种方式（限制类型、限制必要性、指定默认值）

    ```js
    props:{
        name:{
    		type:String,//类型
            required:true,//必要性
            default:'XXX'//默认值
        }
    }
    ```

    **备注:props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据**

### mixin（混入）：

**==功能：可以把多个组件共用的配置提取成一个混入对象==**

使用方法：**第一步：**定义混合，例如：

```js
{
    data(){....},
    methods:{....}
    ....
}
```

**第二步：**使用混合，例如：

(1). 全局混入：`Vue.mixin(xxx)`

(2). 局部混入：`mixin:['xxx']`

### Vue插件：

**==插件本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。==**

**定义插件：**

```js
对象.install=function(Vue,options){
    //添加全局过滤器
    Vue.filter(...)
               
    //添加全局指令
   	Vue.directive(...)
    
    //配置全局混入
    Vue.mixin(...)
              
    //添加实例方法
    Vue.prototype.$myMethod=function(){...}
    Vue.prototype.$myProperty=xxxx
}
```

**使用插件：`Vue.use(插件名)`**

# 组件化编码流程：

1. 拆分静态组件：组件要按功能点拆分，命名不能和html元素冲突
2. 实现动态组件：考虑好数据的存放位置，数据是一个组件在用还是一些组件在用
  3. 一个组件在用：放在组件自身即可
  4. 一些组件在用：放在他们共同的父组件上（==状态提升==）
5. 实现交互：从绑定事件开始

**props适用于：**

1. 父组件==>子组件 通信
1. 子组件==>父组件 通信(要求父给子一个函数)

# webStorage

**浏览器端通过`Window.seesionStorage`和`Window.localStorage`属性来实现本地存储机制**

==相关API==

1. `xxxxStorage.setItem('key'，'value')`;  接收一个键值对作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值
2. xxxxStorage.getItem('key'); 接收一个键名作为参数，返回键名对应的值
3. xxxxStorage.removeItem('key'); 该方法接收一个键名作为参数，并把键名从该存储中删除
4. xxxxStorage.clear()  该方法会清空存储中的所有数据

==备注：==`xxxxStorage.getItem('key')`;如果对应的value获取不到，那么getItem的返回值是null

# 组件的自定义事件：

 1. 一种组件间通信的方式，适用于：**==子组件----->>父组件==**

 2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（==事件的回调在A中==）

 3. 绑定自定义事件：

    1. 第一种方式：在父组件中：`<Demo @atguigu="test"/>`或`<Demo v-on:atguigu="test"/>`
    2. 第二种方式，在父组件中：

    ```js
    <Demo ref="demo"/>
        .....
        mounted(){
            this.$refs.xxx.$on('atguigu',this.test)
        }
    ```

    3. 若想让自定义事件只触发一次，可以使用once修饰符，或`$once`方法

 4. 触发自定义事件:`this.$emit('atguigu',数据)`

 5. 解绑自定义事件:`this.$off('atguigu')`

 6. 组件上也可以<span style="color:red">绑定原生DOM事件，需要使用`native`修饰符</span>

 7. 注意：通过`this.$refs.xxx.$on('atguigu',回调函数)`绑定自定义事件时,回调**==要么配置在methods中，要么用箭头函数==**，否则this指向会出问题！

# 全局事件总线（GlobalEventBus）

1. 一种组件间通信的方式，适用于**任意组件间通信**
1. 安装全局事件总线：

```js
new Vue({
    .....
    beforeCreate(){
    	Vue.prototype.$bus=this//安装全局事件总线，$bus就是当前应用的vm
	},
    ....
})
```

 3. 使用事件总线:

    1.  接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件,**事件的回调留在A组件自身**

    ```js
    methods(){
        demo(data){.......}
    }
    .........
    mounted(){
        this.$bus.$on('xxxxx',this.demo)
    }
    ```

    ​	2. 提供数据：`this.$bus,$emit('xxxx',数据)`

  4. 最好在`beforeDestroy`钩子中用`$off`去<span style="color:red">解绑当前组件所用的事件</span>

# 消息订阅与发布(pubsub)

 1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>

 2. 所有步骤：

    1. 安装pubsub：`npm i pubsub-js`

    2. 引入：`import pubsub form 'pubsub-js'`

    3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的**回调留在A组件自身**

       ```js
       methods(){
           demo(data){.....}
       }
       .....
       mounted(){
           this.pid=pubsub.subscribe('xxx',this.demo)//订阅消息
       }
       ```

       4. 提供数据：`pubsub.publish('xxx',数据)`
       5. 最好在`beforeDestroy`钩子中，用`pubsub.unsubscribe(pid)`去<span style="color:red">取消订阅</span>

### 补充：nextTick

1. 语法：`this.$nextTick(回调函数)`
1. 作用：在下一次DOM更新结束后执行其指定的回调
1. 什么时候用：当改变数据后，要基于更新后的新`DOM`进行某些操作时，要在`nextTick`所指定的回调函数中执行

# Vue封装的过度与动画

**作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名**

### 写法：

 	1. 准备好样式：
 	 - 元素进入的样式：
 	   1. v-enter：进入的起点
 	   2. v-enter-active：进入过程中
 	   3. v-enter-to：进入的终点
 	 - 元素离开的样式：
 	   1. v-leave：离开的起点
 	   2. v-leave-active：离开过程中
 	   3. v-leave-to：离开的终点
 	2. 使用`<transition>`包裹要过度的元素,并配置name属性

```js
<transition name="hello">
    <h1 v-show="isShow">你好啊! </h1>
</transition>
```

3. 备注:若有多个元素需要过度,则需要使用:`<transition-group>`，且每个元素都要指定`key`值

# Vue脚手架配置代理

### 方法一：

在`vue.config.js`中添加如下配置：

```js
devServe:{
	proxy:"http://localhost:5000"
}
```

说明:

1. 优点：配置简单，请求资源时直接发给前端（8080）即可
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理
3. 工作方法：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源）

### 方法二：

编写`vue.config.js`配置具体代理规则：

```js
module.exports={
    devServer:{
        proxy:{
            '/api1':{//匹配所有以'/api1'开头的请求路径
                target:'http://localhost:5000',//代理目标的基础路径
                changeOrigin:true,
                pathRewrite:{'^/api1':''}//将/api1替换为空字符
            }
        },
        proxy:{
            '/api2':{//匹配所有以'/api2'开头的请求路径
                target:'http://localhost:5050',//代理目标的基础路径
                changeOrigin:true,
                pathRewrite:{'^/api2':''}//将/api1替换为空字符
            }
        }
    }
}
/*
	changeOrigin设置为true时,服务器收到的请求头中的host为:localhost:5000
	changeOrigin设置为false时,服务器收到的请求头中的host为:localhost:8080
	changeOrigin默认值是true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理
2. 缺点：配置略微繁琐，请求资源时必须加前缀
