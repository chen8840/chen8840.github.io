### 闭包类型（Fn,FnMut,FnOnce）和move关键字

<span style="color: blue">move关键字是强制让环境变量的所有权转移到闭包中而不管是不是发生了所有权的转移</span>

move关键字和匿名函数是否是FnOnce没有必然联系，之和匿名函数体有关

当匿名函数体里转移了环境变量的所有权的时候，匿名函数就是FnOnce。

当匿名函数体里改变了环境变量的值的时候，匿名函数就是FnMut。

否则匿名函数就是Fn。

关于move修饰的匿名函数需要注意的2点

1.如果函数不是FnOnce，此匿名函数可以重复调用

```rust
let mut x = vec![1];
let mut incr_x = move || {
    println!("{:?}", x);
    x.push(1);
};
incr_x();
incr_x();
incr_x();
```

2.如果捕获变量是复制语义类型，则闭包会实现Copy/Clone。如果捕获变量是移动语义类型，则闭包不会实现Copy/Clone。普通闭包都是移动语义

```rust
fn call<F: FnOnce()>(f: F) { f() }

fn main() {
    //未使用 move
    let mut x = 0;
    let incr_x = || x += 1;
    call(incr_x);
    // call(incr_x); // ERROR: 'incr_x' moved in the call above.
    //使用 move
    let mut x = 0;
    let incr_x = move || x += 1;
    call(incr_x);
    call(incr_x);
    //对移动语义类型使用move
    let mut x = vec![];
    let expend_x = move || x.push(42);
    call(expend_x);
    // call(expend_x); //ERROR: use of moved value: 'expend_x'
}
```

