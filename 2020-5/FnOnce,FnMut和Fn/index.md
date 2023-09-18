### FnOnce,FnMut和Fn

继承结构

FnOnce

FnMut: FnOnce

Fn: FnMut

<br>

<span style="color: red">FnOnce</span>就是说会转移闭包捕获变量的所有权，在闭包前加上move关键字可以限定此闭包为FnOnce

<span style="color: blue">move关键字是强制让环境变量的所有权转移到闭包中而不管是不是发生了所有权的转移</span>

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
  println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let i = vec![1];
    let x: Box<dyn FnOnce() -> ()> = Box::new(move || {
        // print_type_of(i);
        println!("{:?}", i);
    });
    x();
    println!("{:?}", i);
}
```

上面代码会报错

当然如果i是一个实现呢Copy的变量就不会报错了，因为Copy变量会自动复制而不会转移所有权

不用move关键字，只要闭包类转移了环境变量的所有权，闭包就只会实现FnOnce

```rust
#![feature(fn_traits)]
fn main() {
    let mut i = vec![1];
    let mut fn1 = || {
        i
    };
    fn1.call_mut(());　　fn1.call(());　　fn1.call_once(());
}
```

上面代码报错，因为fn1只实现了call_once方法

<span style="color: red">FnMut</span>就是改变了捕获变量

```rust
fn main() {
    let mut i = vec![1];

    let mut x: Box<dyn FnMut() -> ()> = Box::new(|| {
        i.push(2);
    });

    x();
}
```

这个代码里dyn FnMut() -> ()可以改为dyn FnOnce() -> (),但是不能改为dyn Fn() -> ()。因为实例是一个FnMut，只能往上转型，不能往下转型

FnMut捕获是按可变借用捕获的，所以<span style="color: blue">FnMut捕获的变量在闭包外也是不能使用的，即使被捕获的变量实现了Copy</span>

```rust
let mut i = 1;

let mut x: Box<dyn FnMut() -> ()> = Box::new(|| {
    i = i + 1;
});

x();
println!("{}", 1 + i);
```

上面代码的错误简化下就是

```rust
fn test(ii: &mut i32, i: i32) {}

fn main() {
    let mut i = 1;
    let ii = &mut i;
    test(ii, i);
}
```

如果需要变量在FnMut后还能使用，需要用Mutex模拟。用Mutex模拟后，闭包也不再是FnMut了，而是Fn

```rust
use std::sync::Mutex;

fn main() {
    let mut i = Mutex::new(1);

    let mut x: Box<dyn Fn() -> ()> = Box::new(|| {
        let mut ii = i.lock().unwrap();
        *ii = 2;
    });

    x();
    let iii = i.lock().unwrap();
    println!("{}", iii);
}
```

一个啥也不干的闭包就是<span style="color: red">Fn</span>了,Fn以不可变借用捕获环境变量

```rust
fn main() {
    let x: Box<dyn Fn() -> ()> = Box::new(|| {
    });
}
```

Fn能调用FnMut和FnOnce的方法

```rust
#![feature(fn_traits)]
fn main() {
    let mut fn1 = || {};
    fn1.call_mut(());
    fn1.call_once(());
}
```
