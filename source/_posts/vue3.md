---
title: vue3
date: 2022-10-27 17:12:59
tags:
---

# 创建工程文件

使用`vue-cli   或者vite`新一代前端构建工具，不过我现在不用

# 架构

main.js里面引入的不是`Vue`的构造函数了，是一个名为`createApp`的工厂函数   安装开发者工具

# setup

配置项：组件中所有的数据，方法都要配置在`setup`里面,这个配置项是一个函数，同时要将这些数据，方法进行返回才行通过return，而且我们这里返回一个对象就行。在模板中可以直接使用，setup不能是一个`async`函数，这样拿数据的时候不方便

# ref函数

作用：定义一个响应式的数据    使用之前必须先引入        `import {ref} from 'vue'`

语法:   `const xxx = ref(123)`  在`js`中操作数据的话必须  ` xxx.value`   但是在模板中操作数据，则   <div>xxx(这里其实是模板字符串我没写)</div>  不需要直接`xxx.value`了，而是xxx就行

接受的数据可以是基本类型也可以是对象类型

基本类型数据靠   `Object.defineProperty()的get与set完成`  对象类型的数据，借助了vue3.0中的一个新函数   `reactive`函数

# reactive函数

作用：定义一个对象类型的响应式数据（基本类型别用，用ref函数)  也要引用 ` import {reactive} from 'vue'`

`const xxx = reactive(被代理对象)`接收一个对象或数组，返回一个代理对象（Proxy对象）

reactive定义的响应式数据是深层次的，内部基于Proxy实现

# vue3响应式原理

`vue2` 存在问题，新值，删除属性页面不更新，直接通过下标更改数组，页面不更新， 通过this.$set或$delete，通过重写数组7大方法

vue3  通过Proxy，里面也有`get,set，deleteProperty`通过Reflect对被代理的对象属性进行操作    返回：`return Reflect.get()  .set()  .deleteProperty`其实一般为  Object，但是会出现一些问题，封装底层代码的时候，使用Reflect犯错误也没事，避免使用了try catch

reactive定义的数据不需要.value

# setup注意点

在`beforeCreate`之前执行一次，this是`undefined`

setup(){}  参数   props:值为对象，组件外部传递过来，且组件内部声明接收了的属性

context上下文对象：  attrs:对象，组件外部传递过来，但是props里面没有接收，相当于  vue2里面  this.$attrs

​									slots:收到的插槽内容，相当于  this.$slots

​									emit:分发自定义事件的函数，相当于  this.$emit

# computed

需要引入在setup里面配置     import {computed} from `'vue'`

`let fullname = computed(()=>{})   `computed是一个函数，里面还有一个函数，箭头函数和普通函数都没事，因为this为`undefined`

这是简写，还有set和get写法，这样读和写都能看到   别忘了将其return返回

# wacth

也需要引入    使用：   watch(sum,(new,old)=>{},{配置对象})   接收三个参数，第一个监视的值，第二个回调函数，包含new和old，第三个配置对象  如  `immediate:true,deep=true`

监视ref的数据时，监视一个  直接写  sum   监视多个，写   [sum,age]   写成数组形式

监视reactive数据，  无法获得`oldValue`，强制开启深度监视,deep配置无效，监视reactive定义的响应式数据里面的某个对象的属性，在deep有效  一般监视写成   person,（一般这种最多使用）    如果想监视person某个属性则    第一个参数为   ` ()=>person.age`,如果是多个属性   []  写成数组形式，如果监视person里面对象的属性，则`oldValue`也会有效

通过这些避错，我选择将 基本类型用  ref   ,引用类型用   reactive    ,监视的时候都不要   .value   这样都不会出现问题

# watchEffect

也需要引入，是一个函数，   不用指明监视那个属性，监视回调中用到了那个属性，那就会进行监视， `  watchEffect(()=>{})`只要一个参数，它一开会自动执行一次   相当于添加了  immediate=true

有点像computed   但computed注重结果需要return    他不需要return注重过程  只要里面的数据发生变化，那就会执行相应的回调

# vue3生命周期

可以直接当成配置项，跟Vue2一样不过有两个生命周期函数不一样    `beforeUnmount    unmounted`这两个替换了vue2的最后两个函数

如果想当成组合函数使用，必须先引入，import    写在setup里面       全部变名字，但是只有6个，`beforeCreate,created`没有，而且其余6个生命周期函数必须加    on     书写形式    ` onMounted(()=>{})`  他是一个函数，里面有个回调函数

# 自定义hook函数

本质上是一个函数，吧setup函数中使用的组合式API进行了封装，类似于vue2中的`mixin ` ，可以复用代码

就是一个`.js`文件   需要暴露出去   export  default  function(){        return point;  }里面的代码跟setup里面的代码是意义的，别忘记了最后将其return出去    同时如果用到了一些ref等等，需要引入              注意注意，hook本质上是一个函数，所有引入给其他组件的时候需要调用

# toRef与toRefs

作用：创建ref  对象，语法：`const  name = toRef(person,'name')`   因为我们在模板里面写的时候需要用到name属性，一般我们为                

`person.name`  但是感觉这样有点麻烦，我们就借助了`toRef  `可以直接用   name   这样name也是响应式的，也会引起person里面的属性变化    在  return  里面返回     return` {  name:toRef(person,'name')   }`  我们这里不用ref是因为，虽然生成了ref，但是生成的是新的ref对象，他的改变不会影响    原来   setup  里面  person的name变化  

`toRefs ` 返回多个ref对象       `     toRefs(person)    return{     ...toRefs(person)   }`    注意用...因为对象里面不能为对象，所以将其展开，就会生成一系列key-value    但是只能生成第一层的  ref     如age,name  ,a:{b}   这样模板里面使用  则  `age  name   a.b`

# 下面组合式API了解即可



# shallowReatcive  shallowRef

`shallowReactive`  只处理对象最外层属性的响应式，（浅响应式） 可以实现性能优化

`shallowRef  `  只处理基本类型响应式（跟ref差不多）， 不进行对象的相应式处理，也就是说改了页面也不更新，也不求助于reactive

# readonly  shallowReadonly

`readonly`让一个相应式的数据变为只读的（深只读)就是每个属性都不能改

`shallowReadonly `  浅只读，第一层数据不能改，后面可以改

应用场景：别人给我数据的时候，我这边不应该改数据，

# toRaw  markRaw

`toRaw`  将  reactive生成的响应式对象变成普通对象，，这样对对象操作，不会引起页面更新

`markRaw  ` 标记一个对象，让其无法成为响应式对象，  如有些第三方库不应该为响应式，当渲染不可变大数据时，跳过响应式可以提高性能



`customRef  ` :自定义ref

# provide inject

实现祖与后代组件间的通信   祖组件中  provide('car',car)    第一个参数表示传入参数名字，第二个参数表示传入的东西                            后代组件    `const car = inject('car')` 

# 相应式数据的判断

`isRef  isReactive isReadonly isProxy`检查一个对象是否是reactive或`readonly`方法创建的代理

# 组合式API优势

vue2里面为配置对象API

vue3为组合API，这样可以让相关功能代码更加有序组织在一起  其实为设置  hook函数，直接引入，方便点

# 新的组件

vue2里面必须有根标签，但是vue3里面可以没有，其实会添加一个Fragment虚拟元素，减少标签层级

teleport组件  <teleport to='body'></teleport>   可以让里面的元素传送走，to表示传送到某个地方，这样我们可以更方便的在某个参考系使用  这是移到了body里面，相当于body进行定位

# vue3其他改变

移出过滤器  将`Vue.config`  或者其他方法移到了   `app.config   app.mixin`等等之类的