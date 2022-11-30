---
title: axios
date: 2022-10-15 15:30:38
tags:
---

axios外部库，需要npm安装和引入        axios.get().then(()=>{},()=>{})

注意里面两个箭头函数，第一个表示成功，传入response形参，获得的数据应该是response.data，因为返回给我们的是对象，我们需要拿到data数据

const axios = require('axios');CommonJS语法引入   还有 `  import   axios    from   'axios'`