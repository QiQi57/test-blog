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



