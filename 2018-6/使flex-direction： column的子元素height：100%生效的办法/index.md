### 使flex-direction: column的子元素height: 100%生效的办法

在flex-direction: column子元素里直接使用height：100%，height并不会被设置成100%

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .height-500-px {
        height: 500px;
    }
    .height-100-per {
        height: 100%;
    }
    .bg-gray {
        background: gray;
    }
    .bg-yellow {
        background: yellow;
    }
    .flex-column {
        display: flex;
        flex-direction: column;
    }
    .flex-grow-1 {
        flex-grow: 1;
    }
    </style>
</head>
<body>
    <div class="flex-column height-500-px bg-gray">
        <div class="flex-grow-1">
            <div class="height-100-per bg-yellow"></div>
        </div>
    </div>
</body>
</html>
```
![](582229-20180608184322404-686556092.png)
解决办法是在flex-grow-1这一层再加一个flex row
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .height-500-px {
        height: 500px;
    }
    .height-100-per {
        height: 100%;
    }
    .bg-gray {
        background: gray;
    }
    .bg-yellow {
        background: yellow;
    }
    .bg-blue {
        background: blue;
    }
    .flex-column {
        display: flex;
        flex-direction: column;
    }
    .flex-grow-1 {
        flex-grow: 1;
    }
    .flex-column-row {
        display: flex;
        flex-direction: row;
    }
    </style>
</head>
<body>
    <div class="flex-column height-500-px bg-gray">
        <div class="flex-grow-1 flex-column-row">
            <!-- 注意不要加height:100% -->
            <div class="bg-yellow flex-grow-1 height-100-per"></div>
        </div>
        <div class="flex-grow-1 flex-column-row">
            <div class="bg-blue flex-grow-1"></div>
        </div>
        <div class="flex-grow-1 flex-column-row" style="overflow:hidden"> <!-- 为了防止内部的内容撑大，加上overflow:hidden -->
            <div class="flex-grow-1">
                <!-- 里面一层加height:100%是可以的 -->
                <div class="bg-yellow height-100-per"></div>
            </div>
        </div>
    </div>
</body>
</html>
```
![](582229-20180608190257290-12839191.png)
从以上情况可以大致推测

<span style="color:red">flex-column子元素的height:100%会优先于flow布局来计算高度，所以直接在flex-column子元素设height:100%没有效果，因为在计算height:100%的时候，高度为0</span>

<span style="color:red">注意：2023年9月15日在chrome和firefox上验证已没有这个问题，不需要加中间层就可以让height:100%生效</span>
