---
title: Vue2核心
date: 2023-03-27 21:13:45
tags:
---

# 初识Vue

[尚硅谷Vue教程](https://www.bilibili.com/video/BV1Zy4y1K7SH/?p=5&share_source=copy_web&vd_source=b57f06a84c65383f958c6198780152f6)

1.想让vue工作，必须创建一个Vue实例，且要传入一个配置对象

2.Vue模板：

```js
new Vue({
        el: '#root',//el用于指定当前Vue实例为那个容器服务，通常用css选择器字符串
        data: {
            name: 'xxx'
        }
    })
```

3.Vue实例与容器是一一对应的

4.`{{xxx}}`中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性

**注意区分**：js表达式和js代码（语句）

  - 表达式：一个表达式会产生一个值，可以放在任何地方：
    - a+b
    - demo(1)
  - js代码（语句）
    - if语句

# Vue模板语法

- 插值语法：
  - 功能：用于解析标签体内容
  - 写法：`{{xxx}}`中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
- 指令语法：
  - 功能：用于解析标签（包括：标签属性，标签体内容，绑定事件 等等）
  - 例子：`v-bind:href="xxx"`  或  简写为 ` :href="xxx"`,  xxx同样要写js表达式，且可以自动读取到data中的所有属性

v-bind缩写：

```js
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```



**注：**Vue中有许多指令，且形式都是：v-xxxx

# Vue数据绑定

**Vue有两种绑定的方式：**

- 单向绑定（v-bind）：数据只能从data流向页面
- 双向绑定（v-model）：数据不仅可以从data流向页面，还可以从页面流向data

**注意：**

​	1.双向绑定（v-model）一般都应用在表单类元素上（如：input、select）

​	2.v-model：value 可以简写为v-model，因为v-model默认收集的就是value值

# el和data的两种写法

**el的两种写法：**

```js
const v=new Vue({
        //el: '#root',//第一种写法
        data: {
            name: 'xxx'
        }
    })
v.$mount('#root')//第二种写法
```



**data的两种写法：**

**目前两种写法都行，到组件时，data必须使用函数式**

```js
new Vue({
        el: '#root',
    //第一种写法：对象式
        /*data: {
            name: 'xxx'
        }*/
    //第二种写法：函数式
    data:function(){
        console.log(this);//此处的this是Vue实例对象
		return{
        	name:'尚硅谷'
    	}
     }
    })
```

**一个重要原则：**

​		由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不是Vue实例了

**==观察发现：==**

​	1.data中的所有的属性，最后都出现在了vm身上

​	2. vm身上所有的属性及Vue原型上所有属性，在Vue模板中都可以直接使用

# 事件处理

### 事件的基本使用（点击事件）：

1. 使用v-on：xxx或 @xxx 绑定事件，其中xxx是事件名

   2. 事件的回调需要配置在methods对象中，最终会在vm上
   2. methods中配置的函数，不需要箭头函数！否则this不是vm而是windows
   2. methods中配置的函数，都是被Vue管理的函数，this的指向是vm 或组件实例对象
   2. @click="demo" 和 @click = "demo($event)" 效果一致，但后者可以传参

### Vue中的事件修饰符：

1. ==**prevent：阻止默认事件（常用）**==
1. ==**stop：阻止事件冒泡（常用）**==
1. ==**once：事件只触发一次（常用）**==
1. capture：使用事件的捕获模式
1. self：只有event.target是当前操作的元素时才触发事件
1. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

### 键盘事件：

1. Vue中常用的按键别名：
 	1.  回车=>enter
 	2.  删除=>delete（捕获"删除"和"退格"键）
 	3. 退出=>esc
 	4. 空格=>space
 	5. 换行=>tab（特殊，必须配合keydown去使用）
 	6. 上=>up，下=>down，左=>left，右=>right
2. Vue未提供别名的按键，可以使用按键原始的key值去绑定，但要注意转为kebab-case（短横线命名）
3. 系统修饰键（用法特殊）：ctrl、alt、shift、meta
 	1. 配合keyup使用：按下修饰键的同时，在按下其他键，然后释放其他键，事件才被触发
 	2. 配合keydown使用：正常触发事件
4. Vue.config.keyCodes.自定义键名=键码，可以自定义按键别名

# Vue计算属性

1. 定义：通过计算来得到要用的属性
2. **原理：底层借助了Object.defineproperty 方法提供的 getter 和 setter**
3. **get 函数什么时候执行**
 	1. **==初次读取时会执行一次==** 
 	2. ==**当依赖的数据发生改变时会被再次调用**==
4. 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
5. 备注：
 	1. 计算属性最终会出现在vm上，直接读取使用即可
 	2. 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变

```js
new Vue({
        el: '#root',
        data: {
            firstName: '张',
            lastName: '三'
        },
        computed: {
            fullName: {
                //get有什么作用：当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                //get什么时候被调用：1. 初次读取fullName时  2. 所依赖的数据发生变化时
                get() {
                    // 此处的this是vm
                    console.log(this);
                    return this.firstName + '-' + this.lastName;
                },
                //set什么时候被调用：当fullNmae被修改时
                set(value) {
                    console.log(value);
                    const arr = value.split('-');
                    this.firstName = arr[0];
                    this.lastName = arr[1];
                }
            }
        }
    })
```

==**当只有get时的简写：**==

```js
new Vue({
        el: '#root',
        data: {
            firstName: '张',
            lastName: '三'
        },
        computed: {
           fullName() {
               console.log(this);
               return this.firstName + '-' + this.lastName;
            }
        }
    })
```

# Vue监视属性

### 监视属性watch：

1. 当被监视的属性变化时，回调函数自动调用，进行相关操作
2. 监视的属性必须存在，才能进行监视！！
3. 监视的两种写法：
 	1. new Vue 时传入watch配置
 	2. 通过vm.$watch监视

**第一种写法:**

```js
watch: {
            firstName: {
                //初始化时让handler调用一下
                immediate: true,
                //什么时候调用：当被监视的属性发生改变时
                handler(newValue, oldValue) {
                    console.log('被监视了', newValue, oldValue);
                }
            }
        }
```

**第二种写法:**

```js
vm.$watch('firstName', {
    //初始化时让handler调用一下
    immediate: true,
    //什么时候调用：当被监视的属性发生改变时
    handler(newValue, oldValue) {
        console.log('被监视了', newValue, oldValue);
    }
})
```

### 深度监视：

1. Vue中的watch默认不监测对象内部值的改变（一层）
1. 配置deep：true可以监测对象内部值的改变（多层）

==**注意：**==

1. **Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以**
1. 使用watch时可根据数据的具体结构，决定是否采用深度监视

```js
watch: {
    //监视多级结构中某个属性的变化
    'munber.a': {
      	immediate: true,
        handler(newValue, oldValue) {
            console.log('被监视了', newValue, oldValue);
           }
       }
    //监视多级结构中某个属性的变化
    munber: {
        deep:ture,
        immediate: true,
         handler(newValue, oldValue) {
             console.log('被监视了', newValue, oldValue);
            }
        }
     }
```

## 计算属性与监视属性的区别：

1. computed能完成的功能，watch都可以完成
1. watch能完成的功能，computed不一定能完成（如：watch可以进行异步操作）

==**两个重要的原则：**==

1. 所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象
1. 所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等），最好写成箭头函数，这样this才指向vm 或 组件实例对象

# Vue绑定样式

### 绑定class样式：

1. 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 `:class="mood"`
1. 绑定class样式--数组写法，适用于：样式的个数不确定，名字也不确定 `:class="classArr"`
1. 绑定class样式--对象写法，适用于：要绑定的样式的个数确定，名字也确定，但要动态决定用不用 `:class="classObj"`

```js
<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div><br>
        <!-- 绑定class样式--数组写法，适用于：样式的个数不确定，名字也不确定 -->
        <div class="basic" :class="classArr">{{name}}</div><br>
        <!-- 绑定class样式--对象写法，适用于：要绑定的样式的个数确定，名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj">{{name}}</div><br>
    </div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'XXX',
            mood: 'normal',
            classArr: ['a1', 'a2', 'a3'],
            classObj: {
                a1: false,
                a2: false,
                a3: false
            }
        }
    })
</script>
```

### 绑定style样式（较少用）：

```js
<body>
	<div class="basic" :style="styleObj">{{name}}</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'XXX',
            mood: 'normal',
            styleObj: {
                fontSize: '40px',
                color: 'red'
            }
        }
    })
</script>
```

# Vue渲染

### 条件渲染：

1. **v-if 写法：**
 	1. v-if="表达式"
 	2. v-else-if="表达式"
 	3. v-else="表达式"
2. 适用于：切换频率较低的场景
3. 特点：不展示的DOM元素直接被移除
4. **注意：**v-if可以和 v-else-if、v-else 一起使用，但要求结构不能被打断



1. **v-show写法：**v-show="表达式"
2. 适用于：切换频率较高的场景
3. 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

**==备注：==**使用v-if 的时，元素可能无法获取到，而使用v-show一定可以获取到

```js
//v-if与template的配合使用   
<template v-if="n==1">
        <h2>1</h2>
        <h2>2</h2>
        <h2>3</h2>
</template>
```

### 列表渲染：

**v-for指令：**

1. 用于展示列表数据
1. 语法：v-for= "(item，index) in xxx"    **:key="index"**
1. 可遍历：数组、对象、字符串（很少用）、指定次数（很少用）

**==react、vue中的key有什么作用？==**

1. 虚拟DOM中的key的作用：key是虚拟DOM对象的标识，当数据发生变化时，**Vue会根据【新数据】生成【新的虚拟DOM】，随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较**，比较规则如下：
2. 对比规则：
   1. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
      1. 若虚拟DOM中的内容没变，直接使用之前的真实DOM
      2. 若虚拟DOM中的内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
   2. 旧虚拟DOM中未找到与新虚拟DOM相同的key，则直接创建新的真实DOM，随后渲染到页面中
3. **用index作为key可能会引发的问题：**
   1. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作，会产生没有必要的真实DOM的更新==>页面效果没问题,但效率低
   2. 如果结构中还包括输入类的DOM：会产生错误的更新==>页面有问题
4. 开发中如何选择key？
   1. 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号等
   1. 如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表用于展示，使用index作为key是没有问题的

# Vue监视数据的原理

1. vue会监视data中所有层次的数据
2. 如何监测对象中的数据：**==通过setter实现监视，且要在new Vue时就传入要监测的数据==**
  3. 对象中后追加的属性，Vue默认不做响应式处理
  4. 如需给后添加的属性做响应式，请使用如下API：
          	1. **Vue.set(target，propertyName/index，value)**
                    	2. **vm.$set(target，propertyName/index，value)**
5. 如何监测数组中的数据：通过包裹数组更新元素的方法实现：
   1. 调用原生对应的方法对数据进行更新
   2. 重新解析模板，进行更新页面
6. 在Vue修改数组中的某个元素一定要有如下方法：
   1. 使用这些API：push()  pop()   shift()  unshift()  splice()  sort()  reverse()
   2. Vue.set()  或  vm.$set()

**==特别注意：Vue.set（）和vm.$set（）不能给vm或vm的根数据对象添加属性==**

# Vue收集表单数据

1. `<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值
2. `<input type="radio"/>`，则v-model收集的是value值，且要给标签配置value值
3. `<input type="checkbox"/>`
  4. 没有配置input的value属性，那么收集的就是checked（布尔值）
  5. 配置input的value属性：
          	1. v-model的初始值是非数组，那么收集的就是checked（布尔值）
                    	2. v-model的初始值是数组，那么收集的就是value组成的数组

**==备注：v-model的三个修饰符：==**

1. lazy：失去焦点在收集数据
1. number：输入字符串转为有效的数字
1. trim：输入首尾空格过滤

# Vue过滤器

过滤器定义：对要显示的数据进行特定格式化后再显示（适用于一些简单的逻辑处理）

语法：

1. 注册过滤器：`Vue.filter（name，callback）`或`  new Vue{filter：{}}`
1. 使用过滤器：`{{xxx | 过滤器名}}`  或  `v-bind`：属性="xxx | 过滤器名"

**备注：**

1. 过滤器也可以接收额外的参数，多个过滤器也可以串联
1. 并没有改变原本的数据，是产生新的对应的数据

```js
    // 全局过滤器 
    Vue.filter('mySlice', function (value) {
        return value.slice(0, 4)
    })
    new Vue({
        el: '#root',
        data: {
            name: 'XXX',
            mood: 'normal',
            classArr: ['a1', 'a2', 'a3'],
        },
        // 局部过滤器
        filters: {
            timeFormater(value) {
                return dayjs(value).format(str)
            }
        }
    })
```

# Vue指令

### 我们之前学过的指令：

1. v-bind：单向绑定解析表达式，可简写为	:xxx
1. v-model：双向数据绑定
1. v-for：遍历数组/对象/字符串
1. v-on：绑定事件监听，可简写为@
1. v-if：条件渲染：（动态控制节点是否存在）
1. v-else：条件渲染：（动态控制节点是否存在）
1. v-show：条件渲染：（动态控制节点是否展示）

### v-test和v-html指令：

**v-test指令：**

1. 作用：向其所在的节点中渲染文本内容
1. 与插值语法的区别：v-test会替换掉文本内容，`{{xx}}`则不会

**v-htnl指令：**

1. 作用：向指令节点中渲染包含html结构的内容
2. 与插值语法的区别：
  3. v-html会替换掉节点中所有的内容，`{{xx}}`则不会
  4. v-html可以识别html结构
5. 严重注意：**v-html有安全性问题！！！**
  6. 在网站上动态渲染任意HTML是非常危险的，容易导致**XSS**攻击
  7. 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

### v-once指令：

1. v-once所在节点在初次动态渲染后，就视为静态内容
1. 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

### v-pre指令：

1. 跳过其所在节点的编译过程
1. 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译
