### 在less里面使用js函数

```less
.colorPaletteMixin() {
  @functions: ~`(function() {


    this.colorPalette = function() {
      return '123px';
    };
  })()`;
}
// It is hacky way to make this function will be compiled preferentially by less
// resolve error: `ReferenceError: colorPalette is not defined`
// https://github.com/ant-design/ant-motion/issues/44
.colorPaletteMixin();

.test {
  width: `colorPalette()`;
}
```
