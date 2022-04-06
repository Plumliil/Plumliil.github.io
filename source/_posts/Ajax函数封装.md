---
title: Ajax函数封装
date: 2021-08-15 23:39:11
tags:
  - 前端篇
  - ajax
---



```javascript
ajax({
    url:'http://localhost:3000/xxx',
    data:{
        name:'zhangsan',
        age:20
    },
    success:function (data){
        console.log(data)
    }
})
```

<!--more-->

```javascript
function ajax(options){
    // 存储默认值
    var defaults={
        type: 'get',
        url:'',
        data:{},
        header:{
            'Content-Type':'application/x-www-form-urlencoded'
        },
        success:function (){},
        error:function (){}
    }
    // 使用options对象中的属性覆盖defaults中的属性
    Object.assign(defaults,options)
    // 创建ajax对象
    var xhr=new XMLHttpRequest();
    // 拼接请求参数的对象
    var params='';
    // 循环用户传递过来的对象格式参数
    for(var attr in defaults.data){
        params+=attr+'='+defaults.data[attr]+'&';
    }
    <!--more-->
    // 参数后边的&截取掉
    params=params.substr(0,params.length-1);
    // 判断请求方式
    if(defaults.type=='get'){
        defaults.url=defaults.url+'?'+params;
    }
    // 配置ajax对象
    xhr.open(defaults.type,defaults.url)
    if(defaults.type=='post'){
        var contentType=defaults.header['Content-Type'];
        xhr.setRequestHeader('Content-Type',contentType);
        // 判断用户希望的请求参数格式类型
        // 如果类型为json
        if(contentType=='application/json'){
            // 传递json类型的参数
            xhr.send(JSON.stringify(defaults.data))
        }else{
            // 传递普通类型的参数
            xhr.send(params);
        }
    }else{
        xhr.send();
    }
    xhr.onload=()=>{
        // xhr.getResponseHeader();
        // 获取响应头数据
        var contentType=xhr.getResponseHeader('Content-Type');
        var responseText=xhr.responseText;
        // 如果响应中包含application/json
        if(contentType.includes('application/json')){
            // json字符串转化为json对象
            responseText=JSON.parse(responseText);
        }
        // 当http状态码等于200时
        if (xhr.status==200){
            // 请求成功
            defaults.success(responseText,xhr);
        }else {
            // 请求失败
            defaults.error(responseText,xhr);
        }
    }
}
```

