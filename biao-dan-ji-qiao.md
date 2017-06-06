### 禁止使用提交按钮

**jquery如何设置与去除disabled属性？五种方法**

//两种方法设置disabled属性

```js
 $('#areaSelect').attr("disabled",true);
 $('#areaSelect').attr("disabled","disabled");
```

//三种方法移除disabled属性

```js
 $('#areaSelect').attr("disabled",false);
 $('#areaSelect').removeAttr("disabled");
 $('#areaSelect').attr("disabled","");
```

### 提供输入长度反馈

用HTML指定长度

```html
<h1>提供输入长度反馈</h1>
<form>
    <!-- START:main -->
    <p>
        <label for="edtDescription">Description</label>
        <textarea id="edtDescription" name="description" cols="40"
                  rows="5" class="maxLength100"></textarea>
    </p>
    <!-- END:main -->
</form>
```

为最大长度域绑定反馈事件

```js
// START:init
var current, maxLength, delta;

function bindMaxLengthFeedbacks() {
    var mlClass, feedback;
    $("textarea[class^='maxLength']").each(function(){

        var field = $(this);
        // var type = field[0].tagName.toLowerCase();
        mlClass = field.attr('class').match(/\bmaxLength(\d+)\b/)[0];
        maxLength = parseInt(mlClass.replace(/\D+/g, ''), 10);
        field.maxLength = maxLength;
        delta = maxLength;
        field.parent('p').addClass('lengthFeedback');
        feedback = ('<span class="feedback">Remaining:'+ delta +'</span>');
        field.after(feedback);
        // updateFeedback(field);

        field.on('keyup', updateFeedback);
        field.on('keypress', updateFeedback);
        field.onchange = field.onkeyup;
    });
}
// END:init
```

及时给出反馈

```js
// START:update
function updateFeedback(){
   current = $(this)[0].value.length;
   delta = current < maxLength ? maxLength - current : 0;
   if (current > maxLength) {
       $(this)[0].value = $(this)[0].value.substring(0, maxLength);
       delta = 0;
   }
   $(this).next("span").html("Remainning:" + delta );
}
// END:update

$('document').ready(bindMaxLengthFeedbacks);
```

### 同时选择或反选多个ckeckbox

### 表单验证：基本技巧

### 表单验证：进阶技巧

### 表单验证：高级技巧

### 在表单中提供动态的帮助tooltip

### 自动完成输入

### 使用动态多文件上传



