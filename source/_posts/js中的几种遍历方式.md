---
title: js中的几种遍历方式
titleInUrl: Ways-of-loop-in-javascript
date: 2017-02-12 16:54:40
tags: [js, for, for in, for of, foreach, map]
categories: jsBasic
---
#### 几种循环方式对比
| 名称                                       | 简介                                       | 入参                                       | 原生     | 适用对象                                     | this                                     | 跳出循环块          | 直接进入下一次循环    | 特点                                       |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ------ | ---------------------------------------- | ---------------------------------------- | -------------- | ------------ | ---------------------------------------- |
| for(var i=0;i<v.length;i++) {}           | basic                                    |                                          | yes    | array，伪数组(domList)                       | window                                   | break          | continue     |                                          |
| for(key in list) {}                      | 枚举对象的所有可枚举的属性，包括原型                       | 若list为数组，则key为当前索引；若list为对象，key为当前的属性值，value通过list[one]获得 | yes    | array[key会转化且无序，不适合], domList, objWithProto | window                                   | break          | continue     | **会遍历出原型的值**。他是无序的。key会转换为字符串。           |
| array.forEach(one) {}                    | Array.prototype.forEach                  | one是array中的当前位置的元素                       | yes    | array                                    | window                                   | 无              | 无            | **不能自定义退出循环**                            |
| array.map(function(currentValue, index, array) {},newThis); | Array.prototype.Map                      |                                          | yes    | array                                    | 可以通过第三个参数所在作用域（this的值）                   |                |              | 会将返回值拼成数组                                |
| $.map（array,  function(value,key) {}）    | 原理和$.each是一样的，除了将callback的返回值装入了一个数组，并且返回、this指向window之外。 |                                          | Jquery | array,domList，obj(会有原型)                  | window                                   | 无              | 无            | 同each, 特定条件下会返回原型；并且是无序的；more:他的value和key位置是返的 |
| $.each(aray, function(key,value))        | 源码中，如果满足条件的对象，相当于 for in；否则是普通for        | 若list为数组，则key为当前索引；若list为对象，key为当前的属性值，value通过list[one]获得 | Jquery | array,dom,obj(会有原型)                      | 当前属性的值。如果不是对象类型，则value转换成对象形式，如``Boolean {[[PrimitiveValue]]: true}`` | return false; | return true; | **若``length === undefined``或者``jQuery.isFunction( object );``, 会遍历出原型；他是无序的。** |
| $(selector).each(function(index,element, otherPramter)) | 同上                                       |                                          | Jquery | dom                                      |                                          |                |              |                                          |
| for(let value of array)                  | 弥补forEach的缺点，可以遍历dom，且能退出；弥补for in的缺点，还可以遍历dom(无原型) |                                          | ES6    | array,dom                                | window                                   | break          | continue     | 遍历对象需要额外处理；但他是有序的。                       |



这里我们要测试三种常用的数据种类：

1. array

   ``var array = [1,2,3];``

   这是标准的数组，拥有Array.prototype的属性。

   ``array instanceof Array``为true

2. domList

   ``var dom = $('li')`` 或者 ``var dom = document.getElementsByTagName('li');``

   这不是标准的数组，**不**拥有Array.prototype的属性。不能使用``forEach``和原生的``map``。

   其实它有个名字叫**伪数组**（像数组一样有 `length` 属性，且使用0，1，2这种索引可以获取对象），常见的参数的参数 arguments，DOM 对象列表

   权威指南上判断是维数组的代码：

   ```js
   function isArrayLike(o) {   
       if (o &&                                // o is not null, undefined, etc.
               typeof o === 'object' &&            // o is an object
               isFinite(o.length) &&               // o.length is a finite number
               o.length >= 0 &&                    // o.length is non-negative
               o.length===Math.floor(o.length) &&  // o.length is an integer
               o.length < 4294967296)              // o.length < 2^32
               return true;                        // Then o is array-like
       else
               return false;                       // Otherwise it is not
   }
   ```

   ​

3. objWithProto

   带有原型的。 没有length属性，不能使用``for(i){}``

   ```js
   var objWithProto =  {
           name: 'cc',
           age: 24,
           info: {
               title: 'happybangs'
           }
       }
   };
   objWithProto.__Proto__.protoName = 'protoValue';
   ```

#### 总结

* 对于使用jquery的情况

  array和dom遍历用``each``。object可以使用``each``，但是要注意排除原型。三者需要拼凑返回值时用``map``.

   ``if (array.hasOwnProperty(key)) {// 处理代码 }``

* 对于原生ES5的情况
  * array使用``forEach``(不能跳出循环)
  * dom遍历使用``for(i)``, 或者

    ```js
    [].forEach.call(divs, function(div) {
     div.style.color = "red";
     });
    ```
  * 对象使用``for..in``（记得排除原型）   

* 对于可以原生ES6的情况
  array和dom使用 ``for(const obj of array) {}``,

  遍历对象：

  ```js
  // Object.keys(obj)返回["name", "age", "info"]
  for(const key of Object.keys(objWithProto)) {
      console.log(objWithProto[key]);
  }
  ```

  ​