### 如何在Rust中打印变量的类型？

```rust
#![feature(core_intrinsics)]
fn print_type_of<T>(_: T) {
    println!("{}", unsafe { std::intrinsics::type_name::<T>() });
}

fn main() {
    print_type_of(32.90);          // prints "f64"
    print_type_of(vec![1, 2, 4]);  // prints "std::vec::Vec<i32>"
    print_type_of("foo");          // prints "&str"
}
```

需要切换rustup到nightly版本才能运行cargo run

查看rustup版本

```
rustup toolchain list
```

切换到nightly版本

```
rustup default nightly
```
