### 如何让父元素的最小宽度为某一个子元素的内容宽度

具体做法是让除了那个子元素以外，所有子元素都使用flex布局，让后再叠加一层flex-grow:1;width:0的inner布局

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
                overflow: auto;
                resize: both;
            }
            .container {
                min-width: fit-content;
                overflow: hidden;
                border: 1px dashed;
                display: flex;
                flex-direction: column;
            }
            .row-1, .row-2, .row-3 {
                white-space: nowrap;
                display: flex;
            }
            .row-inner {
                flex-grow: 1;
                width: 0;
                overflow: hidden;
                text-overflow: ellipsis;
            }
            .row-1 {
                background-color: lightblue;
            }
            .row-2 {
                background-color: lightpink;
            }
            .row-3 {
                background-color: lightslategray;
            }
    </style>
</head>
<body>
    <div class="auto-size">
        <div class="container">
            <div class="row-1">
                <div class="row-inner">
                    row-1row-1row-1
                </div>
            </div>
            <div class="row-2">
                row-2row-2row-2row-2row-2
            </div>
            <div class="row-3">
                <div class="row-inner">
                    row-3row-3row-3row-3row-3row-3row-3
                </div>
            </div>
        </div>
    </div>

</body>
</html>
```
