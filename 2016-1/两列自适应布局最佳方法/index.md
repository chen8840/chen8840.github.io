### 两列自适应布局最佳方法
```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <style>
    .clearfix:after {
        content: '';
        display: table;
        clear: both;
    }

    .clearfix {
        *zoom: 1;
    }

    .cr {
        background-color: red;
    }

    .cg {
        background-color: green;
    }

    .float-left {
        float: left;
        margin-right: 10px;
        width: 100px;
        height: 300px;
    }

    .auto-right {
        display: table-cell;
        width: 10000px;
        height: 200px;
    }
    </style>
</head>

<body>
    <div class="clearfix">
        <div class="float-left cr"></div>
        <div class="auto-right cg"></div>
    </div>
</body>

</html>
```
