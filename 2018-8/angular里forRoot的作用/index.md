### angular里forRoot的作用

模块A是这样定义的

```Typescript
@NgModule({
    providers: [AService]，
    declarations: [ TitleComponent ],
    exports:      [ TitleComponent ],
})
export class A {}
```

如果有惰性模块lazyModule导入模块A，那么会生成子注入器将AService重新生成

这时如果想将AService变成全局唯一的，那么在lazyModule导入的时候就不要导入providers而只导入TitleComponent

forRoot这时就有用武之地了

改写A

```Typescript
@NgModule({
    providers: []，
    declarations: [ TitleComponent ],
    exports:      [ TitleComponent ],
})
export class A {
   static forRoot() {
        return {ngModule: A, providers: [AService]};
   }
}
```

在appModule中使用A.forRoot导入A模块
```Typescript
@NgModule({
  imports: [A.forRoot()]
})
export class AppModule {}
```
在lazyModule中正常导入A模块
```Typescript
@NgModule({
  imports: [A]
})
export class LazyModule{}
```
