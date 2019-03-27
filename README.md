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






