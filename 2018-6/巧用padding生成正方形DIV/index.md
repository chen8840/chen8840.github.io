### 巧用padding生成正方形DIV
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .square{
        padding: 50%;

        position: relative;
    }
    .c {
        position: absolute;
        background: yellowgreen;
        /* transform: translate3d(-50%, -50%, 0); */
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
    }
    </style>
</head>
<body>
    <div class="square">
        <div class="c"></div>
    </div>
</body>
</html>
```
