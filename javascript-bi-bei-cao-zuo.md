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

        为通过var关键字声明的标识符和函数创建一个私有作用域，使只有定义在这个作用域的函数才能访问这些数据。

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

       私有属性

```js
var obj = (function(){
    var privateField = 22;
    var publicField = 'fooabr';
    function proc() {
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



