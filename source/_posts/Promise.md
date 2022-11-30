---
title: Promise
date: 2022-10-15 15:30:27
tags:
---

# Promise

支持链式调用，解决回调地域问题

# Promise方法

1. Promise.resolve()     如果传入的参数为非Promise对象，则返回的结果为成功的Promise对象；如果传入的参数为Promise对象，则参数的结果决定了resolve的结果    let p1 = Promise.resolve()
2. Promise.reject()
3. Promise.all()    参数为n个promise对象组成的数组，结果返回一个新的promise，只有所有的promise都成功才成功，结果为所有成功的数组集合，有一个失败就失败，失败的那个值作为结果返回
4. Promise.race()  参数为n个promise数组，返回一个新的promise,第一个完成promise的结果状态就是最终的结果状态，不管是失败还是成功。

# Promise对象状态的改变方式

3种，resolve(),    reject(),     throw ' ';

  # 一些问题

Q:一个Promise指定多个成功/失败回调函数，都会调用吗？(相当于多个then) A:当promise改变为对应状态时都会调用

Q:改变promise状态和指定回调函数谁先谁后？A:都有可能，一般指定回调在改变状态。

如何先改变状态在指定回调，1.直接调用resolve()/reject()   2.延长更长时间才调用then() 

什么时候才能得到数据？1.如果先指定回调，则当状态改变时，回调函数调用得到数据，2.如果先改变状态，当指定回调时，得到数据，

其实反正要状态改变才能得到数据

Q:promise.then()返回新promise的结果状态由什么决定，1.抛出异常，变为rejected  2.返回如果是非promise任意值，则变为resolved,

3.返回另一个新的promise，结果由这个promise结果决定

Q：promise如何串链多个操作任务，通过then链式调用，因为then返回的也是promise

Q:promise异常穿透，可以在最后指定失败的回调，

Q：中断promise链，在回调函数返回一个pendding状态的promise对象

# Promise自定义封装，

以后再看

# async await

await 后面如果是promise对象，则是成功的值，如果失败，必须用try catch处理    如果是值，直接将值作为await的返回值

  # 宏任务与微任务

Ajax，定时器，宏任务

promise微任务