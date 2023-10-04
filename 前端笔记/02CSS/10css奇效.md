---
title: 10css奇效
description: 
published: 1
date: 2023-07-26T13:02:33.441Z
tags: 
editor: markdown
dateCreated: 2023-07-26T13:02:31.895Z
---

<center>css奇效</center>



[toc]





### css奇效

> css 特别好玩的属性



### 1. contenteditable属性和style

> contenteditable 让html可以编辑添加

```js
<style onkeydown="handleTabKey(event)" contenteditable style="display: block; -webkit-user-modify: read-write-plaintext-only;"">
        html {
                background-color: #fff;
        }
    </style>
    <script>
        // 阻止Tab 键
        function handleTabKey(event) {
            if (event.key === 'Tab') { // 检测按下的键是否为 Tab 键
                event.preventDefault(); // 阻止默认的 Tab 键行为
                insertTabCharacter(); // 插入制表符或其他所需内容
            }
        }

        function insertTabCharacter() {
            var selection = window.getSelection();
            var range = selection.getRangeAt(0);

            var tabNode = document.createTextNode('\t'); // 插入制表符（可以根据需求修改）
            range.insertNode(tabNode);

            range.setStartAfter(tabNode);
            range.setEndAfter(tabNode);
            selection.removeAllRanges();
            selection.addRange(range);
        }
    </script>
```

> 我去，牛。直接编辑样式就==热更新了==






### 2. css3伪类

> valid 和  invalid 表单验证是否通过

```html
<style>
    input:valid {
        border: 2px solid green;
    }
    input:invalid {
        border: 2px solid red;
    }
</style>

<input type="text" name="username" required>
<input type="text" name="phone" pattern="\d{10}" placeholder="请输入10位数字">
<input type="email" name="email" required>
```





### 3. 新增的html5

> `表单的   datalist `标签： 用户会在他们输入数据时看到域定义选项的下拉列表

```html
<label for="get_input">选择一个水果:</label>
<input type="text" name="foot" id="get_input" list="foot_seclets">
<datalist id="foot_seclets">
    <option value="香蕉">
    <option value="苹果">
    <option value="菠萝">
    <option value="梨子">
</datalist>
```

> ==progress== 下载的标签

```html
<progress value="70" max="100">70 %</progress>
```

> 
