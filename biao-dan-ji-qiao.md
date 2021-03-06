### 禁止使用提交按钮

**jquery如何设置与去除disabled属性？五种方法**

```js
 //两种方法设置disabled属性
 $('#areaSelect').attr("disabled",true);
 $('#areaSelect').attr("disabled","disabled");
```

```js
 //三种方法移除disabled属性
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

```css
label { display: block; }
p.lengthFeedback { padding-bottom: 1.2em; }
p.lengthFeedback span.feedback {
    position: absolute; font-size: 90%;
    font-family: sans-serif; color: silver; text-align: right;
    display: block;
}
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

```js
(function() {
    // START:main
    function toggleAllCheckboxes() {
        $('tbody').find('tr input[type="checkbox"]').each(function(){

                if($(this).attr('checked') === 'checked'){
                    $(this).attr('checked', false);
                }else{
                    $(this).attr('checked', true);
                }
        });
    }

    function checkboxes(){
        if($(this).attr('checked') === 'checked'){
            $(this).attr('checked', false);
        }else{
            $(this).attr('checked', true);
        }

    }

    $("document").ready(function(){
        $("tr input[type='checkbox']").on('click', checkboxes);
        $('#toggler').on('click', toggleAllCheckboxes);
    });
    // END:main
})();
```

### 表单验证：基本技巧

**必填输入项**

使用required这个CSS class属性标记这种输入区域，之后设置样式来给未填的必填输入项提供视觉反馈。

首先要截取到表单的submit事件，一旦挂上submit事件，就只需要获取表单中所有标记为required的元素，然后检测他们是否为非空值。

```js
<h1>必填输入项</h1>
<!-- START:main -->
<form id="registration">
    <p>
        <label for="user_first_name">First name*</label>
        <input type="text" name="user[first_name]" id="user_first_name"
               class="required text" />
    </p>
    <p>
        <label for="user_last_name">Last name*</label>
        <input type="text" name="user[last_name]" id="user_last_name"
               class="required text" />
    </p>
    <p>
        <label for="user_nickname">Nickname</label>
        <input type="text" name="user[nickname]" id="user_nickname" />
    </p>
    <p class="radios">
        <input type="checkbox" id="terms" name="terms" value="yes"
               class="required" />
        <label for="terms">I accept the terms of service*</label>
    </p>
    <p><input type="submit" value="Sign me up!" /></p>
</form>
<!-- END:main -->
```

```css
#registration { font-family: sans-serif; }
#registration p { margin: 0 0 0.5em; }
#registration label { display: inline-block; width: 6em; }
#registration .radios { padding-left: 6.2em; }
#registration .radios label { width: auto; }
#registration p.missing { color: maroon; }
#registration p.missing label { font-weight: bold; }
#registration p.missing input.text,
#registration p.missing textarea { border: 1px solid maroon; background: #fdd; }
```

```js
(function() {
    // START:main
    function checkForm(evt) {
        var value;
            $('.required').each(function() {
            value = $(this)[0].value;
            console.log("@@@@@@" + value);
            if (value) {
                $(this).parent('p').removeClass('missing');
            } else {
                $(this).parent('p').addClass('missing');
                $(this).focus();
                evt.preventDefault();
            }
        });
    }

    $('document').ready(function() {
        $('#registration').bind('submit', checkForm);
    });
    // END:main
})();
```

### 表单验证：进阶技巧

**检查特定格式的输入域**

* 正则表达式的创建

```js
var checkNum = /^[A-Za-z0-9]+$/;
var re=new RegExp(“["+s1+"]“,”g”);
```

* 常用规则

```js
/*
 * a) 用户密码：/^[a-zA-Z][a-zA-Z0-9_]{5,20}$/
 * b) 邮件:/^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/
 * c) 手机:/^[\d]{5,20}$/
 * d) 其它常用验证:请百度
 */
```

* 方式：test

```html
<form action="">
  <label>用户名：</label><span id="check_username">检测</span>
  <input type="text" id="t_username" placeholder="请输入"/>
  <hr/>
  <label>邮箱：</label><span id="check_email">检测</span>
  <input type="text" id="t_email" placeholder="请输入"/>
  <hr/>
  <label>手机：</label><span id="check_phone">检测</span>
  <input type="text" id="t_phone" placeholder="请输入"/>
  <hr/>
</form>
```

```js
$(function () {
  var FIELD_PATTERNS = {
      username: /^[a-zA-Z][a-zA-Z0-9_]{5,20}$/,
      email: /^[a-zA-Z][a-zA-Z0-9_]{5,20}$/,
      phone: /^[\d]{5,20}$/
  };  
  // 用户名
  $("#check_username").click(function(){
    var str = $("#t_username").val();
    if(FIELD_PATTERNS.username.test(str)){
      // todo 
    }else{
      // todo 
    }
  });
  // 邮件
  $("#check_email").click(function(){
    var str = $("#t_email").val();
    if(FIELD_PATTERNS.email.test(str)){
      // todo 
    }else{
      // todo 
    }
  });
  // 手机
  $("#check_phone").click(function(){
    var str = $("#t_phone").val();
    if(FIELD_PATTERNS.phone.test(str)){
      // todo 
    }else{
      // todo 
    }
  });
});
```

### 表单验证：高级技巧

Ajax表单验证的典型示例是全局唯一的输入域。如实现登名域的检查-假设场景要求用户之间的登录名不能重复。Ajax验证的特点是及时动态UI反馈，比如提示正在检查和提示检查结果，因此HTML标记得要预先留出这些元素的位置。至于触发方式，应对不同的输入域进行不同的选择。是在输入同时检查（用高频率的输入域轮询）还是在输入完成后检查（状态改变事件）。

### 在表单中提供动态的帮助tooltip

### 自动完成输入

### 使用动态多文件上传



