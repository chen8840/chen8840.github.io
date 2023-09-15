### 清除浮动的最佳方法
```CSS
.clearfix:after {
  content:'';
  display: table;
  clear: both;
}
.clearfix {
  *zoom: 1;
}
```
