---
title: javascript中继承的几种实现方法
tags:
  - 前端篇
  - JavaScript
date: 2022-07-22 16:45:21
---




#### 继承

在javacript继承很是重要,通过继承可以实现代码的复用,让子类继承父类从而减少代码量,记下来使用多个方法来实现继承.

##### 1.原型链的继承

原型链继承存在的问题:

- 原型中包含的引用类型属性将被所有实例共享,某一个实例改变属性其他的实例属性也会跟着改变
- 子类在实例化的时候不能给父类构造函数传参

~~~javascript
function Animal() {
  this.color = ['yellow', 'black'];
}
Animal.prototype.getColor = function () {
  console.log(this.color)
}
function Dog() { }
Dog.prototype = new Animal();
let dog = new Dog();
dog.getColor(); // ["yellow","black"]
dog.color.push('white');
let dog2 = new Dog();
dog2.getColor(); // ["yellow","black","white"]
~~~

##### 2.借用构造函数实现继承

借用构造函数继承解决了原型链继承的2个问题：引用类型共享问题以及传参问题。但是由于方法定义在构造函数中，所以会导致创建子类实例都会创建一遍方法

~~~javascript
function Animal(name) {
  this.name = name;
  this.getName = function () {
    console.log(this.name);
  }
}
function Cat(name) {
  Animal.call(this, name)
}
Cat.prototype = new Animal();
let cat = new Cat('cat');
cat.getName()
~~~

##### 3.组合继承

组合继承集合了原型链和盗用构造函数，将两者的优点集中起来，基本的思路是使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性，这样既可以把方法定义在原型上，实现重用，又可以让每个实例都有自己的属性。

~~~javascript
function Animal(name) {
  this.name = name;
  this.color = ['yellow', 'black'];
}
Animal.prototype.getName = function () {
  console.log(this.name);
}
function Bird(name, age) {
  Animal.call(this, name);
  this.age = age;
}
Bird.prototype = new Animal();
Bird.prototype.constructor = Bird;
let b1 = new Animal('bird1');
console.log(b1); //{"name":"bird1","color":["yellow","black"]}
let b2 = new Animal('bird2');
b2.color.push('white');
console.log(b2);{"name":"bird2","color":["yellow","black","white"]}

~~~

##### 4.寄生组合继承

使用原型工厂封装继承

~~~javascript
function extend(child, parent) {
  child.prototype = Object.create(parent.prototype)
  child.prototype.constructor = child;
}
function Animal(name, age) {
  this.name = name;
  this.age = age;
  this.color = ['black', 'gray']
}
Animal.prototype.getColor = function () {
  console.log(this.color);
}
function Dog(name, age) {
  Animal.apply(this, [name, age])
}
extend(Dog, Animal)
let d = new Dog('Dh', 5);
console.log(d); // Dog {name: 'Dh', age: 5, color: Array(2)}
d.getColor(); // ['black', 'gray']
~~~

##### 5.class类实现继承使用super关键字实现继承

- extends就是class类实现继承的核心。
- 子类必须在constructor方法中调用super方法，且super方法需要放到第一行。

~~~javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  getName() {
    console.log(this.name);;
  }
}
class Dog extends Animal {
  constructor(name, color) {
    super(name)
    this.color = color;
  }
}
let d = new Dog('Dh', 'white');
console.log(d); // Dog {name: 'Dh', color: 'white'}
d.getName(); // Dh 
~~~

