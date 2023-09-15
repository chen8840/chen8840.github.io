### display: table-cell里面文字打点的方法

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

    .fixed-table {
      font-size: 40px;
      display: table;
      table-layout: fixed;
      width: 100%;

    }

    .fixed-table div {
      overflow: hidden;
      text-overflow: ellipsis;
    }
    </style>
</head>

<body>
    <div class="clearfix">
        <div class="float-left cr"></div>
        <div class="auto-right cg">
          <div class="fixed-table">
            <div>
                ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
            </div>
          </div>
        </div>
    </div>
</body>

</html>
```
