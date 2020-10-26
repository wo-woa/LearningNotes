# VUE

## 入门示例

### 1、安装

（1）CDN引入

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

（2）NPM安装

```javascript
npm install vue
```

### 2、helloworld

（1）完整代码如下：

```html
<!--第一步：创建文件夹及html文件-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之Helloworld</title>
    <!--第二步：引入Vue库-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <!--第三步:创建一个Div-->
    <div id="app">
        <!--Vue的模板的绑定数据的方法，用两对花括号进行绑定Vue中的数据对象的属性 -->
        {{message}}
    </div>

    <!--第四步：创建Vue的对象，并把数据绑定到上面创建好的div上去。-->
    <script type="text/javascript">
        var app=new Vue({ // 创建Vue对象。Vue的核心对象。
            el:'#app', // el属性：把当前Vue对象挂载到 div标签上，#app是id选择器
            data:{    // data: 是Vue对象中绑定的数据
                message:'hello Vue!' // message 自定义的数据
            }
        })
    </script>
</body>
</html>
```

（2）输出结果：

```html
hello Vue!
```

## 内部指令

### 1、v-if 、v-else、 v-show、v-else-if

这几个条件指令用于显示与隐藏各类元素，使用方式如下：
**（1）v-if**

```javascript
<div v-if="isLogin">你好</div>
```

**（2）v-else**

```javascript
<div v-else>请登录后操作</div>
```

**（3）v-show**

```javascript
<div v-show="isLogin">你好</div>
```

**（4）v-else-if**

```javascript
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

**（5）v-if与v-show的区别**

- v-if： 在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建，开销较高，在运行时条件很少改变时使用。
- v-show：调整css dispaly属性，开销较小，在常频繁地切换时使用。

### 2、v-for

**（1）基本用法**

```html
<!-- 模板 -->
<div id="app">
    <ul>
        <li v-for="item in items">
            {{item}}
        </li>
    </ul>
</div>

<!--JS代码 -->
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            items:[20,23,18,65]
        }
    })
</script>
```

**（2）对象遍历**
参数： 第一个为值，第二个为键名，第三个为索引

```html
<!-- 模板 -->
<div id="app">
    <ul>
        <li v-for="(value, key, index) in object">
        {{ index }}. {{ key }} - {{ value }}
        </li>
    </ul>
</div>

<!--JS代码 -->
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            object: {
                firstName: 'John',
                lastName: 'Doe'
            }
        }
    })
</script>
```

### 3、v-text 、v-html

**（1）v-text**
{{xxx}}取值有个弊端，当网速很慢或javascript出错时，会在页面显示{{xxx}}，Vue提供的v-text可以解决这个问题

```javascript
<div>{{ message }}</div>
<!-- 和下面的一样 -->
<div v-text="message"></div>
```

**（2）v-html**
用于输出html代码

```javascript
<span v-html="msgHtml"></span>
```

### 4、v-on

**（1）常规用法**

```javascript
// html
<div>本场比赛得分：{{count}}</div>
<button v-on:click="add">加分</button>

// JS
data:{
    count: 1
},
methods: {
    add() {
        this.count++;
    }
}
```

**（2）缩写**

```javascript
<button @click="add">加分</button>
```

指令详情的更多用法参照[v-on官方API](https://cn.vuejs.org/v2/api/#v-on)

### 5、v-model

以下的model都需要在data中声明初始值：

```javascript
data: {
    count: 1,
    status: [],
    sex: '男',
    selected: ''
}
```

**（1）input**

```javascript
<input type="text" v-model="message">
```

**（2）textarea**

```javascript
<textarea  cols="30" rows="10" v-model="message"></textarea>
```

**（3）checkbox**

```javascript
<input type="checkbox" id="first" value="1" v-model="status">
<label for="first">有效</label>
<input type="checkbox" id="second" value="2" v-model="status">
<label for="second">无效</label>
<div>状态：{{status}}</div>
```

**（4）radio**

```javascript
<input type="radio" id="one" value="男" v-model="sex">
<label for="one">男</label>
<input type="radio" id="two" value="女" v-model="sex">
<label for="one">女</label>
<div>性别：{{sex}}</div>
```

**（5）select**

```javascript
<select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
</select>
<div>Selected: {{ selected }}</div>
```

### 6、v-bind

用于处理html标签的动态属性，即动态赋值。
**（1）常规用法**

```javascript
// html
<img v-bind:src="imgSrc"  width="200px">

// js
data: {    
    imgSrc:'http://liangxinghua.com/uploads/image/20180709/1531106987.png'
}
```

**（2）缩写**

```javascript
<img :src="imgSrc"  width="200px">
```

指令详情的更多用法参照[v-bind官方API](https://cn.vuejs.org/v2/api/#v-bind)

#### bind 和model的区别

v-model是一种双向绑定，那么也就是说，你绑定的元素得有机会改变值。所以实际上v-model基本上只会用在input, textarea, select这些表单元素上。

v-bind一般用来绑定html属性、css样式等

v-model其实是v-bind和v-on的语法糖，两者的结合

```html
<input v-model="message"> 等价于 <input v-bind:value="message" v-on:input="message = $event.target.value" />
```



### 7、v-pre、v-cloak、v-once

**（1）v-pre**
在模板中跳过vue的编译，直接输出原始值，如果在标签中加入v-pre就不会输出vue中的data值了

```javascript
<div v-pre>{{message}}</div>
```

**（2）v-cloak**
在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用

```javascript
// css
[v-cloak] {
    display: none;
}

// html
<div v-cloak>{{message}}</div>
```

（3）v-once
只显示DOM第一次渲染的值，以后不改变了

```javascript
<div v-once>第一次绑定的值：{{message}}</div>
```

### 8、完整示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之Helloworld</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style type="text/css">
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- v-if -->
        <div v-if="isLogin">你好</div>
        <!-- v-else -->
        <div v-else>请登录后操作</div>
        <!-- v-show -->
        <div v-show="isLogin">你好</div>
        <!-- v-else-if -->
        <div v-if="type === 'A'">A</div>
        <div v-else-if="type === 'B'">B</div>
        <div v-else-if="type === 'C'">C</div>
        <div v-else>Not A/B/C</div>
        <!-- v-for基本使用 -->
        <ul>
            <li v-for="item in items">
                {{item}}
            </li>
        </ul>
        <!-- v-for高级使用 -->
        <ul>
            <li v-for="(value, key, index) in object">
                {{ index }}. {{ key }} - {{ value }}
            </li>
        </ul>
        <!-- v-text -->
        <div v-text="message"></div>
        <!-- v-html -->
        <div v-html="msgHtml"></div>
        <!-- v-on部分 -->
        <div>本场比赛得分：{{count}}</div>
        <!-- 常规写法 -->
        <button v-on:click="add">加分</button>
        <!-- @缩写 -->
        <button @click="add">加分</button><br/>
        <!-- v-model input -->
        <input type="text" v-model="message"><br/>
        <!-- v-model textarea -->
        <textarea  cols="30" rows="10" v-model="message"></textarea><br/>
        <!-- v-model checkbox -->
        <input type="checkbox" id="first" value="1" v-model="status">
        <label for="first">有效</label>
        <input type="checkbox" id="second" value="2" v-model="status">
        <label for="second">无效</label>
        <div>状态：{{status}}</div>
        <!-- v-model radio -->
        <input type="radio" id="one" value="男" v-model="sex">
        <label for="one">男</label>
        <input type="radio" id="two" value="女" v-model="sex">
        <label for="one">女</label>
        <div>性别：{{sex}}</div>
        <!-- v-model select -->
        <select v-model="selected">
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>
        <div>Selected: {{ selected }}</div>
        <!-- v-model select -->
        <img v-bind:src="imgSrc"  width="200px"><br/>
        <img :src="imgSrc"  width="200px"><br/>
        <!-- v-pre -->
        <div v-pre>{{message}}</div>
        <!-- v-cloak -->
        <div v-cloak>{{message}}</div>
        <!-- v-pre -->
        <div v-once>第一次绑定的值：{{message}}</div>
    </div>
    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                isLogin: false,
                type: 'A',
                items:[20,23,18,65],
                object: {
                    firstName: 'John',
                    lastName: 'Doe'
                },
                message: 'hello Vue',
                msgHtml: '<h2>hello Vue!</h2>',
                count: 1,
                status: [],
                sex: '男',
                selected: '',
                imgSrc:'http://liangxinghua.com/uploads/image/20180709/1531106987.png'

            },
            methods: {
                add() {
                    this.count++;
                }
            }
        })
    </script>
</body>
</html>
```

## 生命周期

官方文档：
[vue官方生命周期图](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)
[vue生命周期API](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)

### 1、生命周期图解

![img](http://liangxinghua.com/uploads/image/20180709/1531106989.png)

### 2、生命周期表格

| 周期          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| beforeCreate  | 在实例初始化之后，数据观测和事件配置之前被调用               |
| created       | 在实例创建完成后被立即调用，完成数据观测，属性和方法的运算，初始化事件，$el属性未见 |
| beforeMount   | 在挂载开始之前被调用：相关的 render 函数首次被调用，只在虚拟DOM生成HTML |
| mounted       | 在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互 |
| beforeUpdate  | 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程 |
| updated       | 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用 |
| activated     | keep-alive 组件激活时调用                                    |
| deactivated   | keep-alive 组件停用时调用                                    |
| beforeDestroy | 在实例销毁之前调用。实例仍然完全可用                         |
| destroyed     | 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用 |

#### create和mount的区别

|   生命周期   | 是否获取dom节点 | 是否可以获取data | 是否获取methods |
| :----------: | :-------------: | :--------------: | :-------------: |
| beforeCreate |       否        |        否        |       否        |
|   created    |       否        |        是        |       是        |
| beforeMount  |       否        |        是        |       是        |
|   mounted    |       是        |        是        |       是        |



### 3、代码详解

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之Helloworld</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
    </div>

<script type="text/javascript">
var app=new Vue({
    el:'#app',
    data:{
        message:'hello Vue!'
    },
    beforeCreate: function () {
        console.group('beforeCreate 创建前状态===============》');
        console.log("%c%s", "color:red" , "el      : " + this.$el); //undefined
        console.log("%c%s", "color:red","data    : " + this.$data); //undefined 
        console.log("%c%s", "color:red","message: " + this.message)  
    },
    created: function () {
        console.group('created 创建完毕状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el); //undefined
        console.log("%c%s", "color:red","data    : " + this.$data); //已被初始化 
        console.log("%c%s", "color:red","message: " + this.message); //已被初始化
    },
    beforeMount: function () {
        console.group('beforeMount 挂载前状态===============》');
        console.log("%c%s", "color:red","el      : " + (this.$el)); //已被初始化
        console.log(this.$el);
        console.log("%c%s", "color:red","data    : " + this.$data); //已被初始化  
        console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
    },
    mounted: function () {
        console.group('mounted 挂载结束状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el); //已被初始化
        console.log(this.$el);     
        console.log("%c%s", "color:red","data    : " + this.$data); //已被初始化
        console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
    },
    beforeUpdate: function () {
        console.group('beforeUpdate 更新前状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el);
        console.log(this.$el);    
        console.log("%c%s", "color:red","data    : " + this.$data); 
        console.log("%c%s", "color:red","message: " + this.message); 
    },
    updated: function () {
        console.group('updated 更新完成状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el);
        console.log(this.$el); 
        console.log("%c%s", "color:red","data    : " + this.$data); 
        console.log("%c%s", "color:red","message: " + this.message); 
    },
    beforeDestroy: function () {
        console.group('beforeDestroy 销毁前状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el);
        console.log(this.$el);     
        console.log("%c%s", "color:red","data    : " + this.$data); 
        console.log("%c%s", "color:red","message: " + this.message); 
    },
    destroyed: function () {
        console.group('destroyed 销毁完成状态===============》');
        console.log("%c%s", "color:red","el      : " + this.$el);
        console.log(this.$el);  
        console.log("%c%s", "color:red","data    : " + this.$data); 
        console.log("%c%s", "color:red","message: " + this.message)
    }
})
</script>
</body>
</html>
```

### 4、结果解析

在chrome浏览器里打开，F12看console查看，分三个阶段解读：
**阶段一：创建和挂载**

- beforecreated：el 和 data 并未初始化
- created：完成了 data 数据的初始化，el没有
- beforeMount：完成了 el 和 data 初始化
- mounted ：完成挂载

**阶段二：更新**
在chrome console执行以下命令：

```html
app.message= 'yes !! I do';
```

- beforeUpdate：虚拟DOM中根据data变化去更新html
- updated：将虚拟DOM更新完成的HTML更新到页面中

**阶段三：销毁**
在chrome console执行以下命令：

```html
app.$destroy();
```

- beforeDestroy：销毁之前调用
- destroyed：销毁之后调用，之后再执行app.message= ‘hello vue’，页面不会同步更新。

**具体图解如下：**
![img](http://liangxinghua.com/uploads/image/20180709/1531106990.png)

## 常用选项

vue有许多配置选项，这节之类出常用的一些选项

### 1、computed

计算属性：主要是对原数据进行改造输出。改造输出：包括格式化数据（价格，日期），大小写转换，排序，添加符号

```javascript
<div>价格： {{newPrice}}</div>

computed: {
    newPrice () {
        return '￥' + this.price + '元';
    }
}
```

### 2、methods

方法属性：用于绑定html中的事件对应的方法

```javascript
methods:{
    add (num) {
        this.count += num;
    }
}
```

### 3、watch

数据变化监听器：主要用于监测data中的数据变化，迪v-model生效

```javascript
watch: {
    question(val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal);
    }
}
```

### 4、filters

过滤器：通常格式化字符，使用传值

```javascript
filters: {
    filterA(value) {
        return value.toUpperCase();
    }
}
```

### 5、mixins

混入：用于减少代码污染、减少代码量、实现代码重用

```javascript
// 额外临时加入时，用于显示日志
var addLog={
    updated:function(){
        console.log("数据放生变化,变化成"+this.count+".");
    }
}

// 实例化vue
var app = new Vue({
    // 挂载实例
    el:'#app',
    // 页面数据初始化，字符，对象、数组
    data:{
        count: 100
    },
    // 混入
    mixins: [addLog]
})
```

可以混入多个

### 6、extends

扩展：对构造器进行扩展

```javascript
// 扩展
var extendObj ={
    created: function(){
        console.log("我是被扩展出来的");
    }
}

// 实例化vue
var app = new Vue({
    // 挂载实例
    el:'#app',
    // 页面数据初始化，字符，对象、数组
    data:{
    },
    // 扩展
    extends: extendObj
})
```



### 完整的代码示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之Helloworld</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
        <div>价格： {{newPrice}}</div>
        <div>数字： {{count}}</div>
        <div><button @click="add(2)">add</button></div>
        <div><input v-model="question"></div>
        <div>过滤： {{filtera | filterA}}</div>
    </div>

<script type="text/javascript">
// 额外临时加入时，用于显示日志
var addLog={
    updated:function(){
        console.log("数据放生变化,变化成"+this.count+".");
    }
}

// 扩展
var extendObj ={
    created: function(){
        console.log("我是被扩展出来的");
    }
}

// 实例化vue
var app = new Vue({
    // 挂载实例
    el:'#app',
    // 页面数据初始化，字符，对象、数组
    data:{
        message: 'hello Vue!',
        price: 100,
        count: 100,
        question: '',
        filtera: 'abc'
    },
    // 计算属性：主要是对原数据进行改造输出。
    // 改造输出：包括格式化数据（价格，日期），大小写转换，排序，添加符号
    computed: {
        newPrice () {
            return '￥' + this.price + '元';
        }
    },
    // 方法声明：用于绑定html中的方法
    methods:{
        add (num) {
            this.count += num;
        }
    },
    // data属性监听器, 作用v-model
    watch: {
        question(val, oldVal) {
            console.log('new: %s, old: %s', val, oldVal);
        }
    },
    // 过滤器，通常格式化字符，使用传值
    filters: {
        filterA(value) {
            return value.toUpperCase();
        }
    },
    // 混入，作用：减少代码污染、减少代码量、实现代码重用
    mixins: [addLog],
    // 扩展
    extends: extendObj
})
</script>
</body>
</html>
```

### 拓展

除了这6个以外，还有很多比如mount还是挺常用的，可以初始化一些事情。

另外还有功能比较相似的如create（还是mount好），update还是少用比较好

具体可以看vue的生命周期



vue的extends和mixins类似，通过暴露一个extends对象到组件中使用。

extends会比mixins先执行。执行顺序：extends  > mixins > 组件

extends只能暴露一个extends对象，暴露多个extends不会执行。



## 实例事件

vue有实例属性，实例方法，实例事件，前两个跟选项类似，不是很常用，这次只讲实例事件。

### 1、$on（在构造器外部添加事件）

$on接收两个参数，第一个参数是调用时的事件名称，第二个参数是一个匿名方法

```javascript
app.$on('reduce',function(){
    console.log('执行了reduce()');
    this.count--;
});
```

### 2、$once（执行一次的事件）

```javascript
app.$once('reduceOnce',function(){
    console.log('只执行一次的方法');
    this.count--;
});
```

### 3、$off（关闭事件）

```javascript
function off(){
    console.log('关闭事件');
    app.$off('reduce');
}
```

### 4、$emit（事件调用）

```javascript
function reduce() {
    // 事件调用
    console.log('emit事件调用');
    app.$emit('reduce');
}
```

### 完整示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之Helloworld</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <div>数字：{{count}}</div>
        <button onclick="reduce()">on调用</button>
        <button onclick="reduceOnce()">once调用</button>
        <button onclick="off()">off调用</button>
    </div>

<script type="text/javascript">
var app = new Vue({
    el:'#app',
    data:{
        count: 1
    }
})
// $on 在构造器外部添加事件
app.$on('reduce',function(){
    console.log('执行了reduce()');
    this.count--;
});
// 调用
function reduce() {
    // 事件调用
    console.log('emit事件调用');
    app.$emit('reduce');
}

// $once执行一次的事件
app.$once('reduceOnce',function(){
    console.log('只执行一次的方法');
    this.count--;
});
// 调用
function reduceOnce() {
    app.$emit('reduceOnce');
}

// 关闭事件
function off(){
    console.log('关闭事件');
    app.$off('reduce');
}
</script>
</body>
</html>
```

由于是onclick=reduce，所以是function的reduce方法，而不是app中method方法，更加不是app.$中的reduce方法。

实例事件相当于是在外部把方法添加到vue中，但不是添加到method，虽然很像但是调用的方式不同，需要使用emit来调用

on就是最基础的创建，once只能调用一次，off可以删除on或者once的事件



## 自定义指令

vue中的自定义指令通过Vue.directive来实现，主要完成内置指令不能完成的一些事情

### 1、示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之自定义指令</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <div v-test="color">
        {{num}}
    </div>
</div>
<button onclick="unbindApp()">解绑</button>

<script type="text/javascript">
// 解绑
function unbindApp() {
    app.$destroy();
}

// 自定义指令
Vue.directive("test",{
    //1-被绑定
    bind:function (el, binding, vnode) {
        console.log("1-bind 被绑定");
        console.log("el:",el);
        console.log("binding:",binding);
        console.log("vnode:",vnode);
        el.style.color = binding.value;
    },
    //2-被插入
    inserted:function (el, binding, vnode) {
        console.log("2-inserted 被插入");
    },
    //3-更新
    update:function (el, binding, vnode) {
        console.log("3-update 更新");
    },
    //4-更新完成
    componentUpdated:function (el, binding, vnode) {
        console.log("4-componentUpdated 更新完成");
    },
    //5-解绑
    unbind:function (el, binding, vnode) {
        console.log("5-unbind 解绑");
    }
});

var app = new Vue({
    el:'#app',
    data:{
        num: 123,
        color:'red'
    }
})
</script>
</body>
</html>
```

### 2、调试步骤

（1）chrome打开控制器查看
（2）控制台输入“app.num=’通过控制台设置的新name’”
（3）点击解绑按钮

### 3、参数说明

- el：指令所绑定的元素，可以用来直接操作DOM
- binding： 一个对象，包含指令的很多信息
- vnode:：Vue编译生成的虚拟节点

### 4、生命周期

自定义指令有五个生命周期（也叫钩子函数），分别是bind、inserted、update、componentUpdated、unbind，说明如下：

1. bind：只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作
2. inserted：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）
3. update：被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新
4. componentUpdated：被绑定元素所在模板完成一次更新周期时调用
5. unbind：只调用一次，指令与元素解绑时调用

## 组件基础

### 1、组件注册

**（1）全局注册**

```javascript
// script
Vue.component('button-counter', {
    data: function () {
        return {
            count: 0
        }
    },
    template: '<button v-on:click="count++">全局组件显示： {{ count }}</button>'
});

new Vue({
    el: '#app'
});

// html使用
<button-counter></button-counter>
```

**（2）局部注册**

```javascript
// script
new Vue({
    el: '#app',
    components:{
        "button-inner":{
            data: function() {
                return {
                    inner: 0
                }
            },
            template: '<button v-on:click="inner++">局部组件显示： {{ inner }}</button>'
        }
    }
});

// html使用
<button-inner></button-inner>
```

### 2、props属性传值

**（1）属性取值**

```javascript
// script
new Vue({
        el: '#app',
        components:{
            "button-props":{
                template:`<div style="color:red;">参数1： {{ here }}:---参数2： {{fromHere}}</div>`,
                props:['here', 'fromHere']
            }
        }
    });

// html使用
<button-props here="hello" from-here="world"></button-props>
```

PS：如果属性带“-”，props中需要驼峰取值
**（2）在构造器向组件传值（v-bind）**

```javascript
// script
new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        components:{
            "button-props":{
                template:`<div style="color:red;">参数1： {{ here }}:---参数2： {{fromHere}}</div>`,
                props:['here', 'fromHere']
            }
        }
    });

// html使用
<button-props v-bind:here="message" :from-here="message"></button-props>
```

### 3、父子组件

```javascript
// script
// 子组件
var city = {
    template:`<div>Sichuan of China</div>`
}
// 父组件
var parent = {
    template:
        `<div>
            <p> Panda from China!</p>
            <city></city>
        </div>`,
    components:{
        "city": city
    }
}

// 实例化
new Vue({
    el: '#app',
    // 定义局部组件
    components:{
        // 组件注册
        "parent": parent
    }
});

// html使用
<parent></parent>
```

### 完整示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之组件</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!-- 全局组件 -->
    <div><button-counter></button-counter></div>
    <!-- 局部组件 -->
    <div><button-inner></button-inner></div>
    <!-- 常规属性传值 -->
    <div><button-props here="hello" from-here="world"></button-props></div>
    <!-- v-bind传值 -->
    <div><button-props v-bind:here="message" :from-here="message"></button-props></div>
    <!-- 父子组件调用 -->
    <div><parent></parent></div>

</div>

<script type="text/javascript">
    // 定义全局组件
    Vue.component('button-counter', {
        data: function () {
            return {
                count: 0
            }
        },
        template: '<button v-on:click="count++">全局组件显示： {{ count }}</button>'
    });

    // 子组件
    var city = {
        template:`<div>Sichuan of China</div>`
    }
    // 父组件
    var parent = {
        template:
            `<div>
                <p> Panda from China!</p>
                <city></city>
            </div>`,
        components:{
            "city": city
        }
    }

    // 实例化
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        // 定义局部组件
        components:{
            "button-inner":{
                data: function() {
                    return {
                        inner: 0
                    }
                },
                template: '<button v-on:click="inner++">局部组件显示： {{ inner }}</button>'
            },
            // 取值
            "button-props":{
                template:`<div style="color:red;">参数1： {{ here }}:---参数2： {{fromHere}}</div>`,
                props:['here', 'fromHere']
            },
            // 组件注册
            "parent": parent
        }
    });
</script>
</body>
</html>
```

## 制作模板

vue中的模板使用template来实现

### 1、选项模板

```javascript
<div id="app">
</div>

<script type="text/javascript">
    // 实例化
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        template:`<h1 style="color:red">我是选项模板</h1>`
    });
</script>
```

### 2、<template>标签模板

```javascript
<div id="app">
    <template id="demo2">
        <h2 style="color:red">我是template标签模板</h2>
    </template>
</div>

<script type="text/javascript">
    // 实例化
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        template:'#demo2'
    });
</script>
```

### 3、<script>标签模板

```javascript
<div id="app">
</div>

<script type="x-template" id="demo3">
    <h2 style="color:red">我是script标签模板</h2>
</script>

<script type="text/javascript">
    // 实例化
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        template:'#demo3'
    });
</script>
```

### 完整示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之组件</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!-- template标签模板 -->
    <template id="demo2">
        <h2 style="color:red">我是template标签模板</h2>
    </template>
</div>

<!-- script标签模板 -->
<script type="x-template" id="demo3">
    <h2 style="color:red">我是script标签模板</h2>
</script>

<script type="text/javascript">
    // 实例化
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        // 选项模板
        //template:`<h1 style="color:red">我是选项模板</h1>`
        //template:'#demo2'
        template:'#demo3'
    });
</script>
</body>
</html>
```

## 插槽slot

插槽，也就是slot，是组件的一块HTML模板，一个slot最核心的两个问题是显示不显示和怎样显示

### 1、单个slot

单个插槽，别名默认插槽、匿名插槽，不用设置name属性

```javascript
<div id="app">
    <children1>
        <span>12345</span>
    </children1>
</div>

<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        components: {
            children1: {
                template: "<button><slot></slot>单个插槽</button>"
            }
        }
    });
</script>
```

### 2、具名slot

插槽加了name属性，就变成了具名插槽。具名插槽可以在一个组件中出现N次，出现在不同的位置

```javascript
<div id="app">
    <children2>
        <span slot="first" @click="tobeknow">12345</span>
        <span slot="second">56789</span>
    </children2>
</div>

<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        methods: {
            tobeknow: function () {
                console.log("It is the parent's method");
            }
        },
        components: {
            children2: {//这个无返回值，不会继续派发  
                template: "<button><slot name='first'></slot>具名插槽，<slot name='second'></slot></button>"
            }
        }
    });
</script>
```

### 3、作用域slot

vue2.5版本中slot-scope取代了scope，来实现作用域插槽，主要用在组件调用中，具体在template标签上面使用slot-scope来获取插槽slot上面的属性值，获取值的为一个对象，slot-scope=”它可以取任意字符串”，在element-ui的组件中经常看到。

```javascript
<div id="app">
    <!-- 将数据传递给组件 -->
    <tb-list :data="data">
        <!-- 获取slot上面的值 -->
        <template slot-scope="scope">
            <p>索引：{{JSON.stringify(scope)}}</p>
            <p>索引：{{scope.$index}}</p>
            <p>姓名:{{scope.row.name}}</p>
            <p>年龄: {{scope.row.age}}</p>
            <p>性别: {{scope.row.sex}}</p>
        </template>
    </tb-list>
</div>

<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        data: {
            data: [{
                name: 'kongzhi1',
                age: '29',
                sex: 'man'
            }]
        },
        components: {
            // 作用域slot
            'tb-list': {
                template:
                    `<ul>
                        <li v-for="(item, index) in data">
                            <slot :row="item" :$index="index"></slot>
                        </li>
                    </ul>`,
                // 获取值
                props: ['data']
            }
        }
    });
</script>
```

### 完整代码示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门之slot</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <children1>
        <span>12345</span>
    </children1>

    <children2>
        <span slot="first" @click="tobeknow">12345</span>
        <span slot="second">56789</span>
    </children2>

    <!-- 将数据传递给组件 -->
    <tb-list :data="data">
        <!-- 获取slot上面的值 -->
        <template slot-scope="scope">
            <p>索引：{{JSON.stringify(scope)}}</p>
            <p>索引：{{scope.$index}}</p>
            <p>姓名:{{scope.row.name}}</p>
            <p>年龄: {{scope.row.age}}</p>
            <p>性别: {{scope.row.sex}}</p>
        </template>
    </tb-list>
</div>

<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        data: {
            data: [{
                name: 'kongzhi1',
                age: '29',
                sex: 'man'
            }]
        },
        methods: {
            tobeknow: function () {
                console.log("It is the parent's method");
            }
        },
        components: {
            // 单个slot
            children1: {
                template: "<button><slot></slot>单个插槽</button>"
            },
            // 具名slot
            children2: {
                template: "<button><slot name='first'></slot>具名插槽，<slot name='second'></slot></button>"
            },
            // 作用域slot
            'tb-list': {
                template:
                    `<ul>
                        <li v-for="(item, index) in data">
                            <slot :row="item" :$index="index"></slot>
                        </li>
                    </ul>`,
                // 获取值
                props: ['data']
            }
        }
    });
</script>
</body>
</html>
```

# VUE-CLI

官方的说明文档目前只有英文版的，[vue-cli官方](https://cli.vuejs.org/)
其他的中文翻译[中文翻译文档](https://loulanyijian.github.io/vue-cli-doc-Chinese/)
vue-cli是vue官方出品的快速构建单页应用的脚手架，里面集成了webpack，npm，nodejs，babel，vue，vue-router

## 安装

安装vue-cli的前提是你已经安装了npm，在命令行工具中输入npm -v 命令来检测npm的安装以及版本情况。

### 1、npm安装

node下载地址：[ http://nodejs.cn/download/](http://nodejs.cn/download/)
安装成功提示：

```bash
npm -v
3.10.5
```

### 2、vue-cli安装

全局安装vue-cli，命令行：

```bash
npm install vue-cli -g
```

PS：-g代表全局安装，然后查看版本：

```bash
vue -V
2.9.6
```

PS: 注意这里的V是大写

## 初始化项目

### 1、vue init命令讲解

用vue init命令来初始化项目，具体使用方法如下：

```bash
vue init <template-name> <project-name>
```

init：表示要用vue-cli来初始化项目

<template-name>：表示模板名称，vue-cli官方提供的5种模板：

1. webpack：一个全面的webpack+vue-loader的模板，功能包括热加载，linting,检测和CSS扩展。
2. webpack-simple：一个简单webpack+vue-loader的模板，不包含其他功能，让你快速的搭建vue的开发环境。
3. browserify：一个全面的Browserify+vueify 的模板，功能包括热加载，linting,单元检测。
4. browserify-simple：一个简单Browserify+vueify的模板，不包含其他功能，让你快速的搭建vue的开发环境。
5. simple：一个最简单的单页应用模板。

<project-name>：标识项目名称，用户根据自己的项目来起名字。

### 2、项目初始化

在实际开发中，一般都会使用webpack这个模板，命令使用如下：

```bash
vue init webpack my-vue-demo
```

![img](http://liangxinghua.com/uploads/image/20180709/1531106980.png)

Project name：项目名称 ，默认为初始化建项目的名称my-vue-demo，不需要直接回车
Project description：项目描述，默认为A Vue.js project，不需要直接回车
Author：作者，如果有配置git的作者，自动会读取。直接回车
Install vue-router? 是否安装vue的路由插件，需要安装，选择Y
Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。不需要输入n，需要选择y，如果是大型团队开发，最好是进行配置
setup unit tests with Karma + Mocha? 是否需要安装单元测试工具，不需要输入n，需要选择y
Setup e2e tests with Nightwatch? 是否安装e2e来进行用户行为模拟测试，不需要输入n，需要选择y

初始化完成之后会出现以下信息，表示操作成功。

```bash
# Project initialization finished!
# ========================

To get started:

  npm run dev

Documentation can be found at https://vuejs-templates.github.io/webpack
```

### 3、运行项目

cd my-vue-demo，使用cd命令进入到项目目录

```bash
npm run dev
```

以上命令为开发模式下运行项目

```bash
npm run build
```

以上命令为项目发布打包

### 4、成功页面

![img](http://liangxinghua.com/uploads/image/20180709/1531106983.png)

## 项目结构

### 一、总体框架

一个vue-cli的项目结构如下，其中src文件夹是需要掌握，其余了解即可。
文件夹目录如下：
![img](http://liangxinghua.com/uploads/image/20180709/1531106981.jpg)
每个文件夹目录详细说明如下：
![img](http://liangxinghua.com/uploads/image/20180709/1531106982.png)

### 二、配置目录文件讲解

### 1、build目录（webpack配置）

build文件主要是webpack的配置，目录详情如下：
![img](http://liangxinghua.com/uploads/image/20180709/1531106984.png)

### 2、config目录（vue项目配置目录）

config文件主要是项目相关配置，常用的就是当端口冲突时配置监听端口，打包输出路径及命名等，目录详情如下：
![img](http://liangxinghua.com/uploads/image/20180709/1531106985.png)

### 3、node_modules（项目依赖包）

node_modules里面是项目依赖包，其中包括很多基础依赖，自己也可以根据需要安装其他依赖。安装方法打开命令工具，进入项目目录，输入npm install [依赖包名称]，回车

在两种情况下我们会自己去安装依赖：
》项目运行缺少该依赖包
》安装插件：如vuex

PS：有时会安装指定依赖版本，需在依赖包名称后加上版本号信息，如npm install vue-[loader@11.1.4](mailto:loader@11.1.4)

### 三、src项目核心文件讲解

核心文件目录前面已经说明了，下面重点讲解index.html，main.js，App.vue，router的index.js，HelloWorld.vue

### 1、index.html（主页）

index.html为项目的主页，跟其他html一样，但一般只定义一个空的根节点，在main.js里面定义的实例将挂载在根节点下，内容都通过vue组件来填充。说明如下：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>my-vue-demo</title>
  </head>
  <body>
      <!-- 定义的vue实例将挂载在#app节点下 -->
    <div id="app"></div>
  </body>
</html>
```

### 2、main.js（入口文件）

main.js为项目的入口文件，即单入口，主要是引入vue框架，根组件及路由设置，并且定义vue实例，说明如下：

```javascript
// 引入vue框架
import Vue from 'vue'
// 引入根组件
import App from './App'
// 引入路由配置
import router from './router'

// 关闭生产模式下给出的提示
Vue.config.productionTip = false

// 定义实例
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

### 3、App.vue（根组件）

一个vue页面通常由三部分组成：模板(template)、js(script)、样式(style)，说明如下：

```html
<!-- 模板 -->
<template>
    <div id="app">
        <img src="./assets/logo.png">
        <router-view/>
    </div>
</template>

<!-- js代码 -->
<script>
export default {
    name: 'App'
}
</script>

<!-- css样式 -->
<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
```

**[template-模板]**
（1） 模板只能包含一个父节点，也就是说顶层的div只能有一个（如上图，父节点为#app的div，其没有兄弟节点）
（2）<router-view/>是子路由视图插槽，后面的路由页面都显示在此处，相当于iframe

**【script-JS代码】**
vue通常用es6来写，用export default导出，其下面可以包含数据data，生命周期(mounted等)，方法(methods)等。

**【style-CSS样式】**
样式通过style标签<style></style>包裹，默认是影响全局的，如需定义作用域只在该组件下起作用，需在标签上加scoped，<style scoped></style>

引入外部CSS示例：

```html
<style>
    import './assets/css/public.css'
</style>
```

### 4、router（路由配置）

router文件夹下，有一个index,js的路由配置文件，说明如下：

```javascript
// 引入vue框架
import Vue from 'vue'
// 引入vue-router路由依赖
import Router from 'vue-router'
// 引入页面组件，命名为HelloWorld
import HelloWorld from '@/components/HelloWorld'

// 使用路由依赖
Vue.use(Router)

// 定义路由配置
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
```

### 5、HelloWorld.vue（页面组件）

最熟悉的HelloWorld输出，说明如下：

```html
<template>
  <div>
    <!-- 输出变量 -->
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  // 定义页面名称，可以不要
  name: 'HelloWorld',
  data () {
    return {
      // 定义变量
      msg: 'HelloWorld'
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 16px;
  font-weight: normal;
}
</style>
```

# vue-router

[官方文档](https://router.vuejs.org/zh/)
Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。
包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

## 快速入门

### 一、简单介绍

vue-router是vue官方的路由解决方案，简单易用，中文官方地址如下：
[vue-router中文手册](https://router.vuejs.org/zh/)

### 二、安装

vue-router是一个插件包，需要用npm来进行安装的。如果采用vue-cli构建初始化项目会提示安装，也可以自己使用命令安装：

```bash
npm install vue-router --save
```

### 三、解读核心文件

用vue-cli构建项目之后，在src/router/index.js文件中，看到以下的路由核心文件：

```javascript
// 引入vue框架
import Vue from 'vue'
// 引入vue-router路由依赖
import Router from 'vue-router'
// 引入页面组件，命名为HelloWorld
import HelloWorld from '@/components/HelloWorld'

// Vue全局使用Router
Vue.use(Router)

// 定义路由配置
export default new Router({
  routes: [                //配置路由，这里是个数组
    {                        //每一个链接都是一个对象
      path: '/',            //链接路径
      name: 'HelloWorld',        //路由名称，
      component: HelloWorld     //对应的组件模板
    }
  ]
})
```

### 四、使用

在系统入口文件main.js中注入router，代码如下：

```javascript
// 引入vue框架
import Vue from 'vue'
// 引入根组件
import App from './App'
// 引入路由配置
import router from './router'

// 关闭生产模式下给出的提示
Vue.config.productionTip = false

// 定义实例
new Vue({
  el: '#app',
  router, // 注入框架中
  components: { App },
  template: '<App/>'
})
```

## 路由属性配置说明

代码如下：

```javascript
export default new Router({
    mode: 'history', //路由模式，取值为history与hash
    base: '/', //打包路径，默认为/，可以修改
    routes: [
    {
        path: string, //路径
        ccomponent: Component; //页面组件
        name: string; // 命名路由-路由名称
        components: { [name: string]: Component }; // 命名视图组件
        redirect: string | Location | Function; // 重定向
        props: boolean | string | Function; // 路由组件传递参数
        alias: string | Array<string>; // 路由别名
        children: Array<RouteConfig>; // 嵌套子路由
        beforeEnter?: (to: Route, from: Route, next: Function) => void; // 路由单独钩子
        meta: any; // 自定义标签属性，比如：是否需要登录
        icon: any; // 图标
        // 2.6.0+
        caseSensitive: boolean; // 匹配规则是否大小写敏感？(默认值：false)
        pathToRegexpOptions: Object; // 编译正则的选项
    }
    ]
})
```

## 页面跳转

### 一、router-link标签跳转

在html标签内使用router-link跳转，相应于超链接a标签，使用方式如下：

```javascript
<router-link to="/">[显示字段]</router-link>
```

to：导航路径
使用示例如下：

```javascript
<p>导航 ：
   <router-link to="/">首页</router-link>
   <router-link to="/hello">hello</router-link>
</p>
```

### 二、编程式导航-JS代码内部跳转

实际项目中，很多时候都是通过在JS代码内部进行导航的跳转，使用方式如下：

```javascript
this.$router.push('/xxx')
```

具体的简单用法：
（1）先编写一个按钮，在按钮上绑定goHome( )方法。

```javascript
<button @click="goHome">回到首页</button>
```

（2）在<script>模块里加入goHome方法，并用this.$router.push(‘/’)导航到首页

```javascript
export default {
    name: 'app',
    methods: {
        goHome(){
            this.$router.push('/home');
        }
    }
}
```

### 三、其他常用方法

```javascript
//  后退一步记录，等同于 history.back()
this.$router.go(-1)
// 在浏览器记录中前进一步，等同于 history.forward()
this.$router.go(1)
```

## 子路由-路由嵌套

子路由，也叫路由嵌套，采用在children后跟路由数组来实现，数组里和其他配置路由基本相同，需要配置path和component，然后在相应部分添加<router-view/>来展现子页面信息，相当于嵌入iframe。具体看下面的示例：

### 1、src/components/Home.vue（父页面）

```html
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <!-- 添加子路由导航 -->
        <p>导航 ：
            <router-link to="/home">首页</router-link> | 
            <router-link to="/home/one">-子页面1</router-link> |
            <router-link to="/home/two">-子页面2</router-link>
        </p>
        <!-- 子页面展示部分 -->
        <router-view/>
    </div>
</template>

<script>
export default {
    name: 'Home',
    data () {
        return {
            msg: 'Home Page!'
        }
    }
}
</script>

<style scoped>
</style>
```

### 2、src/components/One.vue（子页面1）

```html
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
    </div>
</template>
<script>
export default {
    name: 'One',
    data () {
        return {
            msg: 'Hi, I am One Page!'
        }
    }
}
</script>

<style scoped>
</style>
```

### 3、src/components/Two.vue（子页面2）

```html
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
    </div>
</template>
<script>
export default {
    name: 'Two',
    data () {
        return {
            msg: 'Hi, I am Two Page!'
        }
    }
}
</script>

<style scoped>
</style>
```

### 4、src/router/index.js（路由配置）

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Home from '@/components/Home'
import One from '@/components/One' 
import Two from '@/components/Two'

Vue.use(Router)

export default new Router({
    routes: [
    {
        path: '/', // 默认页面重定向到主页
        redirect: '/home'
    },
    {
        path: '/home', // 主页路由
        name: 'Home',
        component: Home,
        children:[ // 嵌套子路由
            {
                path:'one', // 子页面1
                component:One
            },
            {
                path:'two', // 子页面2
                component:Two
            },
        ]
    }
    ]
})
```

### 5、效果图

![img](http://liangxinghua.com/uploads/image/20180709/1531106986.png)

PS：各部分代码都很简单，也有注释，在采用vue-cli初始化项目完成之后直接复制到相应目录即可查看效果。

## 路由传递参数

### 1、通过<router-link> 标签中的to传参

**基本语法：**

```javascript
<router-link :to="{name:xxx, params: {key:value}}">valueString</router-link>
```

PS：上面to前边是带冒号，后边跟的是一个对象形势的字符串

- name：在路由配置文件中起的name值。叫做命名路由，下一节会讲到。
- params：要传的参数，它是对象形式，在对象里可以传递多个值。

**具体实例如下：**
（1）在src/components/Home.vue里面导航中添加如下代码：

```javascript
<router-link :to="{name: 'one', params:{username:'test123'}}">子页面1</router-link>
```

（2）在src/router/indes.js中添加如下代码，重点是name：

```javascript
{
    path:'one', // 子页面1
    name: 'one', // 路由名称-命名路由
    component:One
}
```

（3）在src/components/One.vue里面接受参数，代码如下：

```javascript
<h2>{{$route.params.username}}</h2>
```

### 2、url中传递参数

（1）在路由中以冒号传递，在src/router/index.js中加入如下代码：

```javascript
{
    path:'/home/two/:id/:name', // 子页面2
    component:Two
},
```

（2）接受参数，在src/components/Two.vuez中加入如下代码：

```html
<p>ID：{{ $route.params.id}}</p>
<p>名称：{{ $route.params.name}}</p>
```

（3）路由跳转，在src/components/Home.vue中加入如下代码：

```html
<router-link to="/home/two/1/张三">子页面2</router-link>
```

PS：to前没有冒号为字符串路由，必须全部匹配。
（4）如果路由参数需要有特定的规则，就需要加入正则表达式了，示例如下：

```javascript
{
    path:'/home/two/:id(\\d+)/:name', // 子页面2
    component:Two
}
```

### 3、编程式导航-params传递参数

（1）在src/router/index.js页面加入如下代码：

```javascript
{
    path:'/home/three', // 子页面3
    name: 'three',
    component:Three
}
```

（2）在src/components/Three.vue页面加入如下代码：

```javascript
<p>ID：{{ $route.params.id}}</p>
<p>名称：{{ $route.params.name}}</p>
```

（3）在src/components/Home.vue中加入如下代码：

```javascript
// template
<button @click="toThreePage">页面3-params传参</button>

// script
methods: {
    toThreePage() {
        this.$router.push({name: 'three', params: {id: 1, name: 'zhangsan'}})
    }
}
```

说明：
A、动态路由使用params传递参数，在this.$router.push() 方法中path不能和params一起使用，否则params将无效。需要用name来指定页面。
B、以上方式参数不会显示到浏览器的地址栏中，如果刷新一次页面，就获取不到参数了，改进方式将第一部中的代码改成如下：

```javascript
{
    path:'/home/three/:id/:name', // 子页面3
    name: 'three',
    component:Three
}
```

### 4、编程式导航-query传递参数

（1）在src/router/index.js页面加入如下代码：

```javascript
{
    path:'/home/three', // 子页面3
    name: 'three',
    component:Three
}
```

（2）在src/components/Three.vue页面加入如下代码：

```javascript
<p>ID：{{ $route.query.id}}</p>
<p>名称：{{ $route.query.name}}</p>
```

（3）在src/components/Home.vue中加入如下代码：

```javascript
// template
<button @click="toThreePage">页面3-params传参</button>

// script
methods: {
    toThreePage() {
        this.$router.push({path: '/home/three', query: {id: 1, name: 'zhangsan'}})
    }
}
```

PS：动态路由使用query传递参数，会显示到浏览器地址栏中，以上链接为
/home/three?id=1&name=zhangsan

## 命名路由-命名视图-重定向-别名

### 1、命名路由

给一个路由命一个唯一的名称，然后跳转调用这个名称即可。
（1）在src/router/index.js中加一个带name的路由，代码如下：

```javascript
{
    path: 'one', // 子页面1
    name: 'one', // 路由名称-命名路由
    component: One // 页面组件
}
```

（2）在src/component/Home.vue页面中调用，代码如下：

```javascript
// template跳转调用
<router-link :to="{name: 'one'}">子页面1</router-link>

// router.push函数跳转调用
router.push({ name: 'user'}})
```

### 2、命名视图

在同一个页面展示多个视图，如果不用嵌套，只能采用命名视图来实现了，代码如下：
（1）在src/router/index.js中，代码如下：

```javascript
import Vue from 'vue'
import Router from 'vue-router'
// 创建页面组件
const Header = { template: '<div>Header</div>' }
const Left = { template: '<div>Left</div>' }
const Right = { template: '<div>Right</div>' }

Vue.use(Router)

export default new Router({
    routes: [
    {
        path: '/', // 主页路由
        components: {
            default: Header,
            a: Left,
            b: Right
        }
    }
    ]
})
```

（2）在src/App.vue中，代码如下：

```javascript
<template>
    <div id="app">
        <router-view />
        <router-view name="a" class="left" />
        <router-view name="b" class="right" />
    </div>
</template>

<script>
export default {
    name: 'App'
}
</script>

<style>
#app {
    text-align: center;
    color: #2c3e50;
    width: 500px;
    border: 1px solid red;
    margin: 0 auto;
}

.left,.right{
    float: left;
    width:48%;
    text-align: center;
    border:1px solid red
}
</style>
```

PS：经过实践，命名视图只能放在最顶级的页面中，即第一步中的代码不能放在其他页面中。

### 3、重定向

重定向是通过route的配置中关键词redirect来实现的，具体代码如下：
（1）在src/router/index.js中，代码如下：

```javascript
export default new Router({
    routes: [
    {
        path: '/', // 默认页面重定向到主页
        redirect: '/home' // 重定向
    },
    {
        path: '/home', // 主页路由
        component: Home,
        children:[ // 嵌套子路由
            {
                path:'/home/two/:id/:name', // 子页面2
                component:Two
            },
            {
                path:'/home/three/:id/:name', // 子页面3
                name: 'three', // 路由名称-命名路由
                redirect: '/home/two/:id/:name' // 重定向-传递参数
            },
        ]
    }
    ]
})
```

（2）在src/components/Home.vue中，代码如下：

```javascript
<router-link to="/">首页</router-link> | 
<router-link to="/home/two/1/lisi">子页面2</router-link>  |
<router-link :to="{name: 'three', params: {id: 1, name: 'zhangsan'}}">子页面3</router-link>
```

**说明1-不带参数的重定向：**

```javascript
redirect: '/home' // 重定向-不带参数
```

**说明2-带参数的重定向：**

```javascript
redirect: '/home/two/:id/:name' // 重定向-传递参数
```

### 4、别名

重定向是通过route的配置中关键词alias来实现的，具体代码如下：
（1）在src/router/index.js中，代码如下：

```javascript
{
    path:'/one', // 子页面1
    component:One,
    alias: '/oneother'
}
```

（2）在src/components/Home.vue中，代码如下：

```javascript
<router-link to="/oneother">子页面1</router-link>
```

**说明1：redirect和alias的区别**

- redirect：直接改变了url的值，把url变成了真实的path路径。\
- alias：url路径没有别改变，这种更友好，让用户知道自己访问的路径，只是改变了<router-view>中的内容。

**说明2：**
别名请不要用在path为’/’中，如下代码的别名是不起作用的。

```javascript
{
    path: '/',
    component: Hello,
    alias:'/home'
}
```

## 过渡动画

### 1、代码示例

（1）在<router-view>标签的外部添加<transition>标签，标签还需要一个name属性，代码如下：

```javascript
<transition name="fade" mode="out-in">
    <router-view />
</transition>
```

（2）加入CSS，一共4个CSS类名，四个类名与transition的name属性有关，比如name=”fade”，相应的css如下：

```javascript
/*页面切换动画*/
/*进入过渡的结束状态，元素被插入时就生效，在过渡过程完成后移除*/
.fade-enter-active {
    transition: opacity .5s;
}
/*进入过渡的开始状态，元素被插入时生效，只应用一帧后立刻删除*/
.fade-enter {
    opacity: 0;
}
/*离开过渡的开始状态，元素被删除时触发，只应用一帧后立刻删除*/
.fade-leave {
    opacity: 1;
}
/*离开过渡的结束状态，元素被删除时生效，离开过渡完成后被删除*/
.fade-leave-active {
    opacity:0;
    transition: opacity .5s;
}
```

### 2、过渡模式mode

- in-out：新元素先进入过渡，完成之后当前元素过渡离开，默认模式。
- out-in：当前元素先进行过渡离开，离开完成后新元素过渡进入。

## mode与404

### 1、mode模式

代码示例：

```javascript
export default new Router({
    mode: 'history', //mode模式
    routes: [...]
})
```

**mode取值说明：**
（1）histroy：URL就像正常的 url，示例：http://localhost:8080/home
（2）hash：默认值，会多一个“#”，示例：http://localhost:8080/#/home

### 2、404页面设置

如果访问的路由不存在，或者用户输入错误时，会有一个404友好的提示页面，配置如下：
（1）在/src/router/index.js中加入如下代码：

```javascript
// 404
{
    path: '*',
    component: () => import('@/components/404')
}
```

（2）在src/components/404.vue中编写如下代码：

```javascript
<template>
    <div class="hello">
        <h1>404 not found</h1>
    </div>
</template>
<script>
export default {
    data () {
        return {

        }
    }
}
</script>

<style scoped>
</style>
```

## 路由钩子

路由钩子，即导航钩子，其实就是路由拦截器，vue-router一共有三类：

1. 全局钩子：最常用
2. 路由单独钩子
3. 组件内钩子

### 1、全局钩子

在src/router/index.js中使用，代码如下：

```javascript
// 定义路由配置
const router = new VueRouter({ ... })

// 全局路由拦截-进入页面前执行
router.beforeEach((to, from, next) => {
    // 这里可以加入全局登陆判断
    // 继续执行
    next();
});

// 全局后置钩子-常用于结束动画等
router.afterEach(() => {
    //不接受next
});

export default router;
```

**每个钩子方法接收三个参数：**
to: Route : 即将要进入的目标 [路由对象]
from: Route : 当前导航正要离开的路由
next: Function : 继续执行函数

- next()：继续执行
- next(false)：中断当前的导航。
- next(‘/‘) 或 next({ path: ‘/‘ })：跳转新页面，常用于登陆失效跳转登陆

### 2、路由单独钩子

使用：在路由配置中单独加入钩子，在src/router/index.js中使用，代码如下：

```javascript
{
    path:'/home/one', // 子页面1
        component: One,
        // 路由内钩子
        beforeEnter: (to, from, next) => {
        console.log('进入前执行');
            next();
        }
}
```

### 3、组件内钩子

使用：在路由组件内定义钩子函数：

- beforeRouteEnter：进入页面前调用
- beforeRouteUpdate (2.2 新增)：页面路由改变时调用，一般需要带参数
- beforeRouteLeave：离开页面调用

任意找一页面，编写如下代码：

```javascript
<script>
export default {
    name: 'Two',
    data () {
        return {
            msg: 'Hi, I am Two Page!'
        }
    },
    // 进入页面前调用
    beforeRouteEnter(to, from, next) {
        console.log('进入enter路由钩子')
        next()
    },
    // 离开页面调用
    beforeRouteLeave(to,from, next){
        console.log('进入leave路由钩子')
        next()
    },
    // 页面路由改变时调用
    beforeRouteUpdate(to, from, next) {
        console.log('进入update路由钩子')
        console.log(to.params.id)
        next()
    }
}
</script>
```

## 路由懒加载

### 1、路由正常模式：

```javascript
// 1、先引入页面组件
import Home from '@/components/Home'

// 2、使用组件
    {
        path: '/home',
        component: Home
}
```

### 2、懒加载模式

大项目中，为了提高初始化页面的效率，路由一般使用懒加载模式，一共三种实现方式。
（1）第一种写法：

```javascript
component: (resolve) => require(['@/components/One'], resolve)
```

（2）第二种写法：

```javascript
component: () => import('@/components/Two')
```

（3）第三种写法：

```javascript
components: r => require.ensure([], () => r(require('@/components/Three')), 'group-home')
```

**PS：**

- 一般常用第二种简写
- 第三种中，’group-home’是把组件按组分块打包, 可以将多个组件放入这个组中，在打包的时候Webpack会将相同 chunk 下的所有异步模块打包到一个异步块里面。



# VUEX

## 背景引言

### 1、vuex是什么？

vuex是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。chrome安装调试工具 devtools extension

### 2、单向数据流

![img](http://liangxinghua.com/uploads/image/20180709/1531106987.png)
**示意图说明：**

- State：驱动应用的数据源（单向数据流）
- View：以声明方式将 state 映射到视图（静态显示出来的数据源）
- Actions：处理用户在view上面操作而导致的状态变化（数据源变化追踪）

一个简单的demo案例：

```javascript
<template>
    <div>
        <!-- view -->
        <div>{{ count }}</div>
        <button @click="increment">increment</button>
    </div>
</template>
<script>
export default {
    // state
    data () {
        return {
            count: 0
        }
    },
    // actions
    methods: {
        increment () {
            this.count++
        }
    }
}
</script>
<style>

</style>
```

### 3、vuex解决的问题

- 多个视图组件，包括父子组件，兄弟组件之间的状态共享
- 不同视图组件的行为需要变更同一个状态

### 4、vuex使用场景

中大型单页应用，需要考虑如何更好地在组件外部管理状态，简单应用不建议使用

### 5、vuex与全局变量的区别

- 响应式：vuex的状态存储是响应式的，当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会得到高效更新
- 不能直接改变store：不能直接改变store的变化，改变store中状态的唯一途径是commit mutation，方便于跟踪每一个状态的变化

### 6、vuex核心流程

![img](http://liangxinghua.com/uploads/image/20180709/1531106988.png)
**示意图说明：**

1. Vue Components：Vue组件。HTML页面上，负责接收用户操作等交互行为，执行dispatch方法触发对应action进行回应
2. Dispatch：操作行为触发方法，是唯一能执行action的方法
3. Actions：操作行为处理模块。负责处理Vue Components接收到的所有交互行为。包含同步/异步操作，支持多个同名方法，按照注册的顺序依次触发。向后台API请求的操作就在这个模块中进行，包括触发其他action以及提交mutation的操作。该模块提供了Promise的封装，以支持action的链式触发
4. Commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法
5. Mutations：状态改变操作方法。是Vuex修改state的唯一推荐方法，其他修改方式在严格模式下将会报错。该方法只能进行同步操作，且方法名只能全局唯一。操作之中会有一些hook暴露出来，以进行state的监控等
6. State：页面状态管理容器对象。集中存储Vue components中data对象的零散数据，全局唯一，以进行统一的状态管理。页面显示所需的数据从该对象中进行读取，利用Vue的细粒度数据响应机制来进行高效的状态更新
7. Getters：state对象读取方法。图中没有单独列出该模块，应该被包含在了render中，Vue Components通过该方法读取全局state对象

**总结说明：**
Vue组件接收交互行为，调用dispatch方法触发action相关处理，若页面状态需要改变，则调用commit方法提交mutation修改state，通过getters获取到state新值，重新渲染Vue Components，界面随之更新

## 入门示例

### 1、安装

```javascript
npm install vuex --save
```

### 2、简单示例

（1）src/vuex/store.js中写入以下代码：

```javascript
// 引入vue
import Vue from 'vue'
// 引入vuex
import Vuex from 'vuex'

// 使用vuex
Vue.use(Vuex)

// 1、state：创建初始化状态
const state = {
    // 放置初始状态
    count: 1
}

// 2、mutations：创建改变状态的方法
const mutations = {
    // 状态变更函数-一般大写
    ADD (state, n) {
        state.count += n;
    }
}

// 3、getters：提供外部获取state
const getters = {
    count: function(state){
        return state.count;
    }
}

// 4、actions：创建驱动方法改变mutations
const actions ={
    // 触发mutations中相应的方法-一般小写
    add ({commit}, data) {
        commit('ADD', data)
    }
}

// 5、全部注入Store中
const store = new Vuex.Store({
    state,
    mutations,
    getters,
    actions
});

// 6、输出store
export default store;
```

**代码说明：**

- state - mutations - getters - actions - store，以上写法基本固定。
- 小型项目用上面的简单管理状态即可。

（2）src/main.js代码中

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
// 引入store
import store from './vuex/store'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store, // 全局注入
  components: { App },
  template: '<App/>'
})
```

（3）src/compontent/Count.vue页面组件中代码如下：

```javascript
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <h2>{{count}}</h2>
        <button @click="clickAdd">新增</button>
    </div>
</template>
<script>
export default {
    data () {
        return {
            msg: 'Vuex test!'
        }
    },
    computed: {
        // 获取state值
        count() {
            return this.$store.state.count;
        }
    },
    methods: {
        clickAdd() {
            //分发action中的add方法
            this.$store.dispatch('add', 1);
        }
    }
}
</script>
<style scoped>

</style>
```

## state-状态对象的获取方法

### 1、在组件的template中直接使用

```javascript
<h2>{{ $store.state.count }}</h2>
```

### 2、在计算属性computed中直接赋值

```javascript
// 方式1：直接获取
computed: {
    count() {
        // this指的是main.js中的vue实例对象
        return this.$store.state.count;
    }
}
```

### 3、通过mapState的对象来赋值

```javascript
// 方式2：利用mapState
computed: mapState({
    // es5写法
    count: function (state) {
         return state.count;
     },
    // es6写法
    count: state => state.count
})
```

### 4、通过mapState的数组来赋值

```javascript
// 方式3：数组获取
computed: mapState(['count'])
```

### 5、通过mapState的JSON来赋值

```javascript
// 方式4：JSON获取
computed: mapState({
    count: 'count'
})
```

PS：一般4和5两种比较常用
**完整示例代码**

```javascript
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <h2>{{ $store.state.count }}</h2>
        <h2>{{count}}</h2>
        <button @click="clickAdd">新增</button>
    </div>
</template>
<script>
import {mapState} from 'vuex'
export default {
    data () {
        return {
            msg: 'Vuex test!'
        }
    },
    // 方式1：在计算属性computed中直接赋值
    // computed: {
    //     count() {
    //         // this指的是main.js中的vue实例对象
    //         return this.$store.state.count;
    //     }
    // },
    // 方式2：通过mapState的对象来赋值
    // computed: mapState({
    //     // es5
    //     // count: function (state) {
    //     //     return state.count;
    //     // },
    //     // es6
    //     count: state => state.count
    // }),
    // 方式3：通过mapState的对象来赋值
    // computed: mapState(['count']),
    // 方式4：通过mapState的JSON来赋值
    computed: mapState({
        count: 'count'
    }),
    methods: {
        clickAdd() {
            //分发action中的add方法
            this.$store.dispatch('add', 1);
        }
    }
}
</script>
<style scoped>

</style>
```

## mutations-getters-actions异步

### 1、mutations（修改状态）

（1）template中直接使用$store.commit( )触发

```javascript
// template
<button @click="$store.commit('ADD')">+</button>

// src/vuex/store.js
const mutations = {
    // 状态变更函数
    ADD (state) {
        state.count++;
    }
}
```

（2）利用mapMutations引入触发

```javascript
<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <h2>{{count}}</h2>
        <!-- 3、、直接调用相应的方法 -->
        <button @click="ADD">+</button>
    </div>
</template>
<script>
// 1、引入mapMutations
import {mapState, mapMutations} from 'vuex'
export default {
    data () {
        return {
            msg: 'Vuex test!'
        }
    },
    // 通过mapState的JSON来赋值
    computed: mapState({
        count: 'count'
    }),
    // 2、methods中加入mapMutations
    methods: mapMutations([
        'ADD'
    ])
}
</script>
<style scoped>

</style>
```

### 2、getters（获取state和过滤）

（1）基本用法

```javascript
// src/vuex/store.js
const getters = {
    count: function(state){
        // 返回加上100
        return state.count + 100;
    }
}
```

（2）常规获取值

```javascript
computed: {
    // 获取getters
    count(){
        return this.$store.getters.count;
    }
}
```

（3）mapGetters获取值

```javascript
// 1、引入mapMutations
import {mapState, mapMutations, mapGetters} from 'vuex'

// 2、使用
computed: {
    // 获取getters
    ...mapGetters(["count"])
}
```

### 3、actions（异步状态修改）

actions和mutations功能基本一样，不同点是，actions是异步的改变state状态，而mutations是同步改变状态。不过实际项目中一般都是通过actions改变mutations中的值。
（1）store.js中增加异步代码

```javascript
// src/vuex/store.js
const actions ={
    // 触发mutations中相应的方法
    add ({commit}) {
        // 增加异步
        setTimeout(()=>{
            commit('ADD')
        },3000);
        console.log('我比reduce提前执行');
    }
}
```

（2）常规使用

```javascript
// template
<button @click="add">+</button>

// script
methods: {
    add() {
        //分发action
        this.$store.dispatch('add');
    }
}
```

（3）mapActions的使用

```javascript
// template
<button @click="add">+</button>

// script
// 引入mapActions
import {mapState, mapActions} from 'vuex'

// 使用mapActions
methods: {
    ...mapActions(['add'])
}
```

## 传递参数

在vuex的方法调用用传递参数，只需要在mutations和actions相应的地方加上参数，然后调用的时候传入即可。
（1）src/vuex/store.js中

```javascript
// actions中传递参数
const mutations = {
    ADD (state, n) {
        state.count += n;
    }
}

// actions中传递参数
const actions ={
    // 触发mutations中相应的方法
    add ({commit}, n) {
        // 增加异步
        setTimeout(()=>{
            commit('ADD', n);
        },3000);
        console.log('我比reduce提前执行');
    }
}
```

（2）页面组件常规调用传递

```javascript
// template
<button @click="add">+</button>

// script
methods: {
    add() {
        // 分发action
        this.$store.dispatch('add', 99);
     }
}
```

（3）页面组件使用mapActions调用传递

```javascript
// template
<button @click="add(99)">+</button>

// script
methods: {
    ...mapActions(['add'])
}
```

## module-模块组

当应用非常复杂，状态非常多的时候，需要将store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块，从上至下进行同样方式的分割。

### 1、大致的结构

```javascript
// 模块A
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

// 模块B
const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

// 组装
const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

// 取值
store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

### 2、详细示例

实际开发中建议把module分开编写。
（1）src/vuex/module1.js

```javascript
// 模块1
const module1 = {
    // 初始化状态
    state: {
        module1: {
            name: '模块1'
        }
    },
    // 编写动作
    mutations: {
        CHANGE1 (state, data) {
            state.module1 = data;
        }
    },
    // 取值
    getters: {
        module1: function(state){
            return state.module1;
        }
    },
    // 创建驱动，可异步
    actions: {
        change1 ({commit}, data) {
            commit('CHANGE1', data)
        }
    }
}

export default module1;
```

（2）src/vuex/module2.js

```javascript
// 模块1
const module2 = {
    // 初始化状态
    state: {
        module2: {
            name: '模块2'
        }
    },
    // 编写动作
    mutations: {
        CHANGE2 (state, data) {
            state.module2 = data;
        }
    },
    // 取值
    getters: {
        module2: function(state){
            return state.module2;
        }
    },
    // 创建驱动，可异步
    actions: {
        change2 ({commit}, data) {
            commit('CHANGE2', data)
        }
    }
}

export default module2;
```

（3）src/vuex/store.js

```javascript
// 引入vue
import Vue from 'vue'
// 引入vuex
import Vuex from 'vuex'
// 引入module1
import module1 from '@/vuex/module1'
// 引入module2
import module2 from '@/vuex/module2'

// 使用vuex
Vue.use(Vuex)

// 模块注入
const store = new Vuex.Store({
    modules: {
        a: module1,
        b: module2
    }
})

// 输出store
export default store;
```

（4）组件中使用，src/compontent/one.vue

```javascript
<template>
    <div id="app">
        <!-- module1 -->
        <h2>{{ module1.name }}</h2>
        <button @click="change1({'name': 'change1'})">module1改变</button>

        <!-- module2 -->
        <h2>{{ module2.name }}</h2>
        <button @click="change2({'name': 'change2'})">module2改变</button>
    </div>
</template>

<script>
// 引入快捷方法
import {mapState, mapGetters, mapActions} from 'vuex'

export default {
    name: 'app',
    data () {
        return {
        }
    },
    computed:{
        // mapState取值
        // ...mapState({
        //     module1: state => state.a.module1.name,
        //     module2: state => state.b.module2.name
        // })

        // mapGetter取值
        ...mapGetters(['module1', 'module2'])
    },
    methods: {
        // mapAction取方法
        ...mapActions([
            'change1',
            'change2'
        ])
    }
}
</script>
<style>

</style>
```

PS：module中命名要唯一，不然获取值和改变值的时候会冲突，目前亲测mapGetters只能获取对象。



# AXIOS

## axios

[中文翻译手册](https://www.kancloud.cn/yunye/axios/234845)

### 1、简介

[官方文档](https://www.kancloud.cn/yunye/axios/234845)
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。简单来说就是前端最火最简单的一个http请求解决方案。

### 2、安装

```javascript
npm install vuex --save
```

### 3、功能

- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

### 4、代码封装

下面给出一份封装好的配置文件，更加详细的直接去官方文档查询即可。
（1）工具类封装

```javascript
// 引入axios
import axios from 'axios';

// 创建axios实例
const httpService = axios.create({
    // url前缀-'https://some-domain.com/api/'
    baseURL: process.env.BASE_API, // 需自定义
    // 请求超时时间
    timeout: 3000 // 需自定义
});

// request拦截器
httpService.interceptors.request.use(
    config => {
        // 根据条件加入token-安全携带
        if (true) { // 需自定义
            // 让每个请求携带token
            config.headers['User-Token'] = '';
        }
        return config;
    }, 
    error => {
        // 请求错误处理
        Promise.reject(error);
    }
)

// respone拦截器
httpService.interceptors.response.use(
    response => {
        // 统一处理状态
        const res = response.data;
        if (res.statuscode != 1) { // 需自定义
            // 返回异常
            return Promise.reject({
                status: res.statuscode,
                message: res.message
            });
        } else {
            return response.data;
        }
    },
    // 处理处理
    error => {
         if (error && error.response) {
            switch (error.response.status) {
                case 400:
                    error.message = '错误请求';
                    break;
                case 401:
                    error.message = '未授权，请重新登录';
                    break;
                case 403:
                    error.message = '拒绝访问';
                    break;
                case 404:
                    error.message = '请求错误,未找到该资源';
                    break;
                case 405:
                    error.message = '请求方法未允许';
                    break;
                case 408:
                    error.message = '请求超时';
                    break;
                case 500:
                    error.message = '服务器端出错';
                    break;
                case 501:
                    error.message = '网络未实现';
                    break;
                case 502:
                    error.message = '网络错误';
                    break;
                case 503:
                    error.message = '服务不可用';
                    break;
                case 504:
                    error.message = '网络超时';
                    break;
                case 505:
                    error.message = 'http版本不支持该请求';
                    break;
                default:
                    error.message = `未知错误${error.response.status}`;
            }
        } else {
            error.message = "连接到服务器失败";
        }
        return Promise.reject(error);
    }
)

/*网络请求部分*/

/*
 *  get请求
 *  url:请求地址
 *  params:参数
 * */
export function get(url, params = {}) {
    return new Promise((resolve, reject) => {
        httpService({
            url: url,
            method: 'get',
            params: params
        }).then(response => {
            resolve(response);
        }).catch(error => {
            reject(error);
        });
    });
}

/*
 *  post请求
 *  url:请求地址
 *  params:参数
 * */
export function post(url, params = {}) {
    return new Promise((resolve, reject) => {
        httpService({
            url: url,
            method: 'post',
            data: params
        }).then(response => {
            resolve(response);
        }).catch(error => {
            reject(error);
        });
    });
}

/*
 *  文件上传
 *  url:请求地址
 *  params:参数
 * */
export function fileUpload(url, params = {}) {
    return new Promise((resolve, reject) => {
        httpService({
            url: url,
            method: 'post',
            data: params,
            headers: { 'Content-Type': 'multipart/form-data' }
        }).then(response => {
            resolve(response);
        }).catch(error => {
            reject(error);
        });
    });
}

export default {
    get,
    post,
    fileUpload
}
```

（2）使用

```javascript
// 引入工具类-目录自定义
import fetch from '@/util/fetch'

// 使用
const TMPURL = ''; // url地址
const params = {}; // 参数
fetch.post(TMPURL + '/login/login', params);
```

