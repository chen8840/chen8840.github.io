### 使用Pure Pipes来替换HTML里面的纯函数

```html
<ul *ngFor="let item of list">
  <li>{{show(item.label)}}</li>
</ul>
```

上面的代码每次变化检测的周期都会执行，即使item.label没有改变

此时应该使用Pure Pipes来代替show函数

```typescript
@Pipe({
  name: 'showlabel',
  pure: true
})
export class ShowlabelPipe {
  transform(label: string) {
    return show(label);
  }
}
```

HTML改为

```html
<ul *ngFor="let item of list">
  <li>{{item.label | showlabel}}</li>
</ul>
```

此时变化检测周期时如果item.label没有改变，是不会调用show函数的
