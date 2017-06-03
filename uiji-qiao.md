### 打造漂亮的tooltip（略）

### 制作友好的弹窗

```html
<p>
    The great thing about <a class="popup" target="_blank"
                             href="http://pragprog.com/titles/pg_js">Pocket Guide to JavaScript</a>
    is that it focuses on a bunch of specific, useful tasks.
</p>
```

```
(function() {
        var POPUP_FEATURES = 'status=yes,resizable=yes,scrollbars=yes,' +
            'width=800,height=500,left=100,top=100';

        function hookPopupLink(evt) {
            var trigger = $(this);
            if (!trigger) return;
            var wndName = trigger.attr('target');
            var myURL = trigger.attr("href");
            if (!trigger) return;
            trigger.blur();
            window.open(myURL, wndName, POPUP_FEATURES).focus();
            evt.preventDefault();
        }

        $('document').ready(function(){
            $("a.popup").on('click',hookPopupLink);
        });
 })();
```

### 预载入图片

实现预载入功能的HTML代码

```html
<ul id="products">
    <li><h2><a href="http://pragprog.com/titles/cppsu">
        <img rel="preloadZoom" src="./images/cppsu.jpg" alt="" />
        <span>Prototype and script.aculo.us</span>
    </a></h2></li>
    <li><h2><a href="http://pragprog.com/titles/vsscala">
        <img rel="preloadZoom" src="./images/vsscala.jpg" alt="" />
        <span>Programming Scala</span>
    </a></h2></li>
</ul>
```

实现预载入功能的JavaScript代码

```css
img { border: none; }
        #products { margin: 0; padding: 0; }
        #products li {
            list-style-type: none; float: left;
            margin-left: 1em; padding: 0.25em 0.5em; border: 1px solid silver;
            font-family: sans-serif; text-align: center;
        }
        #products img { display: block; }
        #products li h2 { font-size: 100%; }
        #products li a { text-decoration: none; }
        #products li a span { text-decoration: underline; }
```

切换（载入完成的）图片

```js
(function() {
        $('document').ready(function(){
            var imgExt = /(\.\w+$)/;
            $('ul img').each(function() {
                    var imgFile = $(this).attr("src");
                    var preloadImage = new Image();
                    preloadImage.src = imgFile.replace(imgExt, '_closeup$1');
                    $(this).hover(
                        function(){
                            $(this).attr("src", preloadImage.src);
                        },
                        function(){
                            $(this).attr("src", imgFile);
                        }
                    );
             });
        });
})();
```

效果

   ![](/assets/GIF2.gif)

### 创建光箱效果

&emsp;&emsp;通过FancyBox jQuery插件来实现把缩略图放大成原始图片的光箱效果。

&emsp;&emsp;**参考文章**：[jQuery：改进图像](http://blog.csdn.net/lovejulyer/article/details/72722054)

### 实现“无限翻页”

```js
(function() {
        // START:lowEnough
        // 判断文档底部离当前显示区域底部距离，跨浏览器兼容
        function lowEnough() {
            var pageHeight = Math.max(document.body.scrollHeight,
                document.body.offsetHeight);
            var viewportHeight = window.innerHeight ||
                document.documentElement.clientHeight ||
                document.body.clientHeight || 0;
            var scrollHeight = window.pageYOffset ||
                document.documentElement.scrollTop ||
                document.body.scrollTop || 0;
            // Trigger for scrolls within 20 pixels from page bottom
            return pageHeight - viewportHeight - scrollHeight < 20;
        }
        // END:lowEnough

        // START:loop
        function checkScroll() {
            if (!lowEnough()) return pollScroll();
            var spinner = $('#spinner');
            spinner.show();
            $.get('more.php', processData);
            function processData(data, dataStatus){
                if(dataStatus === "success"){
                    spinner.before(data);
                    spinner.hide();
                    pollScroll();
                }else{
                    spinner.prepend('<p>Please try again!</p>');
                }
            }
        }
        function pollScroll() { setTimeout(checkScroll, 100); }
        pollScroll();
        // END:loop
})();
```

**效果**：

![](/assets/GIF3.gif)

### 在载入内容时保持显示区域







