### 使用OnPush和immutable.js来提升angular的性能

angular里面变化检测是非常频繁的发生的，如果你像下面这样写代码

```html
<div>
  {{hello()}}
</div>
```

则每次变化检测都会执行hello函数，如果hello函数十分耗时，则会出现占用CPU高的问题。

这时，推荐使用OnPush策略和immutable.js来提升angular应用的性能。

OnPush策略可以阻止angular变化检测传入组件，这样每次变化检测不会进到你的组件里面来调用hello函数。

引入immutable.js的作用是为了更加方便的使用OnPush策略。

看下immutable.js的例子

```javascript
const list1 = Immutable.List([ {a: 1}, {a: 2} ]);
const list2 = list1.push({a: '3'});

console.log(list1 === list2); // false
console.log(list1.get(0) === list2.get(0)); // true

const list3 = list2.pop();

console.log(list1 === list3); // false

const map1 = Immutable.Map({ a: 1, b: 2, c: 3 });
const map2 = map1.set('b', 50);

console.log(map1 === map2); // false

const map3 = map1.set('b', 2);

console.log(map1 === map3); // true

const map4 = map2.set('b', 2);

console.log(map1 === map4); // false
```

这样在父组件里只需要如此写

```typescript
export class AppComponent {
  ii = '';

  list = List([{label: 'default'}]);

  add() {
    if (this.ii) {
      this.list = this.list.push({label: this.ii});
      this.ii = '';
    }
  }
}
```

子组件HTML代码

```html
<div>
  {{hello()}}
</div>

<ul *ngFor="let item of list">
  <li>{{item.label}}</li>
</ul>
```

不需要trackby，因为默认trackby就是比较元素相等，immutable.js数组里不变的元素就是相等的，少写一些代码啦。
