### 防止HTML出现滚动条时页面的抖动
```HTML
<html>
<style>
    * {
        padding: 0;
        margin: 0;
    }
    body {
        font: 40px fantasy;
    }
    .container {
        width: 1200px;
        height: 300px;
        margin: 0 auto;
        background-color: red;
    }
</style>
<body>
    <div class="container">
    </div>
</body>
</html>
```
这样的布局当body出现滚动条时container会往左移动7px左右

为了消除这样的滚动，在.container上加入如下规则
```CSS
padding-left: calc(100vw - 100%);
```
