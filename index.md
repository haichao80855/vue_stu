# vue

## 简介

### 什么是vue

一套构建**用户界面**的**框架**

### vue特性

1. **数据**驱动视图   ---单项数据绑定

   - vue会监听数据变化,从而重新渲染页面结构

2. **双向**数据绑定

   `页面中表单负责采集数据,ajax负责提交数据`

   - 不操作DOM情况下,自动将用户填写数据同步到数据源g
   - js数据变化,会自动渲染到页面上
   
   

### MVVM 

> MVVM是实现数据`驱动视图`和`双向数据绑定`的核心原理

**Model**:当前页面渲染是所依赖的**数据源**

**View**:页面所渲染的**DOM结构**

**ViewModel**:表**Vue的实例**,是MVVM的核心



<img src="知识点.assets/edd0080fb145315fbc96164c219fee7e_hd-16374203520711.png" alt="edd0080fb145315fbc96164c219fee7e_hd" style="zoom:33%;" />



## 基本使用

### 基本步骤

1. 导入vue文件
2. 声明要被vue控制的区域
3. 创建vue实例

## 一.vue指令

### 1.内容渲染指令

1. v-text: 将被控制标签内值覆盖为data对象对应的属性值

```vue
<div id="app" v-text='username'></div>
```

> 缺点:会覆盖原有内容,且只能渲染纯文本

2. {{}} :`插值表达式`

```vue
<div id="app">{{username}}</div>
```

> 只能渲染纯文本内容

| 插值表达式{{}}内能写 什么? |                        |
| -------------------------- | ---------------------- |
| 简单的表达式 :             | 三元表达式,加减乘除... |
|                            | 字符串的拼接,取值赋值  |

3.v-html:将带标签的字符串渲染成真正的html

```
<div id="app" v-html='info'></div>
```

> v-html,v-text写在尖括号内
>
> {{ }}写在两个标签中间
>
> 内容渲染指令不能应用在标签属性上

### 2.属性绑定指令

1.`v-bind`:用于动态修改元素**属性值,**可以简写成冒号`:`

语法：v-bind:属性名="vue变量"

```vue
<img v-bind:src="src">
推荐:<img :src="src">
```

> 插值表达式与属性绑定都可以编写js语句

```js
 <img v-bind:src="src" :alt="'hello' + index">
```

> v-bind绑定内容需要动态拼接字符串外应加单引号

### 3.事件绑定指令

#### 1.基础语法

`v-on:指令='函数名(参数,$event)'`---参数不是必选项

```vue
<button v-on:click='add(参数)'>+1</button>
<button v-on:click='少量代码'>+1</button>
```

```js
methods: {
                add (参数) {
                   log(1)
                } }
```

> 1.事件处理函数要定义到methods对象中

> 2.这些函数就是vm对象的方法,都是添加到vm对象中

> 3.要想传参必须加小括号 v-on:click='函数名()'

> 4.v-on:可简写为@

```vue
<button @click='add'>+1</button>
```

#### 2.注意点:

1.绑定事件`不传参`时候,默认第一个形参就是事件对象e

```
<button v-on:click='add'>+1</button>
```

```js
methods: {
                add (事件对象e) {
                   log(e.targte)    //触发事件对象
                } }
```

#### 3.事件修饰符

`@事件名.修饰符`

绑定事件同时清除默认事件

```vue
<a href="www.baidu.com" @click.prevent='add'>hello</a>
```

> @click.prevent    阻止默认行为
>
> @click.stop   阻止事件冒泡
>
> 清除多个事件可以链式编程

![image-20211121230204820](知识点.assets/image-20211121230204820.png)

#### 4.按键修饰符

按下按键同时判断按的是那个

```vue
<input type="text" @keyup.esc="emp">
```

```vue
<input type="text" @keyup.enter="emp">
```

> esc 与 enter 较为常见

### 4.⭐双向数据绑定指令

1.基本使用

`v-model`

```vue
 <input type="text" v-model='username'>
```

> 数据源的改变可以同步到页面,页面的改变也会同步数据源

> 表单元素使用v-model才有意义

- 如select

  > 下拉菜单v-model应绑定给select
  >
  > 因为v-model本质是修改value的值

- textarea

- input

2.`v-model`的修饰符

![image-20211121234007811](知识点.assets/image-20211121234007811.png)

1.获取转为数字的用户输入

```vue
<input type="text" v-model.number = 'nu1'>
```

2.获取清除两边空格的用户输入数据

```
<input type="text" v-model.trim='n3'>
```

3.不需要实时更新的输入数据而是只需要最后的结果

```vue
<input type="text" v-model.lazy='n3'>
```

> 用户回车或input失焦才会获取改变的值

| v-model绑定给不同的input type,控制不同属性 |                               |
| ------------------------------------------ | ----------------------------- |
| type='text'                                | 控制value的值                 |
| type = 'checkbox'                          | v-model='字符串'  控制checked |
|                                            |                               |

|                          |                |
| ------------------------ | -------------- |
| 获取checkbox 的 value 值 | v-model='数组' |
|                          |                |



### 5.条件渲染指令

作用:用来控制dom元素的显示或隐藏

1.`v-if`原理:每次动态**创建**或**移除**`元素`---`开发常用`

2.`v-show`原理:每次动态为元素**添加**或**删除**`display='none'`

> 频繁切换元素状态v-show性能更好

3.v-else

> v-else-if必须配合v-if使用

```js
	<p v-if = 'type>60'>优秀</p>
        <p v-else-if = 'type<60'>差</p>
        <p v-else>垃圾</p>
```

> 判断要写在单引号内
>
> 若要与字符串比较则字符串要加单引号,外侧要用双引号

```js
  <p v-else-if = "type=='A'">差</p>
```

### 6.列表渲染指令

作用:基于`数组`渲染一个列表结构

`v-for = ''(每个元素,下标) in 列表名''`

```js
<tr v-for='(item,index) in name_arr'>
                    <td>{{index}}</td>
                    <td>{{item.age}}</td>
                    <td>{{item.name}}</td>
                </tr>
```

> 循环生成那个dom结构,就给那个元素身上绑定v-for

> 只要用到了v-for一定要绑定一个:key属性,并且把id作为key的值,且key的值必须为数字或字符串,key值不能重复

| key的注意事项                                |
| -------------------------------------------- |
| 1.key的值只能是**字符串或数字**              |
| 2.key的值必须具有**唯一性**                  |
| 3.建议把数据项**id属性的值**作为key的值      |
| 4.使用**index的值**当作key的值没有意义       |
| 5.建议使用**v-for**指令**一定要指定key的值** |

1.数组方法会改变原始数据,会导致v-for更新

2.不改变数组的原始数据, ,重新赋值  会导致v-for更新

3.或者采用this.$set()  会导致v-for更新

> 单独一行通过索引修改数组内容,不会导致v-for跟新
>
> $set ????  强制更新 ,就地更新

#### 就地更新

什么是就地更新:

> Vue 将**不会移动 DOM 元素**来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。

v-for何时就地更新:

1.没有key

2.key为index

> **只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。



#### Vue key的作用

```




```





⭐v-for + : 可循环为元素更改标签的属性

```vue
 <label v-for="(item,index) in arr" :key="item.id">
     {{item}} <input type="checkbox"  :value="item" v-model="list">
    </label>
// arr: ["科幻", "喜剧", "动漫", "冒险", "科技", "军事", "娱乐", "奇闻"],
```

### 7.虚拟DOM

|          |                                                              |
| :------- | ------------------------------------------------------------ |
| 是什么   | 虚拟 DOM 是轻量级的 JavaScript 对象                          |
| 作用     | UI 进行高效的更新                                            |
| 应用场景 | 在内存中快速找到变化的部分,再更新真实DOM,不频繁操作真实DOM   |
| 怎么用   |                                                              |
| 重点     |                                                              |
| 注意点   | 1.它包含三个参数：元素，具有数据、prop、attr 等的对象，以及一个数组。数组是我们传递子级的地方，子级也具有所有这些参数，然后它们也可以具有子级，依此类推，直到我们构建完整的元素树为止。 |

#### 动态class

语法:

```js
:class="{类名: 布尔值}"
```

> 动态class   false代表使class不生效
>
> 动态class可以与普通class共存  覆盖规则不变
>
> :class="{类名: a>10}"  布尔值可以通过判断得到

应用场景:点击切换高亮

```js
 v-for="(item, index) in list"
      :key="item"
      :class="{ current: index == isIndex }"
      @click="isIndex = index"
```



#### 动态css

语法:

```js
 语法 :style="{css属性名: 值}"
```

...



## 二.过滤器

| filters/filter |                                                              |
| :------------- | ------------------------------------------------------------ |
| 是什么         | 函数                                                         |
| 作用           | 文本的格式化                                                 |
| 应用场景       | 插值表达式,v-bind属性绑定                                    |
| 怎么用         | 拿到管道符前的原值,修改后输出                                |
| 重点           |                                                              |
| 注意点         | 1.必须被定义到**filters**节点下<br />2.过滤器一定要有一个**返回值**<br />3.过滤器形参中的val是**管道符号前面**的值 |

```vue
 <div id="app">{{username | oneUp}}</div>
<p :title='abc | 过滤器名'>
```

```js
 filters: {
                oneUp(val){
                    return val.toUpperCase()
                }	
            }
```



### 私有过滤器与全局过滤器

| 私有过滤器                                              | 全局过滤器                                                |
| ------------------------------------------------------- | --------------------------------------------------------- |
| 定义到vue实例中                                         | Vue.filter定义                                            |
| 只能在当前vue实例中使用                                 | 全局使用                                                  |
| filters: {过滤器名字: (值) => {return "返回处理后的值"} | Vue.filter("过滤器名", (值) => {return "返回处理后的值"}) |
|                                                         | 写在main.js中                                             |

```vue
  //全局过滤器
//Vue.filter('全局过滤器名字',(管道符前的值)=>{})
  Vue.filter('up_name',(str)=>{
              return  str.toUpperCase()
        })
```

> Vue V大写     过滤器的名字需要是字符串

### 连续调用多个过滤器

```vue
 <div id="app">{{username | oneUp| 过滤器2}}</div>
```

### 过滤器传参

```js
 <div id="app">{{username | oneUp(参数1,参数2)}}</div>
```

```
 filters: {
                oneUp(val,参数1){
                    return val.toUpperCase()+num.toString()
                } }
```

> 参数接收的时候第一个值已经被固定,所以接收传入的参数要从第二个开始



## 侦听器

| wach     |                                                              |
| :------- | ------------------------------------------------------------ |
| 是什么   | 函数                                                         |
| 作用     | 监视数据变化,对数据变化进行操作<br />侦听data/computed属性值的改 |
| 应用场景 | 1.判断用户名是否被占用,2.数据同步到localStorage              |
| 怎么用   | 1.定义侦听器2.判断使用哪种格式的侦听器3.开启那些属性         |
| 重点     | 1.对象格式监听器immediate 与 deep 属性                       |
| 注意点   | 1.必须被定义到**watch**节点下<br />2.要监视那个数据变化,就把数据名作为方法名<br />3.形参中新值在前旧值在后 |

### 函格式侦听器:

```vue
 watch: {         // 新值,老值
            username(new_,old_){      
                console.log(new_,old_);
            } }
```

⭐解决问题:使其一进入页面就触发侦听器

| 对象格式侦听器                     | 函数格式侦听器               |
| ---------------------------------- | ---------------------------- |
| 通过immediate:true一进入页面就触发 | 值变化才触发                 |
| 通过deep属性深度监听属性变化       | 对象的属性变化不会触发监听器 |

### 对象式侦听器:

```js
 watch: {
                username:{
                    handle(new_data,old_data){
                        console.log(new_data,old_data);
                    },
			//打卡页面就触发
			immediate:true
                }
            } 
```

> immediate:true 默认为false

#### 深度侦听

⭐解决问题:对象内值无法被监听

```vue
 watch: {
               obj:{
                    handler(new_data,old_data){
                        console.log(new_data,old_data);
                    },
			//对象内的值变化就会被监听到
			deep:true
                }
            } 
```

> !!但是监听到的对象 因为是引用数据类型,所有新旧值一样



⭐解决问题:改变后对象的值不方便

> 如果要直接侦听对象子属性的变化,需要**包裹一层单引号**

```vue
//对象内的值变化就会被监听到
'obj.name':{
                    handler(new_data,old_data){
                        console.log(new_data,old_data);
                    },
                }
```

> 对象.属性监听则不用开启深度监听



## 计算属性

| computed |                                                              |
| :------- | ------------------------------------------------------------ |
| 是什么   | 通过一些列运算得到的**属性值**                               |
| 作用     | 被模板结构或methods方法使用                                  |
| 应用场景 | 对于任何包含响应式数据的复杂逻辑，你都应该使用**计算属性**<br /> 1.**全选按钮**:全选按钮 计算属性:isAll<br />       set(参数)方法-->参数获取当前状态->修改小按钮状态 |
| 怎么用   |                                                              |
| 重点     |                                                              |
| 注意点   | 1.计算属性定义时使方法,使用时使属性<br />2.依赖数据源变化,计算属性会重新求值<br />3.计算属性要在computed节点下,定义成方法格式,**要有返回值** |

```vue
 computed: {
                rgb(){
                    return `rgb(${this.r},${this.g},${this.b})`  
                }
            }
```

> 计算属性也是vue数据变量,所以不要和data里重名

> 计算属性, 基于依赖项的值进行缓存，依赖的变量不变, 都直接从缓存取结果

#### 计算属性完整(对象)写法:

```js
computed: {
  isAll:{
          get(){
              return this.arr.every((item)=>item.checked)
          },
        //   set中形参可以接收 isAll的值
          set(checked){
              //通过大选框 修改小选框
            return  this.arr.forEach(item=>{
                   item.checked = checked
              })
          }
      }
}
```

> 给计算属性赋值时,使用完整写法



## promise

| promise  |          |
| :------- | -------- |
| 是什么   | 构造函数 |
| 作用     |          |
| 应用场景 |          |
| 怎么用   |          |
| 重点     |          |
| 注意点   | 1.       |

#### 1.回调地狱

多层回调函数相互嵌套,就形成了回调地狱

缺点:

1. 代码耦合性太强,难以维护
2. 可读性差

#### 2.Promise概念

1. 是一个构造函数
   - new出来的Promise实例对象,代表**一个异步操作**
2. Promise.prototype上包含一个`.then`方法
3. .then() 方法用来预先指定**成功与失败的回调函数**
   - p.then(result=>{},error=>{})
   - **成功必选**,失败可选



> 创建一个promise对象

```js
new Promise((resolve,reject)=>{})
```

> resolve内传入的值会在then中,reject内传入的值会进入catch中

```js
const promise =  new Promise((resolve,reject)=>{resolve('success')})
const promise2 =  new Promise((resolve,reject)=>{reject('fail')})

promise.then(res=>{
  console.log(res);    //success
})
promise2.catch(res=>{
  console.log(res);   //fail
})
```

> 若想在某个位置`终止执行链`,可以`返回Primise.reject(new Error())`
>
> 之后的.then()内容不执行,直接执行.catch的内容 ,catch中error捕获的是 错误信息 `new Error('你错了')`

```js
const promise =  new Promise((resolve,reject)=>{resolve(1)})
promise.then(res=>{
  return res+1
}).then(res=>{
  console.log(res);   //2
  return res+1
}).then(res=>{
  res+1
  return Promise.reject(new Error('你错了'))
}).then(res=>{
  console.log(res);   //不执行
}).catch(error=>{
  console.log(error);   //Error: 你错了   at eval (main.js?56d7:22)    
})
```



### async 与 await

| async await |                            |
| :---------- | -------------------------- |
| 是什么      | 异步编程解决方案           |
| 作用        | 像写同步代码一样写异步代码 |
| 应用场景    |                            |
| 怎么用      |                            |
| 重点        |                            |
| 注意点      |                            |

> **await** 表示强制等待,**await**后要跟一个promise对象,且等到promise对象resolve成功之后执行,且返回resolve之后的结果

```js
async  test(){
        let data =  await new Promise((resolve)=>{
              setTimeout(() => {
                resolve(100)
              }, 1000);
          })
          console.log(data);   //1s打印   100
        },
```

> ⭐⭐⭐在有**async**修饰的函数中,若想要同步代码等待异步代码,需要在异步代码前添加**await**(异步代码在同步代码前)

```js
async function(){
	await 异步代码   //先执行异步
	log.(123)		//在执行同步
}
```



⭐某个方法的返回值是Primise实例,前面可添加**await**,且方法前必须加**async**

```js
methods: {
    async get_data() {
      let { data: res } = await axios.get(
        "http://123.57.109.30:3006/api/getbooks"
      )
      //参数直接接收返回数据
      this.list = res.data;
    },
  },
```



## axios

| axios    |                    |
| :------- | ------------------ |
| 是什么   | 专注于数据请求的库 |
| 作用     |                    |
| 应用场景 |                    |
| 怎么用   |                    |
| 重点     |                    |
| 注意点   |                    |

1.配置：src/utils/request.js

```js
导入
import axios from "axios";
// 封装请求基地址
axios.defaults.baseURL = "http://localhost:3000" 	// 本地服务器
//导出
export default axios
```

2.封装请求函数：src/api/user.js

```js
// 封装用户相关的接口
import axios from "../utils/request"
// 登录接口 post /login
// post请求必须使用data传参（除非后台特殊说明），get请求必须使用params传参（除非后台特殊说明）
export function login(data){
    return axios({
        url: "/login",
        method: "post",
        data
    })
}
```

3.引入自己封装的登录接口的请求函数

```
import { login } from "@/api/user"
```



> 调用axios的返回值是一个**Promise**对象

```js
//发送get请求
axios({
    method:'get',
    url:'http://www.liulongbin.top:3006/api/getbooks',
    //url查询字符串参数
    params:{},
    //请求体参数
    data:{}
}).then((res)=>{
    console.log(res);
})
```

使用axios拿到的数据是被axios包装过的，真实的服务器返回的数据是对象内的data属性的值

#### 传参方式

> get传参用params
>
> post传参用data

get请求：

```js
axios.get('/user', {
    params: {
      ID: 12345
    }
  }).then()
```

post请求：

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }).then
```



#### then 与 catch

> .then用来指定请求成功之后的回调函数   .catch指定请求失败之后的回调函数
>
> 形参中res是请求得到的结果



⭐⭐解构后重命名

```
let {data:res} = obj
```

使用axios步骤:

1.调用axios之后,使用async和await对其进行简化, 

2.使用解构赋值,从axios封装的大对象中把data属性解构出来

3.把解构出来的data属性,使用冒号对其进行重命名,一般都{data:res}



### axios配置

#### 1.Vue原型挂载axios

⭐**需要在main.js中配置**

> 解决问题:需要多次引入axios

```js
Vue.prototype.$http = axios
```

> 今后在每个.vue组件中要发起请求,直接调用**this.$http**.get/post()

> 缺点:无法实现api复用

#### 2.全局配置请求根路径

```js
axios.defaults.baseURL = '请求根路径'
```













## Vue组件化开发

| 组件化     |                    |
| :--------- | ------------------ |
| 是什么组件 | 后缀名是.vue的文件 |
| 作用       |                    |
| 应用场景   |                    |
| 怎么用     |                    |
| 重点       |                    |
| 注意点     | 1.                 |

### 组件的三个组成部分:

> 组件就是对ui结构的复用

|            |                      |
| ---------- | -------------------- |
| 1.template | 组件的模板结构       |
| 2.script   | 组件的javascript行为 |
| 3.sytle    | 组件的样式           |

### 1.组件中script

1.组件中的data是一个函数

2.数据声明vue的值应在return的对象中 

```vue
 data () {
        return {
            user:'haichao'
        }
    }
```

> 在组件中this是组件的实例对象

### 2.组件中的template 

> 在template 只能有一个`唯一`的根节点



### 3.组件中的sytle

> 启用less语法 

```
写上 <style langs='less'>
```





### 4.组件之间的父子关系

1.组件再被**封装好之后**,彼此之间是**相互独立**的

2.组件在**使用**的是时候,根据彼此的**嵌套关系**,形成了**父子\兄弟关系**





#### 通过components注册的是私有子组件

#### 使用组件的三个步骤:

> vue已经配置好了@指向的就是src 目录

1.使用import 语法导入需要的组件

```js
import Left from '@/components/left.vue'
```

> 导入到srcipt标签内

2.使用components节点注册

```
components:{
      "组件名":组件对象
    }
```

> 与data平齐 
>
> 组件名是自己起的名字,就是后来要用到的标签名
>
> 组件名 与 组件对象可以相同然后简写

```js
components:{
      组件对象
    }
```

3.以标签形式使用刚才的组件

```vue
<组件名></组件名>
```





#### 注册全局组件

⭐如果某个组件频繁用到↓

> 在main.js通过Vue.component()方法,注册全局组件

```
// 导入需要被全局注册的组件
import 组件对象名 from '@/components/comm.vue'
```

```
// 全局注册        
Vue.component('自定义组件名',组件对象)
```

![image-20211126093916413](知识点.assets/image-20211126093916413.png)

> 组件名是自己起的名字,就是后来要用到的标签名

## 组件之间数据共享

### ⭐1.父向子数据传递---Props

| props    |                                                              |
| :------- | ------------------------------------------------------------ |
| 是什么   | 组件的**自定义属性**                                         |
| 作用     | 在封装**通用属性**的时候使用props可以极大提高**组件的复用性** |
| 应用场景 | 配合v-bind ,配合v-for循环                                    |
| 怎么用   | 1.子组件props定义**要接收**的变量,在**子组件使用变量**<br /> 2.父组件内,使用子组件,**属性方式**给props传值 |
| 重点     | 1.props**传入**是简单数据类型则不应修改                      |
| 注意点   | props**传入**是复杂数据类形,可正常修改                       |

> 传入复杂数据类型,其实是传入的复杂数据类型的引用    所以彼此是有关联的
>
> 传入简单数据类型,其实是将数据复制一份传过去   所以之间的数据是没有关联的

#### 声明props

```js
在封装处定义
props:['自定义属性名']
```

```js
使用者:给自定义属性赋值
<Mycomm init='8'></Mycomm>
```

#### 结合v-bind使用props

> 属性加上: 也就是v-bind 属性值内写的就是js代码 想要写字符串就要再加一层引号   ⭐⭐

>  prpos中的数据,可以直接在模板结构中 使用

#### 配合v-for循环

```js
<MyProduct v-for="item in list"
:key="item.id"
:title="item.proname"
:price='item.proprice' 
:intro='item.info'>
 </MyProduct>
```

#### 单项数据流---props是只读的

从**父到子**的**数据流向**,叫`单项数据流`

> 可以理解为props的数据是只读的,**不建议修改**,否则报错
>
> 原因:子组件修改,不通知父级,会造成**数据不一致**
>
> 要想修改,就把这个值赋值给其他变量(data中的数据),修改那个变量
>
> ⭐复杂数据类型,因为修改内容**不会修改内存地址**,所以数据会同步,可以绕过单项数据流,直接对内容进行修改

#### props默认值-default

> 数组格式的props无法定义初始值,需要使用对象格式

```js
props:{
    init:{
    //外界没有传入则默认值生效
      default:20
    }
  }
```

#### props的type值类型

> 可以通过type指定自定义属性的默认类型,传入值不符合则报错

```
props:{
    init:{
      default:20,
      type:Number
    }
  }
```

#### props的required	

> 必填项校验,哪怕有默认值,不传入也报错

#### validator --自定义校验器

```
validator(val){    //val代表传入的值
返回true不报错
返回false报错
}
```



### 2.子向父--自定义事件

#### 1.子组件添加事件

```js
<button @click="sendfun">砍价</button>
```

#### 2.内通过函数调用 `this.$emit`方法,向父组件发送事件

```js
methods: {
    send(){
        this.$emit('自定义事件名'传入数据)
    }}
```

#### 3.父组件内通过定义函数监听子组件传入的自定义事件

```js
<son> @自定义事件名  = '监听事件的函数名' </son>  //1.@后是子组件定义的事件名  2.需要写在子组件标签内
```

```vue
methods: {
    监听事件的函数(传入数据形参){
	log()
    } },
```

> 子组件要 改变 父组件数据时



### 3.通信组件 EvenBus

> 常用于跨组件通信

1. 在**components**目录下创建eventBus.js文件
   1. 在eventBus.js文件内引入vue   `import Vue from "vue";`
   2. 导出vue `export default new Vue()`
2. 确定**发送数据组件**
   1. 引入eventBus  `import eventBus from './eventBus'`
   2. 定义事件且通过eventBus.$emit发送数据   `eventBus.$emit('自定义事件名',数据)`
3. 确定接收数据组件
   1. 引入eventBus  `import eventBus from './eventBus'`
   2. eventBus.$on通过监听事件,接收数据   ` eventBus.$on('自定义事件名',(接收数据)=>{})`
      - 接收数据函数一般定义在vue的`created` 阶段



### 数据传输总结:

#### 子组件**修改父组件**数据

方法一:(简单数据类型)

1. 子组件通过父传子获取父组件数据,	
2. 通过复制该数据,对该数据进行加工
3. 通过子传父将加工好的数据,在父组件进行修改

方法二:(复杂数据类型)

1. 父传子获取复杂数据类型
2. 直接对复杂数据类型进行修改

#### 父组件修改子组件数据

方法一:(复杂数据类型)

1. 子传父获取复杂数据类型
2. 直接对复杂数据类型进行修改

方法二:(简单数据类型)   此方法可解决:无法使定义事件修改子组件数据的问题

1. 给子组件添加ref 属性
2. 父组件内通过this.refs.ref属性值 获取子组件的vue实例
3. 通过.方法获修改值





### 组件之间的样式冲突问题

> 因为样式最后都会被渲染到html页面中所以.vue的样式会全局生效,造成样式冲突

![image-20211126001850286](知识点.assets/image-20211126001850286.png)

#### scoped 的作用

当 `<style>` 标签带有 `scoped` attribute 的时候，它的 CSS 只会应用到当前组件的元素上

> 原因是:当前组件的  **所用标签** 都被添加 [data-v-hash值] 的属性选择器

⭐如何解决?

**自定义属性配合选择器**

方案:样式加上scoped,但是还是有缺陷

缺陷是? 没有办法修改子组件的样式

解决办法是? 子组件要修改前面加`/deep/`

**什么时候会用到deep呢?**

当使用第三方组件库,要修改组件的默认样式时候

⭐尖括号引入法



## 组件的生命周期

#### 生命周期图

![微信图片_20211201215606](知识点.assets/微信图片_20211201215606.jpg)

![image-20211128104944483](知识点.assets/image-20211128104944483.png)

> 不应该使用箭头函数来定义一个生命周期方法
>
> 因为箭头函数绑定了父级上下文，所以 `this` 不会指向预期的组件实例，并且`this.fetchTodos` 将会是 undefined。



### 1.初始化阶段--创建阶段

#### beforeCreate

在实例初始化之后、进行**props/data/methods**的配置**之前**同步调用。

####  ⭐created

在**实例创建完成后**被立即同步调用

可以使用: **props/data/methods的回调函数** 

挂载阶段还没开始，模板结构尚未生成。

| 此阶段作用:                                                |
| ---------------------------------------------------------- |
| 1.发送ajax请求 获取数据. 在methods中定义请求,created中调用 |
|                                                            |

### 2.挂载--创建阶段

> 该钩子在服务器端渲染期间不被调用

#### beforeMount

> 此时还无法操作DoM元素

#### ⭐mounted

> 要操作Dom最早的阶段 mounted



### 3.更新--运行阶段

#### beforeUpdate

> 此阶段**数据是新**的但是**dom结构是旧**的

#### updated

> 此阶段以根据最新数据,对DOM解构完成了渲染
>
> 数据变化后,为了能操作最新的dom,需要将代码写到updated中

4.销毁

### beforeDestroy

> 此阶段将要销毁组件,但是还未销毁

### destroyed

> 此阶段组件已经被销毁,dom结构已经被移出
>



## Vue操作DOM

### ref引用

作用:获取DOM元素或组件的引用

> this就是当前组件的实例
>
> this.$refs 默认是空对象

#### 获取元素DOM

1. 给该标签添加ref属性   `<h1 ref="change">内容</h1>`
2. 获取该元素DOM   `this.$refs.ref属性值`



### this.$nextTick(callback)

> 页面修改完成数据之后就**立即对对应的dom元素进行修改**,会因为dom元素还未更新报错
>
> 通过了解vue生命周期可知 dom更新是有阶段的  先更新数据-->再更新dom

作用:使代码延迟到页面渲染完毕之后再进行执行 回调函数的代码

```js
 this.$nextTick(()=>{
        this.$refs.inpRef.focus()
      })
```



## 动态组件

### 1.component

| component |                                         |
| :-------- | --------------------------------------- |
| 是什么    | vue内置标签                             |
| 作用      | 组件的展示符                            |
| 应用场景  | 1.**组件的切换**                        |
| 怎么用    | is属性的值表示要渲染的组件名,配合v-bind |
| 重点      |                                         |
| 注意点    |                                         |

```vue
<component :is="module"></component>
```

> 组件切换时候会默认会被销毁

### 2.keep-alive保持状态

> 被`keep-alive`标签**包裹**的标签,切换组件时,将组件缓存

```vue
<keep-alive>
<component :is="module"></component>
</keep-alive>
```

#### 2.1keep-alive生命周期函数

| activated-组件激活时调用                                     | deactivated-组件失活时调用 |
| ------------------------------------------------------------ | -------------------------- |
| 组件**第一次**被创建会被先执行**created**也会执行**activated** |                            |

2.2 keep-alive 的include 与 exclude

include作用:指定匹配组件被缓存

exclude:排除谁被缓存

> 只有名称匹配组件会被缓存
>
> 多个组件名逗号隔开
>
> include与 exclude 只能同时使用一个

**扩展**

![image-20211128232920674](知识点.assets/image-20211128232920674.png)



## 插槽

| slot     |                                                             |
| :------- | ----------------------------------------------------------- |
| 是什么   | 组件中不确定,希望用户指定的部分                             |
| 作用     |                                                             |
| 应用场景 |                                                             |
| 怎么用   |                                                             |
| 重点     |                                                             |
| 注意点   | 1.每一个插槽都应有个一个name名称,省略name属性,默认为default |

> 组件内若没有声明插槽,在**组件内容区域**声明的标签会被忽略

```vue
<left><p>hello</p></left>   //left组件内没有插槽,p标签将被忽略
```



#### 标签渲染插槽指定位置

1..要把内容填充到指定插槽中要用v-solt指令

2.v-solt: 指令不能直接用在元素身上,**必须用在template**身上

3.`v-solt:`简写是 #  后面跟的是要放在**插槽的名字**

4.template **只起到包裹的作用**不会被渲染成真实元素

5.solt标签内填写的内容被指定为**默认内容**

```js
<template v-solt:con>
      <p>我是p</p>
    </template>
```

```js
<slot name="con"></slot>
```

> 理解为:将p标签的内容渲染到 名字为con的插槽中

#### 具名插槽

```js
//传输内容
<template #top>
        <h1>title</h1>
      </template>
```

```js
 //接收内容
 <div class="top">
        <slot name="top"></slot>
      </div>
```

> `#`后的值(插槽名)要与 `name ='值'` 一致
>
> 若省略接收slot省略name属性则  `name = 'default'`



#### 作用域插槽---子传父 

> 在声明插槽`solt`时,通过自定义属性给父组件传值,这种用法叫做作用域插槽
>
> 应用:父组件对子组件的内容进行v-for渲染

```vue
<div class="top">
        <slot name="top" msg = 'hello'></slot>
      </div>
```

```vue
<template #top='scope'>     //scope内数据 {'msg':'hello'}
        <h2>{{scope.msg}}</h2>
      </template>
```

> 通过`#插槽名='自定义变量名'` 来接收 
>
> 接收的数据**{标签属性名:属性值,属性名:属性值}**
>
> 建议使用变量名**scope**对属性值进行接收 
>
> 可以直接为接收过来的对象数据进行**解构**
>
> 没有name属性的普通插槽 直接使用v-solt接收数据

```vue
<template #top='{msg}'>
        <h2>{{msg}}</h2>   //hello
      </template>
```



## 自定义指令

### 1.私有自定义指令

> 在**directives**节点下声明私有自定义指令

```js
directives: {
  color: {
    // 指令的定义
    inserted (el) {
      el.style.color = 'red'
    }
  }
}
```

```vue
<h2 v-color=" 'red' ">{{scope}}</h2>
```

#### 1.bind函数

> 当指令**第一次**绑定到元素身上时,就会 自动触发**bind()**函数,当dom**更新时**,bind函数**不会触发**

> 形参中的**el**表示当前指令绑定到的**dom元素对象**

> **binding .value**表示`v-指令=`后传过来值

#### 2.update函数

> DOM每次更新就会被调用

#### 3.函数简写

> 如果bind 与 update 的函数逻辑一致的话可以简写为

```js
color(el, binding) {   //color自定义指令名 
      el.style.color = binding.value;
    },
```

### 全局声明自定义属性

```js
//inserted钩子函数 会在绑定标签时自动执行
Vue.directive('color',{inserted (el,binding) {
  el.style.color = binding.value}
})
```



### ESlint

作用:约束代码风格



## 路由

| slot     |                                   |
| :------- | --------------------------------- |
| 是什么   | 对应关系,hash地址与组件的对应关系 |
| 作用     |                                   |
| 应用场景 |                                   |
| 怎么用   |                                   |
| 重点     |                                   |
| 注意点   |                                   |

1.路由的工作原理

1. 用户点击页面链接导致url中hash值发生变化
2. 前端监听到路由的变化
3. 将hash地址对应的组件渲染到页面中

window.onhashchange  可以监听hash值 的变化



### vue-router

作用:管理spa项目中组件的切换

#### 1.安装与配置

1. 安装vue-router包
2. 创建路由模块
   1. src目录下新建router/index.js 路由模块
3. 导入并挂载路由模块
   1. 导入vue 与 vue-router 包
   2. 调用`Vue.use()`函数,把VueRouter安装未Vue的插件
   3. 创建路由实例对象   `const router = new VueRouter()`
   4. 导出路由实例对象   `export default router`
   5. 在main.js导入路由模块
   6. 对路由进行挂载,与render平级 ,通过router属性挂载路由实例  `router:router`
4. 声明路由链接与占位符
   1. 在主组件声明占位标签   `<router-view></router-view>`
   2. 在路由模块引入子组件   `import above from '../components/子组件.vue'`
   3. 在路由实例对象传入匹配规则  `{path:'/index',component:index}`

#### router-link

作用:替代a链接

1. router-link替代a链接
2. to 替代herf
3. 去掉#

#### redirect重定向

```js
{path:'/',redirect:'/index'},
```

##### 404

```
{
    path: "*",
    component: Notfound
  },
```

> 写在路由最后

#####  路由 - 模式设置

hash路由例如: `http://localhost:8080/#/home`

history路由例如: `http://localhost:8080/home` (以后上线需要服务器端支持, 否则找的是文件夹

[vue配置位置](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90)



#### ⭐嵌套路由

> 通过路由实现组件的**嵌套展示**

##### 通过children属性声明子路由规则

> 子路由规则不要以斜线开头
>
> 与path平级

##### 默认子路由

如果children数组中,某个规则的path值为空,则展示父组件,该子组件会被默认展示

##### 动态路由匹配

> 把hash地址中**可变的部分**定义为**参数项**,从而提高路由的**复用性**

> 使用:声明动态参数
>
> this.$route是路由的**参数对象**
>
> this.$router是**导航对象**
>
> 方法1. this.$route.params.  :后面的值,拿到的就是路径后的**动态参数值**
>
> 方法2. props传参  路由规则对象添加pros:true   通过props拿到动态参数值

通过this.$route.query来访问查询参数

this.$route中path是路径部分   如 /index/1

this.$route的fullPath是完整的地址  如 /index/1?a=1



获取**查询字符串传参**的步骤:

1. app.vue处    /path?参数名=值
2. 接收   $route.query.参数名

获取**路径传参**的步骤:

1. app.vue --- /path/值 – 
2. 路由对象处---需要路由对象提前配置 path: “/path/:参数名”
3.  $route.params.参数名



##### ⭐声明式导航  编程式导航

声明式导航:点击链接实现的导航方式   a  router-link

编程式导航:调用APi方法实现的导航   location.herf

1. this.$router.push('hash地址')      
   - 跳转到指定 hash地址,增加一条历史记录
2.  this.$router.push({name:'/配置的路由名'})
   - 需要在路由对象中进行配置要跳转的路径的name名
3. 编程导航传参传参  ...
4. this.$router.push({path:'/路由路径'})
5. this.$router.replace('hash地址')
   - 跳转到指定 hash地址,替换当前历史记录(不增加)
6. this.$router.go(数值n)
   - -1 后退一步  $router.back()
   -   1 前进一步  $router.forward()
7. this.$router.back()  返回上一步

##### 传参

```vue
 this.$router.push({
        //path
        path:'/part',
        params:{
          name:'haichao'
        }
      })
```



##### 导航守卫

> 可以控制路由的访问权限

##### 全局前置守卫

> 每次导航跳转都会触发的守卫,对路由进行 访问权限控制

```js
//路由实例对象
const router = new VueRouter(...)
// 只要跳转就会触发beforEach 指定的回调  3个形参
router.beforeEach((to,from,next)=>{
//to是将要访问 路由信息对象
//from是将要离开的路由信息对象
// next()表示放行
})
```



##### next() 三种调用方式

![image-20211201232546952](知识点.assets/image-20211201232546952.png)









小总结:

1.vue跨组件修改复杂数据类型时,可以直接进行修改(v-bind,v-model

),也可以修改后再传回去,因为复杂数据类型是修改其引用,不会导致数据不同步







## ES6模块化规范

规范定义:

1. 每个js文件都是一个独立的模块
2. **导入**其他模块成员使用**import**关键字
3. 向外**共享**模块成员使用**export**关键字

在node.js中体验ES6配置

1. 安装v14.15.1或更高版本node.js
2. 在package.json根节点添加"type":"module"

基本语法:





# Vuex

| slot     |                                          |
| :------- | ---------------------------------------- |
| 是什么   | 为 Vue.js 应用程序开发的**状态管理模式** |
| 作用     |                                          |
| 应用场景 |                                          |
| 怎么用   |                                          |
| 重点     |                                          |
| 注意点   |                                          |

### state

### mutation

### Action

### getter

### modle

#### 获取model中state的值

1.基础方法获取子模块的**state**值

```js
$store.state.子模块名.属性名
```

2.通过根模块的getters快捷获取

2.1设置getters

```js
 token: state => state.user.token
```

2.2通过mapGetters引用

```js
computed: {
    ...mapGetters(['token'])
  },
```

#### 模块化中的命名空间

命名空间:**namespase**

> 默认情况下模块中的mutation action getter 是注册在**全局命名空间**的

打开命名空间锁后调用模块action/mutation

方法一:直接调用-带上模块的属性名加/

```js
test () {
   this.$store.dispatch('user/updateToken') // 直接调用action方法
}
```

```js
test () {
   this.$store.commit('user/updateToken') // 直接调用mutation方法
}
```

方法二:辅助函数第一个参数为子模块名字符串

```js
 ...mapMutations('user',['updateToken']),
```

方法三;**createNamespacedHelpers**  创建基于某个命名空间辅助函数

```js
import { mapGetters, createNamespacedHelpers } from 'vuex'
const { mapMutations } = createNamespacedHelpers('user')
...mapMutations(['updateToken']),
```









可选链?

$parent
