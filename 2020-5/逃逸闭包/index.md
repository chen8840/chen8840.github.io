### 逃逸闭包

跑出父作用域的闭包叫做逃逸闭包

逃逸闭包如果引用了环境变量，那么需要使用move关键字，或者是FnOnce也行

```rust
fn test() -> impl Fn()  {
    let s = String::from("");
    move || {
        println!("{}", s);
    }
}

fn main() {
    let f = test();
    f();
}
```

```rust
fn test() -> impl FnOnce()  {
    let s = String::from("");
    || {
        let c = s;
    }
}

fn main() {
    let f = test();
    f();
}
```

即使引用的环境变量实现了Copy/Clone也不行，因为Fn和FnMut是使用的引用
