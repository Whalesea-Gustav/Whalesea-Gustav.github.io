---
title: JSON in JavaScript
date: 2018-09-22 02:00:00
tags: [JavaScript, Font-end]
categories: 网络编程
---

---
# JSON标准
JSON实际上是JavaScript的一个子集，是一种数据交换格式，有如下的数据类型
+ number:          和JavaScri中`number`一致
+ boolean:         `true`和·`false`
+ string:          `string`
+ null:            `null`
+array:            `Array`的表达方式`[...]`
+ object:          `object`的表达方式 `{...}`
JSON字符集必须是**UTF-8**，表示多语言就没有问题了。为了统一解析，JSON的字符串规定必须用双引号`""`，**Object的键**也必须用双引号`""`

JSON应用过程 
1. 把任何JavaScript值序列化成为一个JSON格式的字符串，然后通过网络进行传输
2. 把接受到的JSON格式字符串反序列化为一个JavaScript值，就可以在JS中直接应用

---
# 序列化 serialization
syntax
>  JSON.stringify(value[, replacer[, space]])

## 参数
+ **value**    : 需要convert成JSONstring的值
+ **replacer** : A **function** that **alters the behavior of the stringification process**, or **an array of String and Number objects** that serve as **a whitelist for selecting/filtering the properties of the value object** to be included in the JSON string. If this value is null or not provided, all properties of the object are included in the resulting JSON string
+ **space**    : A String or Number object that's used to **insert white space** into the output JSON string for **readability** purposes.**If this is a Number**, it indicates the number of space characters to use as white space; this number is capped at 10 (if it is greater, the value is just 10). Values less than 1 indicate that no space should be used.**If this is a String**, the string (or the first 10 characters of the string, if it's longer than that) is used as white space. If this parameter is not provided (or is null), no white space is used

需要注意的几点
1. replacer 参数，replacer需要接收key，value两个参数，返回value的值，replacer以一种map的方式处理对象，并返回JSON字符串
2. replacer 作用于序列化的value是**recursive**。会影响例如对象中的对象，`array`中的`array`。
3. replacer 如果作用于对象，如果返回的是`undefined`，该对象的属性将不会被包含在JSON字符串中，其起到一个过滤的作用(filte out)
4. space    自动在对象每个属性后面添加换行符，前面的缩进由传递进去的字符或者数字个space构成
```javascript
function replacer(key, value) {
  // Filtering out properties
  if (typeof value === 'string') {
    return undefined;
  }
  return value;
}

var foo = {foundation: 'Mozilla', model: 'box', week: 45, transport: 'car', month: 7};
JSON.stringify(foo, replacer);
// '{"week":45,"month":7}'
```
```javascript
'use strict';

var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp']
};
var json1 = JSON.stringify(xiaoming);
console.log(json1);
/*
{
  "name": "小明",
  "age": 14,
  "gender": true,
  "height": 1.65,
  "grade": null,
  "middle-school": "\"W3C\" Middle School",
  "skills": [
    "JavaScript",
    "Java",
    "Python",
    "Lisp"
  ]
}
*/
```

## `toJSON()`
the toJSON() method customizes JSON stringification behavior: instead of the object being serialized, the value returned by the toJSON() method when called will be serialized.
其定制了序列化的行为
JSON.stringify() 会向JSON传递一个参数
1. 如果包含`toJSON`方法的对象是作为一个属性值，那么传入的key是这个属性的名称
2. 如果包含`toJSON`方法的对象是`Array`中的一项，那么传入的`key`是index
3. 如果包含`toJSON`方法的对象直接被序列化，那么传入的参数是空字符串`""`
```javascript
var obj = {
    data : 'data',
    toJSON(key) {
        if (key) {
            return  `Now I am a nested object udner key '${key}'`;
        } else {
            return this;
        }
};

JSON.stringify(obj);               //'{"data" : "data"}
JSON.stringify({obj});            //'{"obj" : "Now I am a nested object under key 'obj'"}'
JSON.stringify([obj]);            //'["Now I am a nested object under key '0'"]'
```
---
# 反序列化 deserialization
对于一个JSON字符串，`JSON.parse()`可以将其转化成一个JavaScript对象
> JSON.parse(text[, revivor])

revivor类似replacer，转换解析出来的属性
```javascriot
var obj = JSON.parse('{"name":"小明","age":14}', function (key, value) {
    if (key === 'name') {
        return value + '同学';
    }
    return value;
});
console.log(JSON.stringify(obj));        // {name: '小明同学', age: 14}
```

---
# 具体例子 : 格式化JSON数据
进入JSON数据网页
在console中使用 用`document.QuerySelector` 函数捕获到元素
```javascript
//获得 pre标签下的html文本
var pre = document.querySlector('pre');
``` 
`innerHTML`得到转义之后的真实数据，例如`<` `>`在转义之后的`&lt;` `&gt;`等
`innerText` 则是得到未转义之前的显示形式，也是符号原有的样式
```javascript
var pre_string = pre.innerText;
```
将获得的JSON，反序列化(parse)
```javascript
var pre_date = JSON.parse(pre_string);
```
为了获得更直观的数据表现，需要用到space参数
```javascript
console.log(JSON.stringify(pre_data, null, 4);
```
