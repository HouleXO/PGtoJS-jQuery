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
$(elements).bind('event', handlerFx)；
```

**绑定多个事件**

```js
$( "#foo" ).bind( "mouseenter mouseleave", handlerFx);
```

**停止监听**

```js
$(elementOrSelect).unbind('event', handlerFx);
```

### 利用事件委托

优先使用时间代理，jQuery模拟了所有的事件冒泡，使所有事件到支持事件冒泡并兼容所有浏览器。

**切换条目内容**

```html
<ul id="items">
    <li>
        <div>
            <p>Data 1</p>
            <p>Data 2</p>
        </div>
    </li>
    <li>
        <div>
            <p>Data 1</p>
            <p>Data 2</p>
        </div>
    </li>
    <li>
        <div>
            <p>Data 1</p>
            <p>Data 2</p>
        </div>
    </li>
</ul>
```

```js
$('#items').on('click', 'a', function(){
        var trigger = $(this);
        if(!trigger){ return true; }
        var content = trigger.parent().next();
        if(!content){ return true; }
        content.toggle();
        trigger.html(content.css("display") === "none" ? 'Open' : 'Close');
});
 $('ul li').each(function(){
        $(this).prepend('<p><a href="#" class="toggler">Open</a></p>');
        $(this).children('div').hide();
});
```

      ![](/assets/GIF.gif)

**使用事件代理**

```js
$('a.toggler', '#items').live('click', handlerFx); （jQuery1.7之后弃用）
$('a.toggler').on('click', '#items', handlerFx);
```

### 将行为和自定义事件解耦

**监听一个自定义事件**

```js
// -- 通过多余的参数传入额外的数据
$(elementOrSelector).bind('event', handlerFx)；
```

**处罚一个自定义事件**

```js
$(elements).trigger('event');
$(elements).trigger('event', { foo: 'bar', baz: 2 });
$(elements).trigger('event', ['bar', 42]);
```

### 模拟后台处理

**调度和停止代码的执行**

```js
// 利用定时器模拟后台处理需要两个核心方法
var handle = window.setTimeout(callback, intervalInMs);
window.clearTimeOut(handle);
```

**让用户切换后台处理**

```html
<body>
    <script src="./libs/jQuery321.js"></script>
    <h1>Simulating background processing</h1>
    <!-- START:MAIN -->
    <p id="progress">
        <span class="visual"></span>
        <span class="figure">0%</span>
    </p>
    <p>
    <input type="button" id="btnToggle" value="Toggle" />
    <input type="button" id="btnOtherTask" value="Do some other stuff" />
</body>
```

```css
#progress {
            border: 2px solid gray; text-align: center;
            background: white; color: black; position: relative; width: 30em;
            font-family: sans-serif;
        }
#progress .over50 { color: white; }
#progress .visual {
            position: absolute; left: 0; top: 0; height: 100%; width: 0;
            background: green; z-index: 1;
        }
#progress .figure { position: relative; z-index: 2; font-weight: bold; }
```

```js
(function() {
        // START:MAIN
        var CHUNK_INTERVAL = 25; // ms.
        var running = false, progress = 0, processTimer;

        function runChunk() {
            window.clearTimeout(processTimer);
            processTimer = null;
            if (!running) return;
            // Some work chunk.  Let's simulate it:
            for (var i = 0; i < 10000; i += Math.round(Math.random()*5)){}

            ++progress;
            updateUI(); // See source archive -- just updates a progressbar

            if (progress < 100) {
                processTimer = window.setTimeout(runChunk, CHUNK_INTERVAL);
            } else {
                progress = 0;
                running = false;
            }
        }

        function toggleProcessing() {
            running = !running;
            if (running) {
                processTimer = window.setTimeout(runChunk, CHUNK_INTERVAL);
            }
        }
        // END:MAIN

        var progressbar, visual, figure;

        function updateUI() {
            visual.css('width', + progress + '%');
            progressbar[progress < 50 ? 'removeClass' : 'addClass']('.over50');
            figure.text(progress + '%');
        }

        $(document).ready(function(){
            $('#btnToggle').bind('click', function(){
                toggleProcessing();
            });
            $('#btnOtherTask').bind('click', function() {
                $('h1').first().append(', yeah');
            });
            progressbar = $('#progress');
            visual = progressbar.children('.visual');
            figure = progressbar.children('.figure');
        });
    })();
```

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;![](/assets/GIF1.gif)

