js 继承的几种方式：

```
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};

function Cat(name, color){
	this.name= name
	this.color = color
}

```


一.构造函数绑定

```
function Cat(name, color) {
	Animal.apply(this, arguments)
	this.name = name
	this.color = color
}

var cat = new Cat("小白", "白色")
console.log(cat.species)
```


缺点：

1.只能继承父类的实例属性和方法，不能继承原型属性/方法
2.无法实现函数复用，每个子类都有父类实例函数的副本，影响性能

二.prototype模式

```
Cat.prototype = new Animal()
Cat.constructor = Cat

var cat = new Cat("小白", "白色")
console.log(cat.species)

```
缺点：
1.来自原型对象的引用属性是所有实例共享的
2.创建子类实例时，无法向父类构造函数传参


三、组合继承（个人一般用这种 - -）


```

function Cat(name, color){
  Animal.call(this);
  this.name = name || 'Tom';
  this.color = color
}

Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

```
缺点：

调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）


四、寄生组合继承

```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();

Cat.prototype.constructor = Cat; // 需要修复下构造函数

```



