### angular中ngOnChanges与组件变化检测的关系

1.ngOnChanges只有在输入值改变的时候才会触发，如果输入值(@Input)是一个对象，改变对象内的属性的话是不会触发ngOnChanges的。

2.组件的变化检测：

　　2a.changeDetection如果是ChangeDetectionStrategy.Default的话，无论输入值(@Input)是否发生变化，都会进行组件自身的变化检测。

　　2b.changeDetection如果是ChangeDetectionStrategy.OnPush的话，只有在输入值(@Input)发生变化的情况下，才会进行自身的变化检测。<span style="color: red">（组件自身的事件也会触发变化检测，见 [angular变化检测OnPush策略需要注意的几个问题](/2021-3/angular变化检测OnPush策略需要注意的几个问题/index.md)）</span>

　　因此OnPush的组件在其内部改变属性值是不会反应在页面上的

```typescript
@Component({
  selector: 'app-movie',
  template: `
    <div>
      <h3>{{ title }}</h3>
      <p>
        <label>Actor:</label>
        <span>{{actor.firstName}} {{actor.lastName}}</span>
      </p>
      <div>{{test}}</div>
    </div>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MovieComponent implements OnInit {
  @Input() title: string;
  @Input() actor: Actor;
  test = 'test';

  constructor(private cd: ChangeDetectorRef) {}

  ngOnInit(): void {
    setTimeout(() => {
      this.test = 'test1';
      this.cd.detectChanges();
    }, 1000);
  }
}
```
这种情况要使用ChangeDetectorRef的detectChanges方法手动启用组件的变化检测。

　　2c.ChangeDetectorRef.detach之后的组件是不会在变化检测周期里自动进行变化检测的，需要手动进行变化检测。

3.组件的DoCheck接口无论什么情况都会在变化检测周期被调用。<span style="color:red">（不一定，最顶层的OnPush或detach组件会进行NgDoCheck，但是他们的viewChild组件不会进行NgDoCheck，contentChild组件反而会进行NgDoCheck，见 [angular变化检测OnPush策略需要注意的几个问题](/2021-3/angular变化检测OnPush策略需要注意的几个问题/index.md)）</span>

以上可知，ngOnChanges与组件是否进行变化检测没有直接关系。
