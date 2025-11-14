[TOC]

# Javascript

## js的序列化

```javascript
var dict = {"name":"chuxue","age":22}
console.log(typeof dict); // object
var ser_dict = JSON.stringify(dict);
console.log(typeof ser_dict); // string
```

## JS的反序列化

```javascript
var unser_dict = JSON.parse(ser_dict);
console.log(typeof unser_dict) // object
```

## JS的转义

```javascript
// url解码
console.log(decodeURI('%E4%B8%AD%E6%96%87
'));
// url编码
console.log(encodeURI('中文'));
// url编码，对非英文的字符转义
console.log(encodeURIComponent('https://中文'))
// 对字符串进行url转义，弃用
console.log(escape('http://example.com'))

```

## JS代码执行

```javascript
let cmd = "alert(666)"
eval(cmd)
```

## JS面向对象

创建对象和创建函数的关键字一样是`function`

```javascript
function Person(name){
    this._name=name;
    this.getName=function(){
        console.log(this._name)
    }
    this.setName=function(newName){
        this._name=newName;
    }
}
var p = new Person("chuxue");
p.getName();
p.setName('test');
p.getName();
// 这里的_name代表name变量是一个私有属性，不能在外部访问，只能通过getter方法和setter访问和操作
```

另一种创建方式，利用class关键字

```javascript
class Person{
    constructor(name){
        this.username=name;
    }
    getName(){
        console.log(this.username);
    }
    setName(newName){
        this.username=newName;
    }
}
var p = new Person('chuxue');
p.getName();
p.setName('test');
p.getName();
```

## JS原型

每个函数在创建的时候，javascript都会自动给这个函数添加一个属性

```javascript
function Person(){}
console.log(Person);
console.log(Person.prototype)
```

上面的的`Person.prototype`就是这个函数的原型对象

如果我们用这个函数作为构造函数来创建对象

```javascript
const p  = new Person();
```

那么这个对象内部自动带有`__proto__`的隐藏属性

这个属性会指向`Person.prototype`对象

```javascript
console.log(p.__proto__ == Person.prototype) // true
```

假设这里的对象是一个人的信息，而且大家的年龄一样的话，大家的年龄就可以添加到原型对象的属性可以给大家去访问

```javascript
Person.prototype.age=21;
Person.prototype.sayName=function(){
    console.log(this.name);
}
```

所以可以把公用的属性或方法写在原型中

## 运算符

```javascript
var a = 1+1 // 2
var a = 1-1 // 0
var a = 1*1 // 1 
var a = 1/1 // 1
var a = 3 % 2 // 1 
var a = 1 ;
a++ // a ==2 
var b = 2 ;
b-- // b == 1 
1 > 2 // false 
1 < 2 // true 
1 == 2 // false 
1 == '1' // true 
1 === '1' // false
1 != 2 // true 
1 !== '1' // true
var a = 1 // a == 1 
var a += 1 // a == 2 
var a -= 1 // a == 1 
var a *= 2 // a == 2 
var a /= 2 // a == 1 
var a %= 2 // a == 1
var a = true || false // a == true
var a = false || false // a == false
var a = true && true // a == true
var a = true && false // a == false
var a = 1==1?6:100 // a == 6
```

## 流程控制语句

```javascript
if(表达式){
  表达式为真执行语句
   }else{
       表达式为假执行语句
   }

if(条件一){
   语句
   }else if(条件二){
            语句
            }else{
    语句
}

switch(表达式){
    case n:
        语句
        break;
    case n:
        语句
        break;
    default:
        语句
        break;
}

```

## 循环

```javascript
var i = 0;
while(true){
    if(i = 9){
        break
    }
    console.log(i)
    i++;
}

do{
    语句
}while(条件)
    
for(var i = 0 ; i<9;i++){
    语句
}
```





