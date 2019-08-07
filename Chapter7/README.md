# 第七章:前端

## 一、JQuery

### 1.引用

```html
<head>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js">
</script>
</head>
```

### 2.选择器

| 语法                     | 描述                                                    |
| :----------------------- | ------------------------------------------------------- |
| $("p.intro")             | 选取 class 为 intro 的 <p> 元素                         |
| $(this)                  | 选取当前 HTML 元素                                      |
| $("p:first")             | 选取第一个 <p> 元素                                     |
| $("ul li:first")         | 选取第一个 <ul> 元素的第一个 <li> 元素                  |
| $("ul li:first-child")   | 选取每个 <ul> 元素的第一个 <li> 元素                    |
| $("[href]")              | 选取带有 href 属性的元素                                |
| $("a[target='_blank']")  | 选取所有 target 属性值等于 "_blank" 的 <a> 元素         |
| $("a[target!='_blank']") | 选取所有 target 属性值不等于 "_blank" 的 <a> 元素       |
| $(":button")             | 选取所有 type="button" 的 <input> 元素 和 <button> 元素 |
| $("tr:even")             | 选取偶数位置的 <tr> 元素                                |

### 3.事件

| **鼠标事件** | **键盘事件** | **表单事件** | 文档/窗口事件 |
| ------------ | ------------ | ------------ | ------------- |
| click        | keypress     | submit       | load          |
| dblclick     | keydown      | change       | resize        |
| mouseenter   | keyup        | focus        | scroll        |
| mouseleave   |              | blur         | unload        |
| hover        |              |              |               |

* $(document).ready()

  > 文档完全加载完后执行函数

* click()

  ```js
  $("p").click(function(){
    $(this).hide();
  });
  ```

* dblclick()

  ```js
  $("p").dblclick(function(){
    $(this).hide();
  });
  ```

* mouseenter()

  ```js
  $("#p1").mouseenter(function(){
      alert('您的鼠标移到了 id="p1" 的元素上!');
  });
  ```

* mouseleave()

  ```js
  $("#p1").mouseleave(function(){
      alert("再见，您的鼠标离开了该段落。");
  });
  ```

* mousedown()

  ```js
  $("#p1").mousedown(function(){
      alert("鼠标在该段落上按下！");
  });
  ```

* mouseup()

  ```js
  $("#p1").mouseup(function(){
      alert("鼠标在段落上松开。");
  });
  ```

* hover()

  ```js
  //当鼠标移动到元素上时，会触发第一个函数(mouseenter);当鼠标移出这个元素时，会触发第二个函数
  $("#p1").hover(
      function(){
          alert("你进入了 p1!");
      },
      function(){
          alert("拜拜! 现在你离开了 p1!");
      }
  );
  ```

* focus()

  ```js
  $("input").focus(function(){
    $(this).css("background-color","#cccccc");
  });
  ```

* blur()

  ```js
  $("input").blur(function(){
    $(this).css("background-color","#ffffff");
  });
  ```

  
### 4.效果

#### 4.1.隐藏和显示

* hide()|show()

  ```js
  $("#hide").click(function(){
    $("p").hide();//第一个参数控制速度：fast,slow或者毫秒；第二个参数控制缩进方式；第三个是隐藏后执行函数
  });
   
  $("#show").click(function(){
    $("p").show();
  });
  ```

* toggle()

  ```js
  //切换 hide() 和 show() 方法。
  $("button").click(function(){
    $("p").toggle();
  });
  ```

#### 4.2.淡入淡出

* fade方法

  ```js
  //1.fadeIn()淡入方法
  $("button").click(function(){
    $("#div1").fadeIn();
    $("#div2").fadeIn("slow");
    $("#div3").fadeIn(3000);
  });
  
  
  //2.fadeOut()淡出方法
  $("button").click(function(){
    $("#div1").fadeOut();
    $("#div2").fadeOut("slow");
    $("#div3").fadeOut(3000);
  });
  
  //3.fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换。
  
  $("button").click(function(){
    $("#div1").fadeToggle();
    $("#div2").fadeToggle("slow");
    $("#div3").fadeToggle(3000);
  });
  
  
  //4.fadeTo() 方法允许渐变为给定的不透明度（值介于 0 与 1 之间）。
  $("button").click(function(){
    $("#div1").fadeTo("slow",0.15);
    $("#div2").fadeTo("slow",0.4);
    $("#div3").fadeTo("slow",0.7);
  });
  ```

  
#### 4.3.滑动

* slideDown()

  ```js
  //向下滑动元素。
  $("#flip").click(function(){
    $("#panel").slideDown();
  });
  ```

* slideUp()

  ```js
  //向上滑动
  $("#flip").click(function(){
    $("#panel").slideUp();
  });
  ```

* slideToggle()

  ```js
  //切换
  $("#flip").click(function(){
    $("#panel").slideToggle();
  });
  ```

  
#### 4.4.停止动画
* stop()

  ```js
  $("#stop").click(function(){
    $("#panel").stop();
  });
  ```

  
#### 4.5.链
* 方法链接

  ```js
  $("#p1").css("color","red").slideUp(2000).slideDown(2000);
  
  ```

  
### 5.jQuery HTML
#### 5.1.捕获

* 获得内容

  ```js
  //元素的文本内容
  $("#btn1").click(function(){
    alert("Text: " + $("#test").text());
  });
  
  //元素的内容（包括HTML标记）
  $("#btn2").click(function(){
    alert("HTML: " + $("#test").html());
  });
  
  //表单字段的值
  $("#btn1").click(function(){
    alert("值为: " + $("#test").val());
  });
  ```

* 获取属性

  ```js
  $("button").click(function(){
    alert($("#runoob").attr("href"));
  });
  ```

#### 5.2.设置
* 设置内容

  ```js
  $("#btn1").click(function(){
      $("#test1").text("Hello world!");
  });
  $("#btn2").click(function(){
      $("#test2").html("<b>Hello world!</b>");
  });
  $("#btn3").click(function(){
      $("#test3").val("RUNOOB");
  });
  ```
* 设置属性
  ```js
  $("button").click(function(){
  $("#runoob").attr("href","http://www.runoob.com/jquery");
  });
  ```

#### 5.3.添加元素

* 元素结尾或者开头

  ```js
  $("p").append("追加文本");
  $("p").prepend("在开头追加文本");
  ```

* 被选元素前或者后

  ```js
  $("img").after("在后面添加文本");
   
  $("img").before("在前面添加文本");
  ```

#### 5.4.删除元素

* remove()

  ```js
  //删除被选元素及其子元素。
  $("#div1").remove();
  
  //删除 class="italic" 的所有 <p> 元素：
  $("p").remove(".italic");
  ```

* empty()

  ```js
  //被选元素的子元素。
  $("#div1").empty();
  ```

  