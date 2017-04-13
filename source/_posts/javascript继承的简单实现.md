---
title: javascript继承的简单实现
date: 2017-04-06 11:03:00
tags:
- javascript
- 面向对象
- 前端
---
JavaScript作为一门语法比较松散的语言，在ES6之前并没有像C++/Java等传统OO语言一样有class关键字，也不能通过private，public等关键字来限定权限。本篇就介绍一下JavaScript是如何实现继承的。

JavaScript的继承可以分为两类：
* 基于对象的继承
* 基于类型的继承

## 基于对象的继承
基于对象的继承也叫原型继承。我们知道通过JavaScript字面量创建的对象都会连接到 Object.prototype ，因此我们用Object.prototype来实现继承。本质上是摒弃类，不调用构造函数，而是用``Object.create()``，直接让新对象继承旧对象的属性。例如：
```js
var person = {
    name: "Jack",
    getName: function () { return this.name; }
}
var p1 = Object.create(person);
console.log(p1.getName());    //Jack
```
代码很简单，person有一个属性和一个方法。对象p1通过Object.create()来继承，第一个参数prototype指向person的prototype，这样对象p1就继承了person的属性和方法。

Object.create()还可以指定第二个参数，即数据属性，将其添加到新对象中。数据属性可设4个描述符value， writable，enumerable，configurable 。后3个看名字也能猜出意思，不指定的话默认为false。因为和本篇关系不大，就不跑题了，只看看设置value的情况：
```js
var p2 = Object.create(person, {
    name: {
        value: "Zhang"
    }
});
console.log(p2.getName());    //Zhang
```
用Object.create()相当于创建了一个全新的对象，你可以给该对象任意新增，重载它的属性和方法：
```js
var person = {
    name: "Jack",
    getName: function () { return this.name; },
    getAge: function() { return this.age; } //注意并没有age这个成员变量，依赖子类实现
}

var p3 = Object.create(person);
p3.name = 'Rose';
p3.age = 17;
p3.location = '上海';
p3.getLocation = function() { return this.location; }

console.log(p3.getName());    //Rose
console.log(p3.getAge());     //17
console.log(p3.getLocation());    //上海
```
在person中并没有age这个属性，因此你调用person.getAge();将得到undefined。但在对象p3里新定义了age这个属性，于是就能正确地调用基类的getAge方法。另外子类重载了name的值，且新定义了location属性和getLocation方法。结果如上所示，不赘述。

## 基于类型的继承
基于类型的继承是通过构造函数依赖于原型的继承，而非依赖于对象。例如：
```js
function Person(name) {
    this.name = name;
    this.getName = function () { return this.name; };  
}
function Student(name, age) {
    Person.call(this, name);
    this.age = age;
    this.getAge = function () { return this.age; };
}
Student.prototype = new Person();    //需要通过new来访问基类的构造函数

var p = new Person('Cathy');
var s = new Student('Bill', 23);

console.log(p.getName());    //Cathy
console.log(s.getName());    //Bill
console.log(s.getAge());     //23
```
Student继承自Person。name虽然是在基类Person里被定义的，但用new调用Person的构造函数后，this将被绑定到子类Student对象上，因此name最终是定义在子类Student对象上的。结果如上所示，不赘述。

## 保护隐私

之所以定义getName，getAge等方法就是不想让用户直接访问name，age等属性。可惜上面两种继承均无法保护隐私，均可像p.name，p.age这样直接访问属性。如果认为这些属性的隐私非常重要，希望模拟出OO语言中private属性的效果，可以用函数模块化。

所谓函数模块化，本质上就是在函数内新建一个对象，新对象的方法里使用参数对象的属性，然后将新对象返回。此时新对象里是没有参数对象的属性的，达到了保护隐私的目的。代码如下：

```js
var person = function(spec) {
    var that = {};        //新对象
    that.getName = function () { return spec.name; };  //使用参数的属性
    that.getAge = function() { return spec.age; };  //使用参数的属性
    return that;        //返回新对象
}

var p4 = person({name: 'Jane', age: 20});

console.log(p4.name);    //undefined
console.log(p4.age);     //undefined
console.log(p4.getName());    //Jane
console.log(p4.getAge());     //20
```

进一步实现多层继承也非常方便，效果如下，不赘述：
```js
var student = function(spec) {
    var that = person(spec);        //新对象继承自person
    that.getRole = function() { return 'student'; };  //新对象增加方法
    that.getInfo = function() {
        return spec.name + ' ' + spec.age + ' ' + that.getRole();
    };
    return that;    //返回新对象
};

var p5 = student({name:'Andy', age:12});

console.log(p5.name);       //undefined
console.log(p5.getName());  //Andy
console.log(p5.getRole());  //student
console.log(p5.getInfo());  //Andy 12 student
```
