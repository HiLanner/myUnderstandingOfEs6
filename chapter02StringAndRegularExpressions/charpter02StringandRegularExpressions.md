## String and Regular Expressions
> string是程序语言中最重要的数据类型之一

###better unicode support
> es6之前

###the regular expression y flag
>y标志符作用和g标志符差不多，每次都会从上次匹配的位置结束后的第一个位置开始，只不过g标志符是从接下来的所有字符串中开始匹配，而y必须是在上次结束的位置开始，这就是粘连的特性。例如：

```
var text = "hello1 hello2 hello3",
    pattern = /hello\d\s?/,
    result = pattern.exec(text),
    globalPattern = /hello\d\s?/g,
    globalResult = globalPattern.exec(text),
    stickyPattern = /hello\d\s?/y,
    stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello1 "
console.log(stickyResult[0]);   // "hello1 "

pattern.lastIndex = 1;
globalPattern.lastIndex = 1;
stickyPattern.lastIndex = 1;

result = pattern.exec(text);
globalResult = globalPattern.exec(text);
stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello2 "
console.log(stickyResult[0]);   // Error! stickyResult is null
```
> 其中indexof是指定正则下次开始匹配的位置，indexof值为1时则从test的第二个字符开始匹配，不过需要注意的一点是，/hello\d\s?/会忽略indexof的设置
> 有两点需要注意，1：lastIndex只在正则表达式的exec()方法和test()方法中有效；2:
> 
> 
> 
> 
> 
> 
> 
> 
> 