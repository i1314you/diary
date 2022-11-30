---
title: es6
date: 2022-10-03 10:05:25
cover: http://domain.com/awesome.jpg
---

# 1.暂时性死区

意思就是 let 和 ``const ``声明的变量在块级作用域中，在声明之前不能使用，使用了就会报错   主要是为了减少运行时错误

块级作用域的出现，使得IIFE变得不再需要了

# 2.声明变量的6种方法

``var`` , function , let , ``const`` , import , class

# 3.顶层对象的属性

ES5中，``window.a`` == a         其实这种设计不好，最大的败笔之一，因为程序员可能不小心创建了全局变量，同时顶层对象属性是到处可以读写的，这样不适合模块化编程。 所有ES6规定，let ，``const ``, class声明的全局变量不属于顶层对象的属性

# 4.变量的结构赋值

数组的解构赋值：

用法：let [a,b,c] = [1,2,3]      必须具备  Iterator  接口，才能进行解构赋值

对象的解构赋值：

``let [foo,bar] = {foo:'aaa',bar:'bbb'},``      变量必须与属性同名才能获取值

``let {foo:foo,bar:bar} = {foo:'aaa',bar:'bbb'}``      前面是同名属性，真正赋值的是后面，后面才是变量

字符串的解构赋值：

``let {a,b,c} = 'hel'``

``let {length:len} = 'hello'     len//5  ``      类似数组的对象都有一个length属性

应用场景：

1. 交换变量值： [x,y] = [y,x]
2. 遍历map解构，如       for(let [key] of map){}    for(let [key,value] of map){}    for(let [value] of map){}
3. 输入模块的指定方法     如： `` const {reqGetGoodList} = require()``

# 5.模板字符串

​             相当于可以将变量加入字符串中：```${x}+${y}`` `

# 6.箭头函数

1. 没有自己this
2. 不可以new，就是不能当构造函数
3. 没有``argumengts``对象
4. 不能使用``yield``命令

# 7.扩展运算符

1.  复制数组，对数组进行一个克隆生成新的数组        a2.contact(a1)         a2 = [...a1]    [...a2] = a1  解构赋值加扩展运算符
2. 合并数组       [...a1,...a2]
3. 可以将字符串转为真正的数组       [...'he']      输出    ['h','e']
4. 任何实现了Iterator 接口的对象都可以转换成真正的数组
5. map和set 也是类数组

``Array.from()``用于将两类对象转为真正的数组，类似数组对象和可遍历对象，Iterator

``Object.assign(target,source1,source2)``用于对象的合并      将源对象的可枚举属性添加到目标对象上

指数运算符：a ** a   特点为右结合，注意， a ** b ** c = a ** (b ** c)

``res.push([...arr])``往一个数组里面加入一个数组可以这样理解，因为push只能添加单个元素，所以要将数组中每个元素拆出来，最后将其当成一个数组添加进去，这样会生成一个二维数组，``res.push(...arr)``此处是将数组``arr``的元素添加到res里面，结果还是一维数组

# 8.Symbol 

对象属性名都是字符串，这容易造成属性名的冲突。保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。

```javascript
let s = Symbol();注意不要new 	因为他是基本类型
```

# 9.set和map

注：类似数组

set:

属性：size 返回成员总数

方法：add delete has clear          **`keys()`，`values()`，`entries()`**         `keys`方法和`values`方法的行为完全一致。

Set 结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，

应用：

1.  去重

2. 使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集

   ```javascript
   let a = new Set([1, 2, 3]);
   let b = new Set([4, 3, 2]);
   
   // 并集
   let union = new Set([...a, ...b]);
   // Set {1, 2, 3, 4}
   
   // 交集
   let intersect = new Set([...a].filter(x => b.has(x)));  此处使用[...a]是将其转为数组，同时前面有set是因为b.has要用set
   // set {2, 3}
   
   // （a 相对于 b 的）差集
   let difference = new Set([...a].filter(x => !b.has(x)));
   // Set {1}
   ```

不重要了解：``WeakSet`` 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

首先，``WeakSet`` 的成员只能是对象，而不能是其他类型的值。

3.当我们使用set对数组去重的时候有点不一样                                 /要把数组转换成字符串， 因为你可以自己试试 new Set([1,2]).has([1,2]) 输出为false，但是 new Set(['1,2']).has('1,2') 输出为true，所以这里要转换成字符串

``const str = path.join();``//这里path为一个数组

``if(!set.has(str)){``

​      ``  res.push([...path]);``  //这里应该知道吧，push只能插入单个元素，所以将path用...

​        ``set.add(str);``

​      ``}``

map:

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。为了解决这个问题，ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

属性：size

方法：**set(key, value)**          **get(key)**       **has(key)**        **delete(key)**     **clear()**    map一切操作都是基于键的

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

**（1）Map 转为数组**  使用扩展运算符（`...`）

**（2）数组 转为 Map**  将数组传入 Map 构造函数，就可以转为 Map。

`WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。

# Proxy

# Reflect

# Array.from()

将类数组变为数组，必须有length属性，属性名必须为数值型或者字符串型的数字   

可以接收第二个参数，类似于map方法

将字符串转为数组