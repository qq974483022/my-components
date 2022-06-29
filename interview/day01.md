#####  1.根据你的理解,请简述 JavaScript 脚本的执行原理
         
#####  2.什么是事件冒泡，它是如何工作的？如何阻止事件冒泡?
       事件冒泡是js事件的一种执行时机：执行在冒泡阶段
       当点击事件发生时会一层一层的向目标dom靠近，然后在向上层一层一层的冒泡
#####  3.typeof 和 instanceof 区别
       typeof判断所有变量的类型，返回值有number、string、boolean、function、object、undefined
       typeof对于丰富的对象实例，只能返回object，导致有时候得不到真实的数据类型。
       instanceof用来判断对象，代码形式（obj1 instanceof obj2）（判断obj1是否为obj2的实例），obj2必须为对象，否则会报错。返回的是布尔值。
#####  4.说说你对原型（prototype）理解
        每个函数对象都有一个显示原型对象(prototype)
        每个实例对象都有个隐式原型对象(__proto__)
        当查找属性时才会沿着原型链向上查找，先在自身查找，如果没有找到就沿着原型链查找
#####  5.Call 和 apply，bind 的区别
        共同点：都是用来改变this指向
        不同点：1、执行的时机不同call会立即执行，apply和bind是返回一个函数
               2、传参方式不同 call(this,a1,a2...)/apply(this,a1,a2....)   bind(this,[a1,a2...])
#####  6.var、let、const 之间的区别
        相同点：都是用来定义变量的 
        不同点：1、var 存在变量提升 2、let 和const 不存在变量提升 3、let,一般用来定义变量,const一般用来定义常量
#####  7.使用箭头函数应注意什么/箭头函数和普通函数的区别
        区别：箭头函数不存在this指向的问题，普通函数考虑this指向
        注意点：
#####  8.Promise 有几种状态，什么时候会进入 catch
         两种状态：1、pending--->resolve / pending---->reject
         出现异常会进入catch
#####  9.Object.defineProperty 和 Proxy 的区别 vue双向绑定原理
        vue双向绑定原理： vue数据的双向绑定是通过数据劫持结合发布者-订阅者模式的方式来实现的。其核心就是通过Object.defineProperty()方法设置set和get函数来实现数据的劫持，在数据变化时发布消息给订阅者，触发相应的监听回调。也就是说数据和视图同步，数据发生变化，视图跟着变化，视图变化，数据也随之发生改变；
        Object.defineProperty 和 Proxy 的区别：1、Proxy代理整个对象，Object.defineProperty定义对象上的某个属性。 因此对象上定义新属性时，Proxy可以监听到，Object.defineProperty监听不到。2、Proxy可以监听到数组变更，Object.defineProperty监听不到。
#####  10.Vue-router 是干什么的，原理是什么
        Vue-router 是用于组织SPA应用的页面的映射和跳转
        分为哈希模式和history模式
        哈希模式的原理是 ：url后面会有# ，根据 window.location.hash 来实现，window.onhashchange 可以监听到哈希值的变化
        history的原理是：  h5 提供了 historyApi来实现url的变化，主要是 pushState()和replaceState() 



##### 1、js 为什么是单线程
    如果js是多线程的话，一个线程添加DOM一个线程删除DOM那就会产生差错，浏览器不知道应该听谁的，到底删除DOM还是添加DOM。

    js是单线程语言，浏览器只分配给js一个主线程，用来执行任务（函数），但一次只能执行一个任务，这些任务形成一个任务队列排队等候执行，但前端的某些任务是非常耗时的，比如网络请求，定时器和事件监听，如果让他们和别的任务一样，都老老实实的排队等待执行的话，执行效率会非常的低，甚至导致页面的假死。所以，浏览器为这些耗时任务开辟了另外的线程，主要包括http请求线程，浏览器定时触发器，浏览器事件触发线程，这些任务是异步的。
##### 2、谈谈你对闭包的理解
    闭包就是能够读取其他函数内部变量的函数。例如在 javascript 中，只有函数内部的子
    函数才能读取局部变量，所以闭包可以理解成“定义在一个函数内部的函数“。在本质上，闭包是将函数内部和函数外部连接起来的桥梁。
##### 3、你在项目中是如何封装axios的
    1.在untils下创建request.js文件
    2.在request文件中引入axios，baseURl代表后端服务器地址，timeout便是接口的响应时间,超出退出
    3.增加headers传入token
    4.创建拦截器
##### 4、for in 和 for of 有什么区别 
    1.for in 和 for of 都可以循环数组，for in 输出的是数组的index下标，而for of 输出的是数组的每一项的值。
    2.for in 可以遍历对象，for of 不能遍历对象，只能遍历带有iterator接口的，例如Set,Map,String,Array

##### 5、谈谈你对执行栈和执行上下文的理解
    https://blog.csdn.net/qq_41884544/article/details/114821041

##### 6、js事件循环
    虽然说js是一个单线程执行，但是还是分为同步和异步，也可以说为主线程，和异步线程，那么如果在异步线程中，任务队列绝对起到绝对重要的作用，同步任务一般都会在主线程去执行，执行完之后，再去执行异步任务，也就是在异步线程，那么这时候异步任务就可以进入任务队列去执行异步任务，上诉不断地过程，让我们叫做循环事件（Event loop）
##### 7、在项目中令你深刻的难题
    vue中dom刷新
##### 8、 实现一个 凹凸 
    https://www.freesion.com/article/777064453/
##### 9、元素垂直居中的几种方式
    1. flex + align-items + justify-content
    2. <div>position:relative; <div>position:absolute;top:0;bottom:0;margin:auto;</div></div>
    3.  首先，设置块级元素<div>的父元素的 position:relative；
        其次，设置块级元素<div>的css为：position:absolute;top:50%;left:50%;
        最后，设置该块级元素<div>的transform：transform:translate(-50%,50%);
##### 10、什么是BFC
    块级格式化上下文