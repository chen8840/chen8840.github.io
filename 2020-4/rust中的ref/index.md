### rust中的ref

## [理解Rust的引用与借用](https://www.jianshu.com/p/ac519d8c5ec9)（好文链接）

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let ref a: i32;
    a = &1;
    let ref a: i32 = 1;
    print_type_of(a);

    let c: &u32 = &&&&&2;
    print_type_of(c);
}
```

上面2个a的类型都是&i32

enum带参数时使用match会move走enum的参数，如下这样写会报错

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let os = Some(String::from("s"));
    match os {
      Some(s) => {
        print_type_of(s);
      },
      _ => ()
    };
    println!("{:?}", os);
}
```

改下match的参数匹配模式，用ref来匹配就不会出错了

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let os = Some(String::from("s"));
    match os {
      Some(ref s) => {
        print_type_of(s); // s此时是&alloc::string::String类型
      },
      _ => ()
    };
    println!("{:?}", os);
}
```

如果match的对象是一个引用，会发现参数是引用类型，比如

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let os = &Some(String::from("s"));
    match os {
      Some(s) => {
        print_type_of(s);
      },
      _ => ()
    };
}
```

打印的是&alloc::string::String

其实和下面的代码一样

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    let os = &Some(String::from("s"));
    match os {
      &Some(ref s) => {
        print_type_of(s);
      },
      _ => ()
    };
}
```
