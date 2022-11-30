---
title: node.js
date: 2022-10-15 15:30:10
tags:
---

# CommonJS规范

希望JS能够在任何地方运行   对模块的定义：模块引用，模块定义，模块标识

一个``js``文件就是一个模块，通过   ``require()``引入外部模块  使用相对路径或绝对路径   ./ 或 ../

``var md = require('')``

Node中，每个js代码都是运行在一个函数中，而不是全局作用域 如

``function (exports,require,module,_filename,__dirname){}``

一般暴露文件我们使用``module.exports = {}``

# NPM

是一种实践，可以完成第三方模块的发布，安装，依赖

命令 ：

1. ``npm  -v``
2. ``npm install 包名``
3. ``npm remove 包名``
4. ``npm install 包名 --save   ``        安装并添加到依赖中，一般为这种
5. ``npm install``          下载当前项目所依赖的包
6. ``npm install   包名  -g   `` 全局安装包，（一般为一些工具）

会安装``cnpm``

使用模块名字引入模块时，查找方式跟作用域链差不多，一层一层往上找

# 初识nodejs

DOM,BOM,ajax等等一些其实是浏览器提供的，也就是说我们调用这些API只能在浏览器里面生效，通过`javascript`引擎进行解析

 1.fs 模块       `__dirname`  可以获得基本路径

2.path模块    有一些方法 ` .extname      .join  .basename`    

127.0.0.1访问自己的电脑的服务器  它的域名是localhost         DNS域名服务器     一个web服务器上面有多个服务，所以出现了端口号，在实际应用中，端口号80默认省略

3.http模块，创建服务器

4.require()模块时，得到的永远是`module.exports`指向的对象,其实exports跟他差不多，平时我们用的时候不要混用

COMMONJS规范，每个模块内部，module代表当前模块，`module.exports`是对外的接口，require()用于加载模块

快速创建`package.json `   `npm init -y` 一般我们在做项目的时候第一步就是这样，后面安装的包都会出现在这里面，可以看到版本号

一次性安装所有依赖包   `npm install` 这样他就会读取包管理工具里面的dependencies节点，平常我们将node_modules放在忽略文件里

`npm  i 包名 -d(--save-dev)`  安装到开发环境里面，只在开发阶段用到，项目上线不需要用到

5.解决包下载问题  在`cmd`里面输入如下代码 `npm config get registry`查看当前的下包地址

`npm config set registry=https://registry.npm.taobao.org/`  切换到淘宝镜像下载

# express

全局安装 `npm install -g nodemon`这样我们每次启动服务器在修改代码的时候就不用重启服务器了  `nodemon abc./js`

使用express我们可以轻松创造web网站服务器和`api`接口服务器

安装  `npm i express@4.17.1`

创建基本服务器，  `const express = require('express')      const app=express()     app.listen(80,()=>{})`



`api`接口   ` app.get('/user/:id',(req,res)=>{res.send({})})`这就是一个简单的get请求，通过`res.send`相应数据，挂载到app上

使用:id可以获得动态参数也就是`params参数  使用  req.params获得   query参数使用req.query获得`



`app.use()`就是注册全局中间件  可以添加路由前缀   

注册路由模块    上面一种有时`api`多了不方便，注册路由模块   `const router = require('./use.js')   app.use('api',router)`

创建路由模块   `const express=require('express') const router = express.Router() 这里进行api书写 module.exports=router`

一个简单的get接口  `router.get('user',(req,res)=>{const query = req.query; res.send({status:0,msg='数据请求成功',data:query})})`



中间件：就是对请求进行预处理  多个中间件之间，共享一份`req和res`，我们可以在上游中间件为`req和res`添加属性和方法，可以供下游使用

定义中间件   `app.use((req,res,next)={逻辑函数;next()})`  注意必须有next()才行，如果定义多个中间件，则多写几次`app.use()`

请求到达服务器，会按照顺序进行执行，注意，中间件必须写在接口上方

不使用`app.use()`的叫做局部中间件    `const mw = (req,res,next)=>{next};   app.get('/user',mw,mw2,(req,res)=>{})`多个局部中间件之间使用       ,     分隔

必须在路由之间注册中间件

express内置中间件   `express.static express.json express.urlencoded` 可以解析`json和urlencoded`请求体数据

# 跨域

`cors `      也是一个中间件，`npm install cors `    `const cors = require('cors')  //在路由之前使用 app.use(cors())` 

跨域资源共享，由一系列HTTP响应头组成，相应头部：

`res.setHeader('Access-Control-Allow-Origin','*')`表示允许来自任何域的请求  ，也可单独指定域

默认情况，`cors`仅支持客户端发起的get,post,head请求，如果客户端想发起其他请求，则在服务器配置

`res.setHeader('Access-Control-Allow-Methods','*')`  支持所有请求

`cors`仅支持9个请求头，如果想要发送额外请求头信息，则在服务器配置  content-type默认值只有三个

`res.setHeader('Access-Control-Allow-Header','Content-Type,X-Custom-Header')`

简单请求和预检请求，客户端与服务器发送一次请求，   OPTION请求成功后，才会发起真正的请求，也就是说发起两次请求



JSONP

其实就是通过script标签的`src`属性，请求服务器上的数据，服务器返回一个函数的调用

在客户端上我们使用  `src='http://www.baidu.com'`   发起请求，如Ajax请求 将回调函数名字可以通过请求体query发给服务器      同时定义一个回调函数，  `  callback=function(data){console.log(data)}`

回调函数的名字叫做callback

在服务器上我们使用如下代码，

```javascript
app.get('/user',(req,res)=>{
    const funcName = req.query.callback
    const data = {}
    const scriptStr = `${funcName}(${JSON.stringfy(data)})`
    res.send(scriptStr)
})
```

# 数据库

PK 主键，唯一标识

NN  值不允许为空

UQ 值唯一

AI 值自动增长

SQL 一种编程语言，用来操作数据库里面的数据 只能在关系型数据库中使用

掌握CRUD就行  where and or        order by      count(*)

查询数据     查询的结果会存储在一个结果集里面

```sql
select * from 表名称
select 列名称 from 表名称
SELECT * FROM my_db.users;
```

插入数据  会插入新的数据行    

```sql
insert into my_db.users (id,username,password) values (3,'da','1245')
```

修改数据

```sql
update my_db.users set username='hahha' where id=3
update my_db.users set username='hahha',password='546218' where id=3  修改多个值用,分隔
```

删除数据

```sql
delete from my_db.users where id=3
```

where 用于限定选择的标准

对于and 和 or

```sql
update my_db.users set username='hahha' where id>1 and id<3 
```

order by

```sql
SELECT * FROM my_db.users order by id;  //按照id进行升序排序
SELECT * FROM my_db.users order by id desc;  //降序排序
SELECT * FROM my_db.users order by id desc,username asc; //多重排序
```

count(*) 返回查询结果的总数据条数

```sql
select count(*) from my_db.users;
select count(*) as total from my_db.users where id=1; //可以设置别名同时增加限制条件
```



# 在项目中使用数据库

1.安装数据库 mysql(第三方模块)

2.通过mysql连接MySQL数据库

3.通过mysql执行SQL语句

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1',//数据库ip地址
    user: 'root', //登录数据库的账号
    password: 'admin123',//登录数据库的密码
    database: 'my_db'//指定要操作哪个数据库
})
//这一行代码是为了测试数据库是否成功连接
db.query('SELECT 1',(err,results)=>{
    if(err)return console.log(err.message)
    console.log(results)
})
```

```js
插入数据
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1',//数据库ip地址
    user: 'root', //登录数据库的账号
    password: 'admin123',//登录数据库的密码
    database: 'my_db'//指定要操作哪个数据库
})
const user = {id:'4',username:'123',password:'5414'}
//?表示占位符
const sqlStr = 'insert into my_db.users (id,username,password) values (?,?,?)'
db.query(sqlStr,[user.id,user.username,user.password],(err,results)=>{
    if(err)return console.log(err.message)
    if(results.affectedRows===1)console.log('插入数据成功')
})
```

```js
快捷方式 数据对象的每个属性和数据表的字段一一对应就可以快速插入数据
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1',//数据库ip地址
    user: 'root', //登录数据库的账号
    password: 'admin123',//登录数据库的密码
    database: 'my_db'//指定要操作哪个数据库
})
const user = {id:'5',username:'dadada',password:'5541'}
//?表示占位符
const sqlStr = 'insert into my_db.users set ?'
db.query(sqlStr,user,(err,results)=>{
    if(err)return console.log(err.message)
    if(results.affectedRows===1)console.log('插入数据成功')
})
```

# Web开发模式

前后端分离开发， 不利于SEO，但是vue框架的 SSR解决了此问题

服务端渲染采用Session机制      前后端分离采用JWT认证机制，实际上就是token

cookie    存储在浏览器不超过4kb的字符串，发请求时，会将当前域名所有未过期的Cookie都发给服务器，不具有安全性，不应该将敏感信息发给浏览器

session信息保存在服务器，但是发给用户cookie字符串，用户发请求将cookie给服务器，服务器进行认证，解决了安全问题

session需要依靠cookie，但是cookie默认不支持跨域，所以涉及到跨域时，要做很多额外配置

所以，不存在跨域时，推荐使用session，  存在跨域时，使用JWT

TOKEN原理，客户端发请求，服务端进行账号密码的验证，验证通过后，将用户的信息对象进行加密为token字符串，包括三部分，服务器进行相应，发给客户端，客户端进行保存通过本地存储的方式，客户端再次发起请求，通过请求头Authorization，将token发给服务器，服务器把token字符串还原成信息对象，最后将用户请求的内容发给用户。

包括3部分，中间的部分才是用户真正信息，前面后面只是为了进行加密，保证安全性

```http
Authorization: Bearer <token>
```



cors mysql bcryptjs对密码进行加密  @hapi/joi

- 加密之后的密码，**无法被逆向破解**
- 同一明文密码多次加密，得到的**加密结果各不相同**，保证了安全性

出现了一些问题 ，我们定义类型的时候尽量定义大一点，特别是加密后的密码，不然装不下

drop table user  删除表

1. 安装 `@hapi/joi` 包，为表单中携带的每个数据项，定义验证规则：  
2. 安装 `@escook/express-joi` 中间件，来实现自动对表单数据进行验证的功能

npm i jsonwebtoken@8.5.1 生成token

// 为了方便客户端使用 Token，在服务器端直接拼接上 Bearer 的前缀	token: 'Bearer ' + tokenStr,

token: 'Bearer ' + tokenStr,解析token