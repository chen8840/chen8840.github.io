### rxjs的几点使用心得

## 1.对错误的处理

日常使用中，点击按钮需要往后台发消息，为了不重复发消息，经常需要把点击事件做成subject，然后把发消息的过程做成switchMap，类似下面的写法

```typescript
const subject = new rxjs.Subject();

subject.pipe(
  rxjs.operators.switchMap(index => {
return rxjs.of(index);
  })
).subscribe({
  next: console.log,
  error: err => console.error('error2', err),
  complete: () => console.log('complete')
});


subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);
subject.complete();
```

但是如果某一个发消息的observer报了一个500错误，那么会导致后面的点击事件不会继续调用发消息的过程。

```typescript
const subject = new rxjs.Subject();

subject.pipe(
  rxjs.operators.switchMap(index => {
    if (index === 2) {
      return rxjs.throwError(new Error('error'));
    }
    return rxjs.of(index);
  })
).subscribe({
  next: console.log,
  error: err => console.error('error2', err),
  complete: () => console.log('complete')
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);
subject.complete();
```

如上，当index等于2时，rxjs抛出错误，后面的3，4都不会执行了。

为了使后台的错误不影响rxjs，我们需要对switchMap里面的observer做catchError的特殊处理。如下：

```typescript
const subject = new rxjs.Subject();

subject.pipe(
  rxjs.operators.switchMap(index => {
    if (index === 2) {
      return rxjs.throwError(new Error('error')).pipe(
        rxjs.operators.catchError(err => {
          return rxjs.empty();
        })
      );
    }
    return rxjs.of(index);
  })
).subscribe({
  next: console.log,
  error: err => console.error('error2', err),
  complete: () => console.log('complete')
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);
subject.complete();
```

这样做会跳过抛出错误的后台请求，保证rxjs继续往下执行。

## 2.如何保证loading = false必执行

如果rxjs抛出错误，subscribe的error分支会执行，complete分支不会执行

如果rxjs不抛出错误结束，subscribe的error分支不会执行,complete分支会执行

有个操作符，不管抛不抛出错误，他都会执行。

```typescript
var loading = true;
rxjs.from([1,2,3]).pipe(
  rxjs.operators.map(i => {
    if (i === 2) {
      throw new Error('error');
    }
    return i;
  }),
  rxjs.operators.finalize(() => {
    console.log('set loading')
    loading = false;
  })
).subscribe({
  next: console.log,
  error: console.error,
  complete: () => console.log('complete')
});
```

## 3.如何依次发送后台请求

碰到需要同时发送后台请求时，一般使用forkjoin方法。

如果请求A依赖于请求B的结果，需要A返回后再发送请求B。这时可以用concat配合bufferCount操作符来实现

```typescript
var a = rxjs.Observable.create((observer) => {
  setTimeout(() => observer.next('a1'), 2000);

  // setTimeout(() => observer.error(new Error('error')), 3000);
  setTimeout(() => observer.complete(), 4000);
});

var b = rxjs.Observable.create((observer) => {
  setTimeout(() => observer.next('b1'), 2000);
  // setTimeout(() => observer.next('b2'), 3000);
  // setTimeout(() => observer.next('b3'), 3300);
  setTimeout(() => observer.complete(), 4000);
});

rxjs.concat(a, b).pipe(
  rxjs.operators.bufferCount(2),
).subscribe({
  next: console.log,
  error: err => console.error('error123', err),
  complete: () => console.log('complete')
});
```

