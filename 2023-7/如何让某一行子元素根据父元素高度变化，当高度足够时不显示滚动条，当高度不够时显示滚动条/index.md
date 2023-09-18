### 如何让某一行子元素根据父元素高度变化，当高度足够时不显示滚动条，当高度不够时显示滚动条

只需要父元素设置flex布局，子元素设置overflow: auto;即可

上代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
            .auto-size {
                width: 400px;
                height: 300px;
                overflow: auto;
                resize: both;
            }
            .container {
                height: 100%;
                background-color: lightgray;
                overflow: hidden;
                border: 1px dashed;
                box-sizing: border-box;
                display: flex;
                flex-direction: column;
            }
            .row-1 {
                background-color: lightblue;
                line-height: 40px;
            }
            .row-2 {
                background-color: lightpink;
                overflow: auto;
            }
            .row-3 {
                background-color: lightblue;
                line-height: 40px;
            }
    </style>
</head>
<body>
    <div class="auto-size">
        <div class="container">
            <div class="row-1">
                row-1row-1row-1
            </div>
            <div class="row-2">
                <div class="row-inner">
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                    <div>row-2</div>
                </div>
            </div>
            <div class="row-3">
                row-3row-3row-3
            </div>
        </div>
    </div>
</body>
</html>
```
