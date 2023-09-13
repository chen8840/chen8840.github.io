让DIV垂直居中的几种办法

1.使用CSS3 的伸缩盒布局
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #container {
        height: 400px;
        width: 100%;
        background-color: gray;
        display:-webkit-flex;
        display: flex;
        flex-direction: row;
        -webkit-flex-direction: row;
        -webkit-justify-content: center;
        justify-content: center;
        -webkit-align-items: center;
        align-items: center;
    }
    #container > div{
        height: 200px;
        width: 200px;
        background-color: red;

    }
</style>
</head>

<body>
<div id="container">
    <div></div>
</div>
</body>
</html>
```
2.position:absolute 和 margin 联合使用
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #container {
        height: 400px;
        width: 100%;
        background-color: gray;

        position: relative;
    }
    #container > div{
        height: 200px;
        width: 200px;
        background-color: red;

        position: absolute;
        top: 50%;
        left: 50%;
        margin: -100px 0 0 -100px;
    }
</style>
</head>

<body>
<div id="container">
    <div></div>
</div>
</body>
</html>
```
3.position:absolute 和 margin: auto联合使用
```HTML
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<style>
    #container {
        height: 400px;
        width: 100%;
        background-color: gray;

        position: relative;
    }
    #container > div{
        height: 200px;
        width: 200px;
        background-color: red;

        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
</style>
</head>

<body>
<div id="container">
    <div></div>
</div>
</body>
</html>
```
4.position:absolute和translate的联合使用
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
    html,body {
        height: 100%;
        margin: 0;
        padding: 0;
    }
    div {
        position: absolute;
        left: 50%;
        top: 50%;
        background-color: red;
        transform: translate(-50%,-50%);
        font-size: 440px;
    }
    </style>
</head>
<body>
    <div>W</div>
</body>
</html>
```
5.让div inline-block化
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
    html,body {
        height: 100%;
        margin: 0;
        padding: 0;
        text-align: center;
    }
    div {
        display: inline-block;
        background-color: red;
        font-size: 440px;
        vertical-align: middle;
    }
    i {
        display: inline-block;
        height: 100%;
        vertical-align: middle;
    }
    </style>
</head>
<body>
    <div>W</div><i></i>
</body>
</html>
```
