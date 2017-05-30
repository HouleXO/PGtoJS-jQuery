**动态选择方法及属性**

方括号操作符

```js
object['propertName']          // => object.propertyName
object['methodName'](arg1)     // => object.methodName(arg1)
```

切换行为

```js
// 根据shouldBevisible的值来调用show()或者hide（）
element[shouldBevisible ? show() : hide();
// 避免IE中动画带来的巨大开销
element[isIE ? 'simpleEffect' : 'complexEffect']();
```

拼接方法名称

```js
element[(enable ? 'add' : 'remove') + 'ClassName']('enabled');
```

**通过模块模式实现代码访问控制**

```
    为通过var关键字声明的标识符和函数创建一个私有作用域，使只有定义在这个作用域的函数才能访问这些数据。
```

```js
(function(){
    var privateField = 22;
    function innerFunc() {
        notSoPrivate = 23;
        return notSoPrivate;
    }
    alert(privateField); // => 22
    innerFunc();
    alert(notSoPrivate); // => 23
})();
alert(typeof privateField); // => Undefined
alert(notSoPrivate); // => 23 (变量被泄露在外部)
```

```
   私有属性
```

```js
var obj = (function(){
    var privateField = 22;
    var publicField = 'fooabr';
    function processInternals() {  alert( 'Internal stuff : ' + privateField); }
    function run (){
       processInternals();
       alert('still private stuff: ' + privateField); 
       alert('public stuff: ' + publicField); 
    }
    // 暴露私有属性和方法
    return {
        publicField : publicField,
        run : run
    };
})();
obj.run(); // 弹出三个对话框
obj.publicField; // 'fooabr'
// 变量未被泄露在外部
obj.processInternals; // Undefined
obj.privateField; // Undefined
```

**使用可选/可变/命名参数**

声明参数（命名参数）

```js
function repeat(rant, times){
    while (--times >= 0){
        alert(rant);
    }
}
repeat('I am hungery!!', 5); // 弹出5个对话框
```

动态获取不定数量的参数

```js
function repeat(times){
    while (--times >= 0){
        for(var index=1, len = arguments.length; index < len; ++index){
            alert(arguments[index]);
        }
    }
}
repeat(2, 'ccc', 5); // 连续弹出4个对话框
```

为可选参数设置默认值

```js
function repeat(times, rant){
    if (undefined === rant){
        rant = 'IE6 must die!'
    }
    while(--times >= 0){
        alert(rant);
    }
}
repeat(3); // 连续弹出三个对话框
repeat(3, 'so should IE7...'); // 连续弹出三个IE7相关的对话框
```

用字面量对象实现伪命名参数

```js
function repeat(options){
	options = options || {};
	for(var opt in {repeat.defaultOptions || {}}){
		if(!opt in options){
			options[opt] = repeat.defaultOptions[opt];
		}
	}
	for(var index=0; index<options.times; ++index){
		alert(options.rant);
	}
}
repeat.defaultOptions = {times:2, rant:'IE6 must die!!'};
repeat(); // 弹出2个与IE6有关的对话框
repeat(3); // 弹出3个与IE6有关的对话框
repeat(times:2, rant:'FLASH must die!!'); // 弹出2个与flash有关的对话框
```



