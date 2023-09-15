### inline-block容器的高度撑开位置
block的高度是从最上面撑开的

那么inline-block呢？

直接上代码
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #test {
        line-height: 0;
        font-size: 80px;
        background-color: gray;
    }
    #test span {
        display: inline-block;
        background-color: red;
        height: 1px;
        width: 150px;
    }
    #s1 {

    }
    #s2 {
        line-height: 1;
    }
</style>
</head>

<body>
<div style="height: 100px;"></div>
<div id="test">
    <span id="s0"></span>
    <span id="s1">33</span>
    <span id="s2">33</span>
</div>
</body>
</html>
```
![](582229-20151126135736109-1121949459.png)

看到没？3个inline-block的撑开位置是不一样的。

同时把他们的父元素撑开了。

把3个inline-block高度加高试试

```CSS
#test span {
    display: inline-block;
    background-color: red;
    height: 10px;
    width: 150px;
}
```
![](582229-20151126140832031-193274808.jpg)

撑开方向也不一样。

把span换成button试一试
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #test {
        line-height: 0;
        font-size: 80px;
        background-color: gray;
    }
    #test button {
        //display: inline-block;
        line-height: 0;
        font-size: 80px;
        background-color: red;
        height: 1px;
        width: 150px;
    }
    #s1 {

    }
    #s2 {
        line-height: 1 !important;
    }
</style>
</head>

<body>
<div style="height: 100px;"></div>
<div id="test">
    <button id="s0"></button>
    <button id="s1">33</button>
    <button id="s2">33</button>
</div>
</body>
</html>
```
![](582229-20151126141720218-572786149.jpg)

height是1的时候跟span差不多的表现

增加height之后：
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #test {
        line-height: 0;
        font-size: 80px;
        background-color: gray;
    }
    #test button {
        //display: inline-block;
        line-height: 0;
        font-size: 80px;
        background-color: red;
        height: 30px;
        width: 150px;
    }
    #s1 {

    }
    #s2 {
        line-height: 1 !important;
    }
</style>
</head>

<body>
<div style="height: 100px;"></div>
<div id="test">
    <button id="s0"></button>
    <button id="s1">33</button>
    <button id="s2">33</button>
</div>
</body>
</html>
```
![](582229-20151126141854202-735722376.jpg)

中间元素的撑开方向与span不一样了
