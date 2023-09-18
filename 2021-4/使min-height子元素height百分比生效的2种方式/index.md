### 使min-height子元素height百分比生效的2种方式

方式1，使用flex

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html, body {
            padding: 0;
            margin: 0;
            /* height: 2400px; */
            height: 100%;
            background: lightgray;
        }
        .body {
            display: flex;
            background: yellow;
            box-sizing: border-box;
            min-height: 600px;
            padding: 10px;
        }
        .child {
            flex-basis: 100px;
        }
        .childchild {
            height: 100%;
            background: red;
        }
    </style>
</head>
<body>

    <div class="body">
        <div class="child">
            <div class="childchild"></div>
        </div>
    </div>
</body>
</html>
```

这种方式若childchild高度超过child，会撑开child元素，若想childchild的高度不撑开child，用下面的方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html, body {
            padding: 0;
            margin: 0;
            /* height: 2400px; */
            height: 100%;
            background: lightgray;
        }
        .body {
            display: flex;
            background: yellow;
            box-sizing: border-box;
            min-height: 600px;
            padding: 10px;
            flex-direction: column;
        }
        .child {
            flex-basis: 100px;
            flex-grow: 1;
            height: 0;
        }
        .childchild {
            height: 800px;
            background: red;
        }
    </style>
</head>
<body>

    <div class="body">
        <div class="child">
            <div class="childchild"></div>
        </div>
    </div>
</body>
</html>
```

方式2，使用grid

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html, body {
            padding: 0;
            margin: 0;
            /* height: 2400px; */
            height: 100%;
            background: lightgray;
        }
        .body {
            display: grid;
            min-height: 600px;
            background: yellow;
            padding: 10px;
        }
        .child {
          height: 100%;
          background: red;
        }

    </style>
</head>
<body>
    <div class="body">
        <div class="child">
        </div>
    </div>
</body>
</html>
```

 这种方式child也会撑开body
