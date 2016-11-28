##function
###函数默认的参数值
>在函数的变量列表中初始化变量值，防止函数值没有正确被传递

```
function makeRequest(url, timeout = 2000, callback = function() {}) {

    // the rest of the function

}
```
>在调用这个函数时只需第一个参数值被正确传递，剩下两个参数值均有默认值，当调用函数时，如果剩下两个参数值被正确传递则默认失效

###How Default Parameter Values Affect the arguments Object
>当默认参数值存在时，arguments对象的行为不同，在ES5非严格模式下，arguments对象随着着函数参数值的变化而变化，eg：

```
function mixArgs(first, second) {
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d";
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a", "b");
```
> 输出
```
true
true
true
true
```
>在非严格模式下arguments对象通常随着参数的变化而变化。因此，当first和second被定义新值，arguments[0]和arguments[1]也随之变化，在严格模式下，　arguments没有这种特性，eg：

```
function mixArgs(first, second) {
    "use strict";

    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d"
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a", "b");
```
>得到结果
```
true
true
false
false
```

###Default Parameter Expressions








