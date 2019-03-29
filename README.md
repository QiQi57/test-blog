## 箭头函数

箭头函数：语法简洁，替代匿名函数，不能用作构造函数，没有this，arguments，super，new.target

1.箭头函数 通过call／apply 调用时，第一个context参数会被忽略，只接收后面参数。

2.不绑定arguments

3.像函数一样使用箭头函数
'use strict';
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log( this.i, this)
  }
}
obj.b(); 
// undefined
obj.c(); 
// 10, Object {...}

4.箭头函数不能用作构造器，和 new一起用会抛出错误


5.箭头函数没有prototype属性。

6.yield 关键字通常不能在箭头函数中使用（除非是嵌套在允许使用的函数内）。因此，箭头函数不能用作生成器。

var func = () => ({foo: 1});

7.箭头函数在参数和箭头之间不能换行。
8.箭头函数 不绑定this（es5中每个函数都有this），它只会从自己的作用域链的上一层继承this
9.具体使用语法。。。

es5中this问题：

```
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    //  回调引用的是`that`变量, 其值是预期的对象. 
    that.age++;
    console.log(that.age);
  }, 1000);
}
```

箭头函数解决this指向的问题
```
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| 正确地指向 p 实例
    console.log(this.age);
  }, 1000);
}

var p = new Person();
```

## this
this 知识点：
1.严格模式下（非全局环境）this 的值是undefined；无论是否在严格模式下，在全局执行环境中this都指向全局对象。
2.在函数内部，大多数情况下，函数的调用方式决定了this的值；可以使用bind方法来设置函数的this值，而不用考虑函数如何被调用；
es2015 引入了支持this词法解析的箭头函数。
sample1:
function f2(){
  "use strict"; // 这里是严格模式
  return this;
}

f2() === undefined; // true
window.f2()===window //true

3.使用apply／call，把this的值从一个环境传到另一个
// 将一个对象作为call和apply的第一个参数，this会被绑定到这个对象。
var obj = {a: 'Custom'};

// 这个属性是在global对象定义的。
var a = 'Global';

function whatsThis(arg) {
  return this.a;  // this的值取决于函数的调用方式
}

whatsThis();          // 'Global'
whatsThis.call(obj);  // 'Custom'
whatsThis.apply(obj); // 'Custom'

4.bind 方法：调用f.bind(someObject)会创建一个与f具有相同函数体和作用域的函数，但是在这个新函数中，
this将永久地被绑定到了bind的第一个参数，无论这个函数是如何被调用的。

function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var h = g.bind({a:'yoo'}); // bind只生效一次！
console.log(h()); // azerty

5.在箭头函数中，this与封闭词法环境的this保持一致（foo 的 this 被设置为他被创建时的环境）。
在全局代码中，它将被设置为全局对象：
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true

在函数内创建的箭头函数：
var obj = {
  bar: function() {
    var x = (() => this);
    return x;
  }
};
// 作为obj对象的一个方法来调用bar，把它的this绑定到obj。
// 将返回的函数的引用赋值给fn。
var fn = obj.bar();
console.log(fn() === obj); // true

var fn2 = obj.bar;
// 那么调用箭头函数后，this指向window，因为它从 bar 继承了this。
console.log(fn2()() == window); // true

6.作为对象的方法：
当函数作为对象的方法被调用时，this指向调用该函数的对象。
注意：最靠近的引用才是最重要的。
var o = {prop: 37};

function independent() {
  return this.prop;
}

o.f = independent;

console.log(o.f()); // logs 37

o.b = {g: independent, prop: 42};
console.log(o.b.g()); // 42

7.作为构造函数:
当一个函数用作构造函数时（使用new关键字），它的this被绑定到正在构造的新对象。

8.作为一个dom事件处理函数
this指向触发事件的dom元素

9.作为一个内联事件处理函数
this 指向监听器所在的dom元素



## promise
Promise 
异步编程解决方案
1.三种状态
pending/fulfilled/rejected

pending -> fulfilled  
pending -> rejected

 new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value); //
  } else {
    reject(error);
  }
});

异步操作成功时调用resolve
异步操作失败，调用reject

2. 可以用then方法分别指定resolved状态和rejected状态的回调函数。
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});

3.catch 用于指定发生错误时的回调函数
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});

4.finally：不管promise对象最后状态如何，都会执行的操作
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});

5. promise.all
let p=promise.all([p1,p2]) 
p的状态有p1和p2决定：
p1,p2都是fulfilled，p状态会变成fulfilled
p1,p2中有一个被rejected，p状态会变成rejected
 
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});

6. promise.race
const p = Promise.race([p1, p2, p3]);

p1,p2,p3有一个状态改变，p的状态就会跟着改变。


7.promise.resolve()
const p=Promise.resolve('done');
等同于
const p=new Promise((resolve,reject)=>resolve('done'))

8.promise.reject()
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))


## async 函数返回一个promise对象，可以使用then添加回调函数。
当函数执行的时候，遇到await就会先返回，等待异步操作完成，再接着执行函数体内后面的语句。

async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

函数是对Generator函数的改进，和generator对比
1.内置执行期
2.更好的语义
async   对应 *
await   对应 yield
3.更广的适应性
yield后面只能是Thunk函数或promise对象，而async函数的await后面可以是promise对象或原始类型的值（会自动转成promise对象）

4.返回值是promise
async 函数返回的是promise对象，generator返回的是Iterator对象

async function f() {
  await new Promise(function (resolve, reject) {
    throw new Error('出错了');
  });
}

f()
.then(v => console.log(v))
.catch(e => console.log(e))
// Error：出错了

注意点：
1.await 后面是promise对象，可能会返回rejected，错误异常处理

async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法

async function myFunction() {
  await somethingThatReturnsAPromise()
  .catch(function (err) {
    console.log(err);
  });
}

2.多个异步操作，同时触发
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;


3.await命令只能用在async函数之中，如果用在普通函数，就会报错

async 函数与 promise，generator 函数的比较
async函数 的实现最简洁，最符合语义，几乎没有语义不相关的代码。


## es6 vs es5
es5继承：
先创建子类，然后把父类的方法添加到子类的原型上，

function people(){
    this.name="this is default name";
    this.age="18";
}
people.prototype.getName=function(){
    return this.name;
}

function student(){
    this.school="";
}
student.prototype=new people();
student.constructor=student;
var xiaoMing=new student();
xiaoMing.getName();

es6
先将父类的方法添加到this，然后在this基础上添加方法
class people{
    constructor(){
        this.name="this is default name";
    }
    getName(){
        return this.name;
    }
}

class student extends people{
    constructor(prop){
        super(prop);
        this.school="middle school";
    }
}
var xHong=new student();
xHong.getName();






