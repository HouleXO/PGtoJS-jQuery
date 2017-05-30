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

### 通过模块模式实现访问控制





