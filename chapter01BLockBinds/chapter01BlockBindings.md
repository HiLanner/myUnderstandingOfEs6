#块绑定
##var变量提升
###变量提升
1. **变量提升**使用var定义的变量等同于在函数内顶部定义如果是函数外部的则类似在在全局作用域中定义，例如：

```
function getValue(condition){
		console.log("1////"+value);
		if (condition) {
			var value = "blue";
			console.log("2////"+value);
		} else {
			console.log("3////"+value);
			return null;
		}
		console.log("4////"+value);
	}
	getValue(true);
```

这相当于

```
function getValue(condition) {

    var value;

    if (condition) {
        value = "blue";

        // other code

        return value;
    } else {

        return null;
    }
}

```
###块级定义
1. 必须在function内部
2. 在块内（通过｛｝符号表示）

**let关键字**
>let关键字定义变量和var的使用相同，你可以使用let代替var，但是这会限制变量的作用域只在当前代码块内有效，并且变量不会提升到顶部，例如:

```
function getValue(condition) {

    if (condition) {
        let value = "blue";

        // other code

        return value;
    } else {

        // value doesn't exist here

        return null;
    }

    // value doesn't exist here
}
```
>这使得getValue函数表现的类似C语言家族，使用let不会使变量提升并且比变量value不会不会再if语句块外访问到，如果condition的值为false那么value就是没有被定义和初始化

**let不会重新定义变量**
> 如果一个变量在一个域内已经被定义，如果用let再次定义回引发错误，例如：

```
var count = 30;

// Syntax error
let count = 40;
```
> 在这个例子中，count被定义了两次：一次使用的是var，还有一次使用的是let，因为let不会在相同的作用域中重新定义一个标识符，所以let声明会抛出一个错误，另一方面如果let在他的包含域内定义一个已有变量，这样不会抛出错误，例如：

```
var count = 30;

// Does not throw an error
if (condition) {

    let count = 40;

    // more code
}
```
> 这个let的定义不会抛出错误因为他在if的代码段内创建一个新的名称为count的变量，在这个if代码块内，新的变量count会屏蔽掉全局的变量count直到if代码块结束

**const关键字**
> 使用const定义的变量会被当成常量，意味着变量一旦被const定义便不会被改变，每一个被const定义的变量必须被初始化

```
// Valid constant
const maxItems = 30;

// Syntax error: missing initialization
const name;
```
**const VS let**
> const和let相同点： 
> 
> 1.属于块级的定义，块级外无法访问到let或者const定义的变量，而且不会变量提升，例如

```
if (condition) {
    const maxItems = 5;

    // more code
}

// maxItems isn't accessible here
```
> 2.还有一点是在一个相同的作用域内当一个变量已经被标识符定义，再次使用const定义该标识符的时候会抛出错误

```
var message = "Hello!";
let age = 25;

// Each of these would throw an error.
const message = "Goodbye!";
const age = 30;
```
> const和let的不同点：已经被const定义的变量不能再被重新赋值，否则会抛出错误不仅在严格模式和非严格模式中，例如：

```
const maxItems = 5;

maxItems = 6;      // throws error
```
> 和其他语言一样被定义的变量不能在赋新值，和其他语言不一样的是,const定义的object内的属性值可以被改变，例如：

```
const person = {
    name: "Nicholas"
};

// works
person.name = "Greg";

// throws an error
person = {
    name: "Greg"
};
```
>  改变属性值不会引发错误的原因是因为，这改变是变化person内部的属性值并不是改变person绑定的值（object）

**TDZ**
>
**Block Binding in Loops**
> 在循环中，使用var时，例如：

```
for (var i = 0; i < 10; i++) {
    process(items[i]);
}

// i is still accessible here
console.log(i);                     // 10
```
>  而使用let时，例如：

```
for (let i = 0; i < 10; i++) {
    process(items[i]);
}

// i is not accessible here - throws an error
console.log(i);
```
**Function in Loops**
> 使用关键字var可导致循环内部的函数出现很多错误，因为循环变量在循环作用域外部也可以被访问到，例如：

```
var funcs = [];

for (var i = 0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}

funcs.forEach(function(func) {
    func();     // outputs the number "10" ten times
});
```
> 这是因为i在每次循环的迭代中被公用，这意味着在循环内部创建的函数共享一个变量。当循环结束时，变量i＝10
> 为了解决这个问题，开发者一般在循环内部使用立即执行函数来强制复制变量的副本，例如：

```
var funcs = [];

for (var i = 0; i < 10; i++) {
    funcs.push((function(value) {
        return function() {
            console.log(value);
        }
    }(i)));
}

funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
});
```
>上面的例子在函数内部使用了一个立即执行函数，变量i被传递到立即执行函数内，在立即执行函数内部会创建他自己的副本，这是函数每次迭代中会使用的值，所以每次在循环中调用函数的时候还得到期待的0到9

**Let Declarations in Loops**
>let关键字可以通过模拟立即执行函数的在上个例子中的作用简化循环，在每次循环迭代中let会创建一个新的变量（和之前变量值同名）并且初始化它，这样可以一方面省略立即执行函数一方面得到预期的效果，例如：

```
var funcs = [];

for (let i = 0; i < 10; i++) {
    funcs.push(function() {
        console.log(i);
    });
}

funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
})
```
> 这样使用起来很像是使用var配合立即执行函数但是使用let会更加简洁，let关键字通过循环每次都会创建一个新的变量i，所以每一次循环内部创建的函数都会得到i的副本，每个i的副本都会拥有在循环开始时分配的值，在for－in和for－of也一样

```
var funcs = [],
    object = {
        a: true,
        b: true,
        c: true
    };

for (let key in object) {
    funcs.push(function() {
        console.log(key);
    });
}

funcs.forEach(function(func) {
    func();     // outputs "a", then "b", then "c"
});

```
> for-in循环展示出和for相同的功能，在每一次循环中，会创建一个新的key值绑定，而且函数会有自己的key变量值副本

**Const Declarations in Loops**
> 在for循环中，使用const会引发错误，例如：

```
var funcs = [];

// throws an error after one iteration
for (const i = 0; i < 10; i++) {
    funcs.push(function() {
        console.log(i);
    });
}
```
> 这里变量i值被const定义为常量，所以在第二次for循环中执行i＋＋会引发错误
> 但是在使用for－in或者for－of时会达到和let一样的效果，例如：

```
var funcs = [],
    object = {
        a: true,
        b: true,
        c: true
    };

// doesn't cause an error
for (const key in object) {
    funcs.push(function() {
        console.log(key);
    });
}

funcs.forEach(function(func) {
    func();     // outputs "a", then "b", then "c"
});
```
> 这是因为在每次循环迭代中，都会创建一个新的绑定而不是去修改已经存在的值

**Global Block Bindings**
> let和const和var在全局作用域的行为不同，当var在全局作用域中被使用时，它会创建一个新的全局变量（全局对象的属性window），这意味着可以用var重写一个已经存在的变量，例如：

```
// in a browser
var RegExp = "Hello!";
console.log(window.RegExp);     // "Hello!"

var ncz = "Hi!";
console.log(window.ncz);        // "Hi!"
```
> 如果使用在全局作用域中使用let或const，一个新的绑定在全局作用域中被创建但是全局对象并没有被添加属性，这也意味着你不能用let和const重写全局变量，而是只能覆盖它。例如：

```
// in a browser
let RegExp = "Hello!";
console.log(RegExp);                    // "Hello!"
console.log(window.RegExp === RegExp);  // false

const ncz = "Hi!";
console.log(ncz);                       // "Hi!"
console.log("ncz" in window);           // false
```
> 在这个例子中，使用let和const创建的变量屏蔽了全局同名变量的访问，这意味着window.RegExp和RegExp并不相同，同理，const定义的ncz创建了一个绑定而不是创建了一个全局对象的属性，所以当你不想在全局作用域中创建全局对象使用let和const会更安全






