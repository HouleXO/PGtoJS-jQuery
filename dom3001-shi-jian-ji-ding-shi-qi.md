### 获取DOM元素的引用

  元素引用需要先指定上下文节点或根节点，然后以这个节点作为根元素，在其之上进行选择操作。尽可能缩小上下文范围，以争取更快的执行速度和更小的内存占用。

**通过ID属性获取对应元素**

```js
$('#element')
```

**通过XPath/CSS选择来获取元素**

```js
$('selectors')
```

**元素间移动（遍历DOM）**

```js
// 从具有指定id属性的元素开始，到达最近的class属性为category的<h2>元素
// 然后遍历这个元素的兄弟元素直到遇到一个class 属性为summary的元素
$('#someDeeplyNestedElement').find('h2 .category').find('.summary');

// 找到带有名为siri的CSS class的元素
// 设置这些元素的直接容器的CSS文本缩进属性
$('.siri').css('text-indent', '-999px');
```

### 动态修饰内容

**设置元素的样式**

```js
$(element).css('prop', 'value');
$(element).css({prop: 'value', prop2: 'value2'});
```

**获取元素的样式**

```js
$(element).css('prop');
```

### 修改元素的内容

**更新元素的全部内容**

```js
$(element).html('<p>new internal HTML</p>');
$(element).text('The <div> and <span> elements carry no inhreent semantics')
```

**向元素中注入其他内容**

```js
$(element).before('<p>new content</p>');
$(element).prepend('<p>new content</p>');
$(element).append('<p>new content</p>');
$(element).after('<p>new content</p>');
```

### 在DOM加载完成后运行脚本

```js
$(document).ready(function(){
  // 在这里写你的代码...
});
```

### 监听及停止监听事件

**在某个元素监听事件**

```js
$(elementOrSelect).bind('event', handlerFx);
```

**在多个元素上监听事件**

```js
$(elements).bind('event', handlerFx);
```

**停止监听**

```js
$(elementOrSelect).unbind('event', handlerFx);
```

### 利用事件委托-优先使用时间代理

**切换条目内容**

```

```

**使用时间代理**

```js
$('a.toggler', '#items').live('click', handlerFx);
```

### 将行为和自定义事件解耦

**监听一个自定义事件**

**处罚一个自定义事件**

### 模拟后台处理

**调度和停止代码的执行**

**让用户切换后台处理**

