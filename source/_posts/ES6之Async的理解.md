---
title: ES6之Async的理解
date: 2018-04-25 19:19:32
tags: 小米
categories: 前端
---
async的使用
<!--more-->
- async 方法执行会立即返回一个promise对象，然后继续执行下面的代码。async方法内部的代码是同步执行的，后面的await需要等待前面的await方法执行完后才能执行。
- await 后面可以是表达式，也可以是返回promise的执行函数；
- 重要说明：
async 方法中，如果方法体内没有await，方法体内是变量或者函数声明，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。


```
//async 方法中，如果方法体内没有await，方法体内是变量或者函数声明，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。
async function a(){
    console.log('async inner')
    function g(){};
}

var c = a();
c.then((result)=>{
    //result 为undefined
    console.log('c then',result)
})
```

```
//如果在async 方法中，如果方法体内没有await，return方法体内是变量或者函数声明，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。

async function a(){
    console.log('async inner')
    return function g(){};
}

var c = a();
c.then((result)=>{
    //result function g(){}
    console.log('c then',result)
})
```

```
//如果在async 方法中，如果方法体内没有await，是一个promise对象，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。

async function a(){
   new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('finished')
        },2000)
     }).then((result)=>{
         console.log(result)
     })
}

var c = a();
c.then((result)=>{
    console.log(result)
})
```


```
//如果在async 方法中，如果方法体内没有await，return是一个promise对象，则执行async 方法会立即返回一个promise，且PromiseStatus为pending。

async function a(){

   new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('finished')
        },2000)
     }).then((result)=>{
         console.log()
     })
}

var c = a();
c.then((result)=>{
    console.log(result)
})
```

```
如果async方法体中最后是await，则async方法立即执行完始终返回promise，且status为pending,如果在async的前面有return，则在async返回的promise的then方法中，可以获取到async内部promise的返回值。 总结，async的正确用法为，有await，且前面有return，不管需不需要，最好要这么写return await xxxx。

async function a(){
    return await new Promise((resolve,reject)=>{
        setTimeout(()=>{
            //resolve('finished')
            reject('finished')
        },2000)
     }).then((result)=>{
         console.log()
         return result;
     }).catch(err =>{
         console.log()
         return result;
     })
}

var c = a();
c.then((result)=>{
  //result 为async 内部promise then里面return 的result值。
    console.log(result)
})

```
