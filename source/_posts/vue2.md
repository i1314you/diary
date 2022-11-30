---
title: vue2
date: 2022-10-23 11:34:35
tags:
---

# MVVM

M：模型   就是后端数据    ``vue``中对应data里面的数据

V：视图    前端视图			对应DOM节点，就是模板代码

VM：视图模型   将后端数据与前端视图进行连接，可以实时监测变化           对应``vm``实例对象

data中所有属性都出现在了``vm``身上，  模板语法：里面可以放入``vm``所有属性和``vue``原型上的属性

# 数据代理

通过一个对象代理对另一个对象中属性的操作，本质上是通过``Object.defineProperty``来实现,见桌面图

``vue``中的数据代理   通过`vm`对象来代理data对象中属性的操作，就是将data对象中的数据移到`vm`上

目的让编码更加方便   每添加一个`vm`上的属性，都指定一个getter/setter,这样外边改data里面的数据也会发生改变，一旦data里面的数据发生改变，那么页面就会发生改变，这就是**响应式**，里面其实还有一个Observer(订阅发布模式)

# 事件的基本使用

@xxx绑定事件，其实跟DOM里面的事件一样（监听）               @click='demo'        @click='demo($event)'两种效果一致，只是后者可以传参，这里跟DOM监听一样，可以获得   `e.target`

事件修饰符： `@click.prevent=''`

prevent阻止默认事件，stop阻止事件冒泡，once事件只触发一次，capture捕获，

键盘事件：`@keyup=''    按键别名  回车：enter  如@keyup.enter=''`

delete   esc   space   tab    up   down  left   right  只有触发这些事件才会执行

# 计算属性和监视属性

计算属性：computed 底层借助`Object.defineProperty`提供的getter和setter     与methods相比，有缓存机制，效率更高，        最终出现在`vm`上，直接使用即可    简写跟methods里面一样，也是一个函数，但是不是，使用的时候直接在模板语法使用 如  `{{fullname}}`

监视属性：watch    默认只监视一层，如果监视所有层则   `deep:true;`

简写属性：跟计算属性一样，写成函数，但是有两个参数   如：`isHot(newVal,oldVal){}`只要不写immediate和deep就行

区别：计算属性通过return返回，不可以开启异步任务，监视属性通过自己写函数，可以开启异步任务，computed能完成的，watch也可以完成，只是有时候computed更简单点

被`vue`管理的函数，写成普通函数；不被`vue`管理的函数（定时器回调，Ajax回调，Promise回调）最好写成箭头函数，这样this的指向才是`vm`或组件实例对象

# class样式

1. 字符串写法：         `:class='a'`                     
2. 数组写法：     `:class='arr'`
3. 对象写法：     `:class='obj'`

以上三种将`a,arr,obj`全部保存在data里面，可以实现动态绑定

绑定style样式差不多            内联             ` :style='obj'          将这个obj写在data里面`           `:style='{fontSize:xxx}'`

# 条件渲染

v-if   切换频率低的场景，不展示的DOM元素直接被移除

v-show   频率高的场景，DOM没有被移除，仅仅样式隐藏

# 列表渲染

`v-for = '(item,index) in xxx' :key='yyy'   `      一般遍历数组和对象

# key的作用

(见桌面)

# Vue监测数据

`vue`为监视data中所有的数据，

1.监测对象中的数据：对象后添加到属性，`Vue`默认不做响应式

如果想要响应式，就要     ` Vue.set(obj,propertyName,val)或vm.$set()`

2.监测数组中的数据，做了两件事，调用原生方法对数组进行更新，重新解析模板，进而更新页面

3.在`Vue`中修改数组的某个元素要使用改变原数组的7个方法，如push或者`Vue.set()`

特别注意：`Vue.set()不能给vm或vm`的根数据对象添加属性，就是说只能给data里面已存在的属性再次添加属性，有个嵌套关系

数据劫持：为每一个data数据添加getter，setter

响应式原理：本质上也是`Object.defneProperty()`见上面数据代理

# 收集表单数据

一般input框，v-model收集的就是value值，用户输入的就是value值

单选框，需要给单选框radio配置value属性才行，那么v-model收集的就是配置的value属性值

多选框 checkbox  配置value属性，1.v-model初始值是非数组，那么收集的就是checked（勾选或未勾选）2.如果初始值是数组，那么我们就可以获得value值

v-model三大修饰符    `v-model.lazy       .number       .trim`

v-model一般只适用于表单数据收集value

# 生命周期

`beforeCreate    `           初始化，数据代理还没开始

 created     此时`vm`可以访问data里面的数据和methods方法

   `beforeMount         `未经编译的DOM结构，此时是虚拟DOM

  mounted   			模板解析完成，此处可以进行一系列的操作，如定时器，事件

`beforeUpdate `          数据是新的，页面是旧的，此时数据和页面不同步 

 updated   根据新数据生成新的虚拟DOM，与旧虚拟DOM比较  完成页面更新，即Model->View的更新

` beforeDestroyed  `解绑一些事件，如关闭定时器

​    destroyed  没啥用

# 组件与模块

实现应用中局部功能代码和资源的集合     一个`.vue`文件就是一个组件

模块：就是`js`文件

创建组件   `Vue.extend({})`       后面我们会简写

组件本质上是一个`VueComponent`构造函数，每次创建组件的时候都会生成一个新的构造函数，  如果使用如<school/>那`Vue`会帮我们创建组件实例对象  即执行 ` new  VueComponent(options)`

`在组件中，this指向组件实例对象（vc）在new Vue中指向Vue实例对象`

一个重要的内置关系      ` VueComponent.prototype.__proto__==Vue.prototype`      让`vc`可以访问到`Vue`原型上的属性和方法

# 单文件组件

安装 `Vetur`插件，可以生成模板，有高亮效果。

在script标签里面将组件暴露出去      export default{}   //省略了`Vue.extend()`

# 创建脚手架文件

全局安装  ` npm install -g @vue/cli   (仅第一次执行)           查看是否安装好，vue`

`vue create xxx`

`npm run serve`

我们引入的`vue`文件是残缺的，没包括模板解析器，在main.js里面使用render函数可以渲染App组件，这样就解决了模板问题

# ref

跟id差不多，就是给html元素也就是p,div打标识，然后可以获取DOM结构

如果使用ref的话，先打标识，ref='a'    获取的方式为      `this.$refs.a`此处this是`vc`实例对象

但是有点不同，如果给子组件打标识的话，id最后获取到的是DOM结构，ref获取到的是打标识的那个组件实例对象（有用后面）

# props

用于父组件向子组件传递数据，如

父组件中<Student   name='houzi'   age='18'/>  (:age='18'表示传入数字18）子组件中，也就是Student组件内部中，用  props:['name','age']进行接收，这样可以用到模板中，跟data里面的数据一样，保存在`vc`中，但是props数据不能更改，跟data数据不一样这点，    三种props书写，我们这里简写。

还可以子组件给父组件传递数据，在父组件中 <Student :getSchoolName='getSchoolName'/>父组件中要定义这个函数，在子组件中用props声明接收这个函数，然后调用这个函数传参，父组件中`getSchoolName`中的形参就是子组件传递的数据

# mixin

功能：可以把多个组件共用的配置提取成一个混入对象

其实就是一个`js`文件，最后将其暴露出去  定义混合{ data(){},methods:{}}

使用混入，在组件中，引入，然后借助 `  mixins配置项       mixins:['xxx']`   注意写成数组形式跟props一样,全局混入，在main.js里面写        `Vue.mixin()`没有引号，其实一般不用这种全局混入

# 插件 （我不用）

# 组件自定义事件

给组件用的                      `js`内置的事件如`click,keyup`给html元素用的

1.绑定自定义事件，其实就是跟props差不多，用于子组件给父组件传递数据，在父组件中`<Student @demo='sendStudentName'/>`事件回调也就是`sendStundentName` 在父组件中，子组件触发demo事件那么父组件执行回调，就可以收到参数

这样就给student子组件绑定了demo事件，在子组件中，通过   this.$emit('demo',参数)       触发自定义事件，然后传参，这里跟props传递数据差不多，只是不用props接收数据。

2.第二种写法，给<Student ref='student'>给组件打标识，然后再父组件中       mounted里面`this.$refs.student.$on('demo',this.sendStudentName)`   $once触发一次      其实这一步就跟第一步也差不多，不过更加灵活，可以增加异步任务

解绑自定义事件，在子组件中调用     this.$off()        在组件中@click也是自定义事件，还是需要在子组件中触发click事件

# 全局事件总线

实现任意组件间的通信         

安装全局事件总线，1.要考虑到要让每个组件都能看到2.要有$on,$emit,$off  API        所以我们将其绑定到`vm`上面

在main.js里面     `beforeCreate(){           Vue.prototype.$bus = this       }`因为此处模板还没有开始渲染，所以添加$bus在`Vue`的原型对象上，同时  this代表`vm`，这样所有的`vm`和所有的`vc`都可以访问到。

使用方法：在一个组件中  mounted里面   mounted(){this.$bus.$on('hello',回调函数)}      绑定hello事件，设置回调函数，在回调函数里面可以设置形参进行接收参数  注：最好在`beforeDestroy(){this.$bus.$off('hello')}`   解绑自定义hello事件。

在另一个组件里面    this.$bus.$emit('hello',xxx)   触发hello事件，将xxx数据传递给上面那个组件

# 消息订阅与发布

安装库       `npm i pubsub-js  `

引入`import   pubsub   from   'pubsub-js'`    

A组件想获得数据  则  在`mounted(){ this.pid = pubsub.subscripe('xxx',this.demo)}``this.demo`就是事件回调,但是这里跟全局事件总线不一样，接收两个参数，第一个为事件名字，第二个才是数据

B组件给A组件传递数据   `pubsub.publish('xxx',数据)`

解绑事件，在A组件里面`beforeDestroy(){pubsub.unsubscripe(this.pid)}`这里跟全局事件总线不一样，传的是`pid`不是绑定事件名字。

# nextTick

`this.$nextTick(回调函数)`  在下一次DOM更新结束后执行其指定的回调，因为执行顺序是先执行methods里面的方法，在渲染模板，如果我们没有`nextTick`那么后面即使改了DOM某些操作，也不会生效，所有需要先渲染模板，在执行指定回调，用`setTimeout`也可以

# 动画效果

这边直接使用外部第三方库就行   如引入    animate.css然后再看教程使用

# axios

对Ajax的二次封装是一个库  通过  ` npm i aixos `     引入     用的话 `  import   axios    from   'axios'`

# 跨域

`jsonp      cors     代理服务器`在vue.config.js里面配置

```js
module.exports = {
  devServer: {
    proxy: {
      '/api': {   //请求前缀
        target: '<url>',//请求路径，
        pathRewrite:{'^/api':''}//碰到api将其变成空的
        ws: true,//可要可不要
        changeOrigin: true//可要可不要
      },
```

# 插槽

默认插槽，具名插值，作用域插值（太麻烦，反正我现在不用）

让父组件可以向子组件指定位置插入html结构，也是一种组件间通信方式，适用于父组件-》子组件

父组件中     <Category><div></div></Category>

子组件中      <div><slot>这里面可以放默认内容</slot></div>此时父组件中里面的内容会出现在slot这里

具名插槽：给slot添加标签       <slot name='a'></slot>

父组件中         <div slot='a'></div>

这样可以让父组件中内容精确放在子组件中

作用域插槽：   父组件里面的子组件中写入  DOM结构，必须有template标签才行，给标签添加   v-slot属性，在子组件中，slot    标签里面，写上    :games = 'games'   有点像props数据，通过这个传递给  父组件中里面那个子组件，   父组件里面那个子组件收到的是一个对象，   如  `v-slot = 'youxi',`          获取数据则  ` youxi.games`       也可以通过解构赋值出来   其实这个插槽就是可以实现组件的复用，在父组件里面写入DOM结构，改变组件的样式

# vuex

集中式管理数据的一个插件，可以多组件共享数据，用于任意间组件通信   vue2用vuex3,vue3用vuex4

安装`vuex     npm i vuex@3` 引入   `impor     使用  Vue.use(vuex)`         需要先引入store   在`vm上写入store配置项`

再index.js里面   `Vue.use(Vuex)`   然后`export default new Vuex.Store({})`这样`vm和vc`上面都有store属性了

store配置项：state,mutations,actions,getters(对state里面的数据进行加工，有点像计算属性，接收一个形参，该新参是state，想要获得数据，则`state.sum`,要有return返回结果)

`mapState,mapGetters `      要先从`vuex`里面引入

在computed里面定义,      `...mapState(['sum','a])`数组里面写法比较简单，要写成''形式，不然会报错，当然还有对象写法麻烦，要写...表示一个对象里面不能添加另一个对象，必须要使用扩展运算符

`mapMutations,mapActions` 在methods里面使用

`...mapActions(['add'])`此时调用store里面的add方法，在组件中也要使用add方法，同时传参也是在组件中传，如add(2)

模块化命名空间  添加  `namespaced:true;`  一个模块里面有4个`vuex`里面的方法   在`new Vuex.Store({modules:{}})`添加modules属性，

读取state数据 `...mapState('a',['sum','b])`

读取getter数据同理

`this.$store.dispatch('a/add',person)`    person表示传递的参数   `...mapActions('a',['add'])`     传参也是在组件中传 add(2)

# 路由

`vue-router`插件库  单页面应用，不会刷新页面  路由理解：就是一组key-value,如/class=>组件

安装  ，`npm i vue-router引入  import VueRouter from 'vue-router'  使用Vue.use(VueRouter)`

配置路由  `export default new VueRouter({routes:[{}]}) 里面的配置项不会看文档吧，对了   注意点，一定要在vm里面配置router`

实现切换：<router-link to='/about'></router-link>相当于a标签

展示位置   <router-view></router-view>

路由组件放在pages里面，非路由组件放在components里面，切换路由组件，默认被销毁，每个组件都有$route属性，整个应用只有一个router,可以通过$router获得

嵌套路由，多级路由，   就是在某一个路由里面添加children配置项，跳转路径要写完整路径，包含前面路由的路径也要写

路由传参：1.:to='{path:   ,query:{}}'传递query参数在router-link里面写   接收参数 `$route.query.`

2.params参数，使用前，在路由配置项path里面必须占位如  path='/detail/:id/:title'     在to里面一般写法为/hello/a直接这样写就行，当前前面还有路径       注意如果用to的对象写法必须  写成   name简化路由，给路由命名，不能写成path

简化路由给路由配置项里面添加   name属性  在后面编程式路由导航尽量都写name属性

路由里面还有  props配置，三种写法，同时要在组件里面接收，其实就是让路由组件更方便接收参数，反正我不用，还是直接this.$route.query.id吧

<router-link replace></router-link>默认为push模式，就是有历史记录，有回退上一级路由功能，如果添加replace属性，就没有回退功能

编程式路由导航，让路由跳转更灵活    如  `this.$router.push({name:'',params:{}}),还有this.$router.replace`

还有`this.$router.forward(),back(),go()`一般不用

缓存路由组件：让不展示的路由组件保持挂载，不被销毁，如组件里面有input框，我们一般要进行缓存

<keep-alive include='News'><router-view></router-view></keep-alive>注意是在router-view外面添加keep-alive里面有include属性,注意里面的名字为组件的名字，即要给想要缓存的组件配置name属性，除了配置的组件，其他组件都不缓存

缓存一个组件include='News',缓存多个组件:include='['News','Message']'

两个新的生命周期函数      activated  deactivated路由组件被激活或者失活时触发，专门为路由组件的生命周期函数，因为在某些转态即使清空定时器，如果保持keep-alive那定时器也不会被销毁，会缓存，所有用这两个生命周期函数

路由守卫：就是在路由跳转的时候增加权限   全局守卫：`router.beforeEach((to,from,next)=>{})to表示当前组件，from表示跳转组件，next放行   ``afterEach `没有next          独享守卫：在   routes里面配置，跟path同级，`beforeEnter`  组件内守卫，在路由组件里面配置

路由器两种工作模式   跟routes平级，mode:'history'  默认hash模式，出现   /#   兼容性好，后面的值不会传给服务器

history模式，美观，兼容性略差，应用部署时需要后端人员支持，解决刷新页面出现问题