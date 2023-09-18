### 使用生命周期来限制类型

生命周期一般用来修饰引用类型，但是他也可以用来修饰泛型和类型的

比如以下代码

```rust
trait Foo {}

struct FooImpl<'a> {
    s: &'a u32
}
impl<'a> Foo for FooImpl<'a> {}

fn foo<'a>(s: &'a u32) -> Box<dyn Foo + 'a> {
    Box::new(FooImpl {s: s})
}
```

如果foo函数的返回值类型中不加上'a的修饰，是会报错的，因为trait Foo默认的生命周期是'static

明显比参数s的生命周期长，这是rust不允许的
