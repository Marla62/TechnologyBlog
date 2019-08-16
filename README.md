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

#### 2019年5月24日09:28:04

- css3 的变量 variable, 小程序可兼容

  > 可以实现的功能有 换主题色

- 参考链接 ： http://www.cnblogs.com/xiaohuochai/p/7182771.html

- <https://www.ruanyifeng.com/blog/2017/05/css-variables.html>

1. 在根节点声明的变量可以全局使用;子节点可以读取父节点的变量值。

   ```css
   /**var()函数可以用在变量的声明;注意，变量值只能用作属性值，不能用作属性名。*/
   :root {
     --primary-color: red;
     --logo-text: var( --primary-color );
   }
   ```

2. 声明用 :root{--name:value} 格式 {--变量名:变量值}

3. 读取用 :var(--name,默认值)格式 var函数第二个参数支持传入默认值,在变量为undifined时 

```wxml

/*小程序使用css变量demo*/
<view style='--bgColor:#000000;'>
    <view style='width:200rpx;height:200px;background:var(--bgColor,pink)'>

    </view>
</view>
```

- 小程序检测不到 ::after ::before,但是不影响使用

#### 2019年5月25日16:33:23

- 小程序的多列选择 组件

- 小程序比较好的setData方式

  ```javascript
  //小程序比较好的setData方式
    var data = {a: this.data.a,b: this.data.b};
    data.a += 12;
    data.b += 13;
    this.setData(data)
  //另一种setdata()
  this.setData({
      a:this.data.a+=12, 
      b:this.data.b+=13
  })
  //westore的写法 1
  store.data.a += 12;
  store.data.b += 13;
  this.update()
  //westore的写法 2
  this.update({
      a:store.data.a+=12,
      b:store.data.b+=13
  })
  ```

- 预约取餐时间算法(仅支持时间间隔对60取余为0的间隔)

  ```javascript
      /**
       * 预约时间算法
       * @param bet 时间间隔 20
       * @param sh 从8点开始营业 8
       * @param eh 晚上十点关门 10
       * @param m 从25分钟开始 25
       * @returns {Array}
       */
      getTimeArrByTime: function (bet, sh, eh, m) {
             if (bet % 60 != 0){
        console.log("仅支持时间间隔对60取余后等于0的时间间隔")
        return false;
      } 
        let timeArr = [];
        let tmpSh = sh;
        //传入开始营业时间和结束营业时间,得到['08:00',08:20]数组
        for (sh; sh <= eh; sh++) {
          for (let j = 0; j < 60 / bet; j++) {
            let str = '';
  
            if (sh < 10) {
              if (bet * j < 10) {
                str = '0' + sh + ':' + '0' + bet * j
              } else {
                str = '0' + sh + ':' + bet * j
              }
            } else {
              if (bet * j < 10) {
                str = sh + ':' + '0' + bet * j
              } else {
                str = sh + ':' + bet * j
              }
            }
            if (sh == tmpSh && bet * j < m) {
              continue
            }
            timeArr.push(str);
  
            if (sh >= 22) {
              break
            }
          }
        }
        return timeArr;
      },
  ```

  #### 2019年5月28日16:06:15

- css3旋转属性

  transform-origin:center; 定义旋转中心

  transform:rotate(30deg); 旋转30度

- `bind`事件绑定 不会阻止冒泡事件向上冒泡，`catch`事件绑定可以阻止冒泡事件向上冒泡。

- 微信小程序获取用户信息流程

  1. wx.login()方法会返回code,把code交给后台,后台用code换用户信息(可拿到unid,opid)

#### 2019年5月29日20:10:30

mongoose的CRUD (挖坑)

```javascript
//删除指定数据
var conditions = {_id: req.params.id};
orderModel.remove(conditions, function (error) {
  if (error) {
    console.error(error);
  } else {
    res.send({errorCode:0})
  }
});
//查询所有数据

```

#### 2019年5月30日15:03:15

小程序tabBar跳转(不可行)

```
app.json
{
  "tabBar": {
    "list": [{
      "pagePath": "index",
      "text": "首页"
    },{
      "pagePath": "other",
      "text": "其他"
    }]
  }
}
//跳转方法
wx.switchTab({
  url: '/index'
})
```

#### 2019年6月5日21:11:45

- .find方法 会返回符合条件的value

```javascript
let arr = [{key:1,value:a},{key:2,value:b}]
let res = arr.find(item=>item.key==1)
console.log(res)
//res = {key:1,value:a}
```

- js中的栈

> Javascript中对函数进行排序 是利用栈来实现的
>
> 如果栈中存储的东西过多会造成栈的溢出
>
> 当函数return的时候 这个函数才会在堆中被踢出

- jsonwebtoken

npm一个可以发自动生成token的npm包

<https://github.com/auth0/node-jsonwebtoken>

#### 2019年6月6日20:01:10

- 通过后台生成的二维码跳转到小程序的某个页面,并在onLoad读取参数

```javascript
//二维码链接 https://apitest.xiucan.com/table/1234124
//    
var scene = decodeURIComponent(prame.q);
console.log(scene);
//apitest.xiucan.com/table/1234124
```

#### 2019年6月8日10:28:26

- 数组中两个元素互换位置,两种方式

  ```javascript
  //解构赋值
  [arr[1],arr[2]]= [arr[2],arr[1]]
  ```

  ```javascript
  array.splice(index2,1,...array.splice(index1, 1 , array[index2]));
  //splice会返回被影响的元素
  ```

  array.splice(index1, 1 , array[index2])会将index1位置上的元素替换为index2位置的元素，同时返回[array[index1]]（注意此时返回的是数组，所以在代码中加入了扩展运算符...将数组转为参数序列）。再利用同样的方式将index2位置上的元素替换为被删除的原数组的array[index1]的值。完成交换。

#### 2019年6月11日21:27:25

git 拉取master以外分支的代码

```
先 将master git clone 到本地
再cd 文件夹目录
git branch -r 或者 git branch -a 查看远端分支
git checkout origin/dev(分支名)
成功检出新分支。
```

#### 2019年6月14日18:53

- async函数和Generator函数  
> ES2017 标准引入了 async 函数，使得异步操作变得更加方便。
> async 函数是什么？一句话，它就是 Generator 函数的语法糖  

function*(){} 函数，叫Generator一种用来处理异步的函数，用*声明的异步任务，在yield之后，
必须手动 .next()才会往下执行，它的执行过程是可控的;  
async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已。
```javascript
const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
//async函数  
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
async函数是对 Generator 函数的改进。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。

#### 20190615 10点06分

- web worker

> web worker是现代浏览器提供的一个JavaScript多线程解决方案。完全受主线程控制，并且不能操作dom

实例化一个worker 

```javascript
const worker = new Worker('worker.js')
```

worker的构造函数的参数是一个脚本文件，该文件就是worker线程要执行的文件，worker脚本必须来自网络。

- 可以用来做什么

  1.我们可以用 `Web Worker` 做一些大计算量的操作；
  2.可以实现轮询，改变某些状态；
  3.页头消息状态更新，比如页头的消息个数通知；
  4.高频用户交互，拼写检查，譬如：根据用户的输入习惯、历史记录以及缓存等信息来协助用户完成输入的纠错、校正功能等
  5.加密：加密有时候会非常地耗时，特别是如果当你需要经常加密很多数据的时候（比如，发往服务器前加密数据）。
  6.预取数据：为了优化网站或者网络应用及提升数据加载时间，你可以使用 `Workers` 来提前加载部分数据以备不时之需。

- 使用方法

  1. 使用postMessage向worker发送消息。参数可以是任何数据类型。

  ```javascript
  worker.postMessage('Hello World');
  worker.postMessage({method: 'echo', args: ['Work']});
  ```

  主线程使用onmessage监听事件返回

  ```javascript
  worker.onmessage = function (event) {
    console.log('Received message ' + event.data);
    doSomething();
  }
  
  worker.terminate();//完成任务后关闭
  ```

主线程和worker之间通信，传递信息是 在 拷贝信息 而不是拷贝地址，所以worker中改变数值，不会影响主线程

##编写优雅的代码  
##### promise解决回调嵌套问题  
###### 避免.then嵌套过多的问题  
```javascript
this.axios().then(res=>{
    //do someing
    retuen new Promise(resolve=>{reolve(res)})
    //返回一个promise对象,用来进行.then的链式操作
}).then(res=>{
    
})
```
##### 方法的结构赋值  
```javascript
//解构接参(注意:!!对象中的键名需要对应
let fn = function({a,b,c,d,e}){
    console.log(a,b,c,d,e);
    //10 20 80 40 30
}
//调用fn
fn({a:10,b:20,e:30,d:40,c:80})
//使用此方法  即使传参时传入顺序不同,也不会影响参数的接收

//普通函数
let fn1 = function(a,b,c,d,e){
    console.log(a,b,c,d,e);
    //10,20,30,40,80
}
//调用fn1
fn1(10,20,30,40,80)
```
###### 简化判断条件
```javascript
// 之前的写法
let errorcode;//errorcode是个变量
if (errorcode== 12012 ||errorcode== 12023 ||errorcode== 12011||errorcode== 12062||errorcode== 12222) {
  //do something
}
//优化后的写法 利用数组的includes方法
let errorArray = [12012,12023,12011,12062,12222];//可能会出现的code码都可以写进去
if (errorArray.includes(errorcode)) {
  //do something
}
```
#### !!注意
对对象赋值不要用null;
let obj = null;
obj.a = 1;//报错
obj = {};
obj.a =1;//正确  

#### 指针问题 

## 支付宝的坑

1. 关于支付宝的点击事件

   支付宝的点击事件总共有三个默认参数 click(e,a,b) 

   e是事件对象,a和b 分别是一个空方法,不知道干嘛用的

1. 当要将一个函数作为一个函数的参数时,不要加(),加了括号返回的是函数的返回值  

##微信小程序的坑
1. 小程序渲染层内核不一致导致某些api不可用  
  > 数组的扁平化方法 Array.prototype.flat() 不支持  
  解决: 重新定义此方法到数组对象的原型链上
  ```javascript  
    /**
     * 2019年8月16日
     * 给数组添加flat方法
     */
    addMethodToArray() {
      Array.prototype.flat = function(depth) {
        // let arr = this.value; //读不到
        let arr = this; //数组本身
        let res = [],
          depthArg = depth || 1,
          depthNum = 0,
          flatMap = arr => {
            arr.map((element, index, array) => {
              if (Object.prototype.toString.call(element).slice(8, -1) === 'Array') {
                if (depthNum < depthArg) {
                  depthNum++;
                  flatMap(element);
                } else {
                  res.push(element);
                }
              } else {
                res.push(element);
                if (index === array.length - 1) depthNum = 0;
              }
            });
          };
        flatMap(arr);
        return res;
      };
    },

  ```