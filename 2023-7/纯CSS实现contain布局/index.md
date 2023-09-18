### 纯CSS实现contain布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .container {
        width: 200px;
        height: 200px;
        overflow: auto;
        resize: both;
        text-align: center;
        background: #691;
        font-size: 0;
    }
    .square{
        display: inline-block;
        vertical-align: bottom;
        height: 100%;
        max-width: 100%;
        position: relative;
        overflow: hidden;
    }
    .square img {
        vertical-align: bottom;
    }
    .square-inner {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        height: 0;
        margin: auto;
        padding-bottom: 100%;
        background: lightcoral;
    }
    </style>
</head>
<body>
    <div class="container">
        <div class="square">
            <img src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==" height="100%">
            <div class="square-inner"></div>
        </div>
    </div>
</body>
</html>
```
