### appendChild与Transition动画
在createElement之后，直接把这个div append到body中，是不会触发css3 transition动画的

必须要让浏览器计算div的css属性后，然后再设置div的style，才会触发transition动画

代码如下
```JAVASCRIPT
var e = document.createElement('div');
e.className = 'box e';
document.getElementById('wrapper').appendChild(e);
window.getComputedStyle(e).opacity; // 或者e.offsetWidth
e.className += ' in';
```
