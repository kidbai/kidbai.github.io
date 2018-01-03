# 私有变量
"严格来讲，JavaScript 中没有私有成员的概念;所有对象属性都是公有的。不过，倒是有一个私有 变量的概念。任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。" 

通过下面的例子，我们来解读上面这一句话:

```
    function animal() {
        var name = 'Joe'  //私有变量
        this.species = 'Husky'  //公有变量
    }

    var dog = new animal()
    console.log(dog.species)  // Husky
```

我们可以发现，外部是可以直接访问species这个变量的，但无法访问name，那么该如何访问内部的私有变量？我们可以

```
    function animal() {
        var name = 'Joe'  //私有变量
        this.species = 'Husky'  //公有变量
        this.say = function () {
            return name
        }
    }
    var dog = new animal()
    console.log(dog.say())   // 输出Joe
```

有权访问私有变量和私有函数的公有方法称为**特权方法(privileged method)**，在上面的例子中，say这个函数，就是该对象的特权方法。

## 静态私有变量

我们先来看一个例子:

```
(function(){
    var name = "";
    Person = function(value){
    name = value;
};
    Person.prototype.getName = function(){
        return name;
    };
    Person.prototype.setName = function (value){
        name = value;
    };
})();
var person1 = new Person("Nicholas");
console.log(person1.getName()); //"Nicholas"
person1.setName("Greg");
console.log(person1.getName()); //"Greg"
var person2 = new Person("Michael");
console.log(person1.getName()); //"Michael"
console.log(person2.getName()); //"Michael"

var person3 = new Person('youngbye');
console.log(person1.getName()); //youngbye
console.log(person2.getName()); //youngbye
console.log(person3.getName()); //youngbye
```

这个模式创建了一个私有作用域，并在其中封装了一个构造函数及相应的方法。在自执行的函数中，Person由于没有使用var定义，所以它成为了一个全局变量，外部是可以访问Person。
其次，getName和setName是在原型上定义，符合典型的原型模式。
 Person 构造函数与 getName()和 setName()方法一样，都有权访问私有变量 name。 在这种模式下，变量 name 就变成了一个静态的、由所有实例共享的属性。在一个实例上调 用 setName()会影响所有实例。而调用 setName()或新建一个 Person 实例都会赋予 name 属性一个 新值。结果就是所有实例都会返回相同的值。