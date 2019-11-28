# 同步异步
 JS按理来说是从上往下解读代码，它是单线程的（同一时间只能做一件事情）

 事件调用 -> 把任务交给了事件引擎（所有的js事件全部都是异步的）

同步:
    代码从上往下依次执行，如果一个地方卡住了，下面的代码就不执行了
异步:
    代码从上往下依次执行，如果一个地方卡住了，是不会阻止下面代码执行的

    定时器,所有的事件  promise

    js先执行主线程的代码,如果主线程有异步代码,比如定时器,promise或者事件那么会把异步代码放到异步队列中存储,当异步代码压入到主线程中执行,压入的方式是如果有微任务就先执行微任务,执行完微任务再执行宏任务,当主线程空间的时候执行压入的代码,执行完之后再从异步队列中压入异步代码到主线程中,这个过程叫事件循环.

    注意的是执行完微任务是第一层的,如果在宏任务中开个微任务,那么先执行宏任务,再执行宏任务中的微任务

    setTimeout(function(){
            promise()
        })

    异步的操作是不容易进行维护开发的,同步操作才利于维护开发(上面的代码执行完才会执行下面的,有序的)

    promise是解决异步编程顺序问题的(也就是说,让异步的代码同步执行)

  # Promise  ->承诺

    为什么要用promise?
        promise解决了异步编程的问题

        在then里面就走"同步"

  # 如何使用promise?
     new Promise(function(resolve,reject){
         主线程
         当异步代码执行完,通过异步代码的结果去调用resolve或者reject

         异步代码有可能报错或者错误,如果报错或者错误就执行reject
         一般都是resolve(放异步的结果)
     })

     它有一个返回值,返回值是promise对象,这个对象有then方法
     then(成功函数,失败函数)
     第一个then　（微任务）
        成功函数里面的参数就是异步的结果

    第二个then  (微任务)
        第一个then的(返回值)


虽然promise解决了异步编程的问题，但是在then的外面还是异步的

没有promise也能进行开发，只不过维护起来麻烦点

 then中包含2个函数，第一个函数是成功之后的回调，第二个函数是失败之后的回调

 finally：不管成功还是失败都会进的回调函数

  如果代码有可能会报错，下面的代码是不会执行的，如果使用try，catch
        那么try中的代码报错会进catch，报错是不会影响后面代码执行的
        try{
            
        }catch(e){

        }
   第一个then的返回值，是第二then的参数 ....
        fetch().then(function(d){
            return d.json();
        }).then(function(d){
            console.log(d); //d就是d.json()
        });

        当进第一个then的时候，d就是返回的数据，但是这个数据是被promise包了一层
        d.json() -> '[]'->[]


        JSON ->  长得像对象和数组的字符串，本质是字符串

        '[]'JSON  ->  []数组
        '{}'JSON  ->  {} 对象


        JSON取值是不方便的，可以使用JSON.parse(),把JSON转成对象

        parse必须为标准的JSON格式才成功转换
        '{"name":"zf"}'
        '[]' -> []

        对象转JSON -> JSON.stringify() 的副作用是函数和undefined会被过滤掉
        [] -> '[]'
# promise all
如果是用promise.all,三个请求都要请求完毕(不能保证谁先执行谁后执行完),才做别的事情
# race 
在数组中只要有一个异步成功就返回这次成功的结果,all是数组中的异步操作都要成功才返回成功结果

# json是字符串，把json转成对象使用JSON.parse  //字符串转对象


console.log(JSON.stringify({a:5,fn:function(){},u:undefined,n:null}))   //   对象转字符串   把函数和undefined过滤
  
console.log(JSON.parse("{'name':'zf'}")) //报错，因为不是标准格式
console.log(JSON.parse('{"name":"zf"}'))
    