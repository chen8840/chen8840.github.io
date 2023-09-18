### impl和dyn用法

先推荐一个链接

### [理解 Rust 2018 edition 的两个新关键字 —— impl 和 dyn](https://www.codercto.com/a/19075.html)

官方文档中，impl是限定泛型的语法糖，所以

```rust
trait Trait {}

fn foo<T: Trait>(arg: T) {
}

fn foo(arg: impl Trait) {
}
```

这2种情况是相同的

但是当impl做为返回值的时候就有不同了

```rust
trait Fly {
  fn fly(&self) -> bool;
}
struct Duck;
impl Fly for Duck {
  fn fly(&self) -> bool {
    return true;
  }
}

fn foo<T>() -> T where T: Fly
{
    Duck
}fn main() {}
```

这样编译会报错

```
expected type parameter `T`, found struct `std::boxed::Box`
```

将返回T改为Box<T>也仍然会报错

```rust
trait Fly {
  fn fly(&self) -> bool;
}
struct Duck;
impl Fly for Duck {
  fn fly(&self) -> bool {
    return true;
  }
}

fn foo<T>() -> Box<T> where T: Fly
{
    Box::new(Duck)
}

fn main() {
}
```

这种情况用impl就不会报错

```rust
trait Fly {
  fn fly(&self) -> bool;
}
struct Duck;
impl Fly for Duck {
  fn fly(&self) -> bool {
    return true;
  }
}

fn foo() -> impl Fly
{
    Duck
}

fn main() {
}
```

## 关于dyn
dyn用于&dyn Fly和Box(dyn Fly)这样的引用和智能指针的限定，表明Fly是一个trait而不是struct，不加dyn也不会报错。
