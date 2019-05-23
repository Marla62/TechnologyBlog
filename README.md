#### 2019年3月 [a.html](a.html) 24日

1. mpvue+vuex实现全局状态管理
2. mpvue+vuex本地小程序的搭建

#### 2019年3月25日

1. 微信小程序连接蓝牙设备(卡在搜索不到另外一台手机的蓝牙信息,iOS搜索安卓)
2. vue-router实现原理(三种路由模式)<https://segmentfault.com/a/1190000018584560>
3. westore 全局状态管理,以及父子组件传值

#### 2019年3月27日17

1. this的指向问题

2. 暂时性死区(let和块级作用域造成)

   > 只要块级作用域内存在let命令,它所声明的变量就"绑定"(binding)这个区域,不再受外部的影响

   ```javascript
   var tmp = 123;
   if (true) {
     tmp = 'abc'; // ReferenceError
     let tmp;
   }
   ```

3. for循环中,var和let表现不一样

   > 用var声明的变量 全局的存储单元中只有一个

   ```javascript
   var a = [];
   for (var i = 0; i < 10; i++) {
     a[i] = function () {
       console.log(i);
     };
   }
   a[6](); //10
   ```

4. 函数表达式和函数声明语句的区别

   ```javascript
   // 函数声明语句
   {
     let a = 'secret';
     function f() {
       return a;
     }
   }
   
   // 函数表达式
   {
     let a = 'secret';
     let f = function () {
       return a;
     };
   }
   ```

   

#### 2019年3月28日

1. this的指向性问题

   全局环境中，this会返回顶层对象。但是，Node 模块和 ES6 模块中，this返回的是当前模块。
   函数里面的this，如果函数不是作为对象的方法运行，而是单纯作为函数运行，this会指向顶层对象。但是，严格模式下，这时this会返回undefined。
   不管是严格模式，还是普通模式，new Function('return this')()，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全策略），那么eval、new Function这些方法都可能无法使用。

2. 数组遍历问题

   ```javascript
       let arr = [1,2,3];
       arr[-2] = -2;
       arr['a'] = 'a';
       arr[12] = 12;
       console.log(arr)
       console.log(arr.length)
   ```

   #### 2019年3月29日

   1. 搭建毕业设计admin环境,express+node+MongoDB
   2. 字符串扩展 padStart和padEnd在头部或尾部将字符串补全长度
   3. 本地配置json-server

#### 2019年4月15日11:21:45

```
let order =[];
let obj = {'a':2}
order.push(obj); 正确执行
order = order.push(obj);报错
```

#### 2019年5月14日15:56:42

1. 了解到vuex的定义

vuex和单纯的全局对象不同,vuex的存储是响应式的,当vue组件从store中读取状态的时候,若store的状态发生变化,那么相应的组件也会得到高效的更新

westore变相的模仿了这一机制。小程序改变数据后想要同步到view层,必须使用this.setData(); 而westore声明的全局变量，在状态发生变化后，调用this.update(),其实是隐式的调用了this.setDdata().从而将数据的变化更新到view层。

1. 学习使用vuex

参考链接<<https://blog.csdn.net/sps900608/article/details/79534231>>

vue-router的路由钩子使用方法

注意:在vue-router中访问不到vue实例,解决办法,将判断登录的路由写到main中

####2019年5月15日23:38:33

安装并配置python环境

1. 下载EXE安装包,安装时勾选add path,以添加到全局变量(系统用户变量中添加python路径)
2. 打开DOS界面,键入python,启动解释器
3. print 'hello world' 回车
4. 退出python提示符  使用**Ctrl-z**再按**Enter**

#### 2019年5月16日16:54:32

1. 了解到nodejs是用来做什么的?有什么优势

   参考链接<https://www.zhihu.com/question/33578075>

   nodejs是一个JavaScript的运行环境。拥有非阻塞，事件驱动的特性。并且跑在Google的变态V8引擎上。

- 什么是非阻塞？什么是事件驱动？（对比阿帕奇来看）

  > 我们再看传统的服务器（比如Apache）。每次一个新用户连到你的网站上，你的服务器就得开一个连接。每个连接都需要占一个进程，这些进程大部分时间都是闲着的（比如等着你好友发新鲜事，等好友发完才给用户响应信息。或者等着数据库返回查询结果什么的）。虽然这些进程闲着，但是照样占用内存。这意味着，如果用户连接数的增长到一定规模，你服务器没准就要耗光内存直接瘫了。

  1. 可以把非阻塞的服务器想象成一个loop循环,这个loop会一直跑下去,一个新请求过来了,这个loop就接住了这个请求,把这个请求传给其他的进程（比如传给一个搞数据库查询的进程），然后响应一个回调（callback）。

  2. 服务器只在用户那边有事件发生的时候才响应，这就是**事件驱动。**

     如果数据库把结果返回来了，loop就把结果传回用户的浏览器，接着继续跑。在这种方式下，你的服务器的进程就不会闲着等着。从而在理论上说，同一时刻的数据库查询数量，以及用户的请求数量就没有限制了。

#### 2019年5月17日15:32:22

微信小程序在PC端,获取当前经纬度会存在极大误差,调试慎用动态获取的经纬度

根据经纬度 计算两点距离

```javascript
const distanceBetweenPoints = function (lat1, lng1, lat2, lng2) {
  if (!lat2 || !lng2 || !lat1 || !lng1) {
    return 0;
  }
  var pk = (180 / 3.14169)
  var a1 = lat1 / pk
  var a2 = lng1 / pk
  var b1 = lat2 / pk
  var b2 = lng2 / pk
  var t1 = Math.cos(a1) * Math.cos(a2) * Math.cos(b1) * Math.cos(b2)
  var t2 = Math.cos(a1) * Math.sin(a2) * Math.cos(b1) * Math.sin(b2)
  var t3 = Math.sin(a1) * Math.sin(b1)
  var tt = Math.acos(t1 + t2 + t3)
  return (6366000 * tt)
}
```

#### 2019年5月19日20:12:10

vue 中跳转方式

```
this.$router.push({
  path: `/name`,
  query:{name:value}
})

this.$router.push({
  name: `name`,
  params:{name:value}
})
```

restful API 规范

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
- DELETE（DELETE）：从服务器删除资源。

mongoose修改一条数据

typeModel.update({条件},{主题内容},callbck())

####2019年5月20日12:45:56

获取当前路由,在vue中 this.$route.path

小程序高度自适应 给image标签加 model="widthFix"

####2019年5月21日10:48:53

- webstorm自带的postman工具:tools->http client
- 对象的属性名是变量的写法

```
let a = 12;
let obj={};
obj[a] = 1;//{12:1}
```

####2019年5月22日16:09:52

- 发现forEach循环中不能使用 continue,


- ![1558515967145](C:\Users\Administrator.SC-201904161959\AppData\Roaming\Typora\typora-user-images\1558515967145.png)

  以上代码并不是同步执行;console.log()是同步还是异步的问题?

  ```javascript
  let obj ={id:111,per:{name:"Tom",age:26}};
  console.log(obj.per);console.log(obj.per.name);obj.per.name ='jack';
  //点开obj.per后,name是修改过的值,在展开对象是js引擎会重新获取该对象的值。展开对象时，它其实是重新去内存中读取对象的属性值，所以当我们展开对象后看到的name属性值是Jack
  ```

  js中对象是引用类型,每次使用对象,都只是使用了对象在堆中的引用。

  参考链接<https://github.com/Mmzer/think/issues/30>

- 除了六大基本类型,其余都是引用类型 如obj和数组

#### 2019年5月23日21:20:19

小程序中 scroll-view便签无法设置display:flex属性,要实现横向滚动,这种写法是正确的

```
<scroll-view> /*固定宽度和屏幕等宽*/
	<view style="display:flex">/*要超过滚动容器的宽度*/
		view*8
	</view>
</scroll-view>
```

