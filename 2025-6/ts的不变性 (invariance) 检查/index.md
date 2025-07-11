为了完全理解**不变性 (Invariance)**，我们必须先了解它的两个“兄弟”概念：**协变 (Covariance)** 和 **逆变 (Contravariance)**。这三者统称为“型变 (Variance)”，它们描述了当类型之间存在继承关系时（例如 `Cat` 是 `Animal` 的子类），这些类型在更复杂的类型（如泛型 `Container<T>` 或函数）中如何相互赋值。

我们先用一个简单的父子类关系作为基础：

```typescript
// 基础类
class Animal {
  name: string = "animal";
}

class Cat extends Animal {
  meow() { console.log('Meow!'); }
}

// 明确的子类型关系: Cat 是 Animal 的一个子类型
// Cat ⇏ Animal
```

现在，我们来看这三种“型变”：

### 1. 协变 (Covariance)

**概念**：如果 `Cat` 是 `Animal` 的子类，那么 `Producer<Cat>` 也是 `Producer<Animal>` 的子类。类型的子类型关系**保持不变**。

这通常发生在**只读**或**输出**位置，比如函数的返回值。

**例子**：

```typescript
type Producer<T> = () => T; // 一个生产类型 T 的函数

let catProducer: Producer<Cat> = () => new Cat();
let animalProducer: Producer<Animal>;

// 把一个“猫的生产者”赋值给一个“动物的生产者”
animalProducer = catProducer; // ✅ 这是安全的！

// 为什么安全？
// animalProducer 的使用者期望得到一个 Animal。
// catProducer 实际返回一个 Cat。
// 因为 Cat 是 Animal，所以这个期望完全被满足。
```

在 TypeScript 中，函数的返回值类型是协变的。

### 2. 逆变 (Contravariance)

**概念**：如果 `Cat` 是 `Animal` 的子类，那么 `Consumer<Animal>` 反而是 `Consumer<Cat>` 的子类。类型的子类型关系**反转**了。

这通常发生在**只写**或**输入**位置，比如函数的参数。

**例子**：

```typescript
type Consumer<T> = (input: T) => void; // 一个消费类型 T 的函数

let animalConsumer: Consumer<Animal> = (animal: Animal) => console.log(animal.name);
let catConsumer: Consumer<Cat>;

// 把一个“动物的消费者”赋值给一个“猫的消费者”
catConsumer = animalConsumer; // ✅ 这是安全的！(注意方向)

// 为什么安全？
// catConsumer 的使用者会传入一个 Cat。
// animalConsumer (它现在的身份是 catConsumer) 接收到这个 Cat。
// animalConsumer 内部的代码设计用来处理任何 Animal，所以处理一个 Cat 完全没问题。

// 反过来则不安全：
// let dogConsumer: Consumer<Dog> = (dog: Dog) => dog.bark();
// animalConsumer = dogConsumer; // ❌ 错误！
// 如果允许，animalConsumer(new Cat()) 就会调用 dog.bark()，导致运行时错误。
```

在 TypeScript 中，当开启 `strictFunctionTypes` 编译选项时，函数参数是逆变的。

### 3. 不变性 (Invariance)

**概念**：如果 `Cat` 是 `Animal` 的子类，但 `Container<Cat>` 和 `Container<Animal>` **没有任何关系**。它们不能相互赋值。

这通常发生在类型参数同时被用作**输入**和**输出**（即可读又可写）的位置。

**不变性检查**就是 TypeScript 编译器阻止这种不安全赋值的机制。

**例子：**

```typescript
type Container<T> = {
  value: T; // 可读可写
};

let catContainer: Container<Cat> = { value: new Cat() };
let animalContainer: Container<Animal> = { value: new Animal() };

// 尝试协变赋值 (Cat -> Animal)
// animalContainer = catContainer; // ❌ 错误! (在严格模式下)

// **为什么不安全 (写)？**
// 1. 如果赋值成功，animalContainer 和 catContainer 指向同一个对象。
// 2. 我们可以合法地执行 animalContainer.value = new Dog()，因为 Dog 是 Animal。
// 3. 但这样一来，catContainer.value 现在也变成了 Dog。
// 4. 当我们试图访问 catContainer.value.meow() 时，程序就会崩溃，因为 Dog 没有 meow 方法。

// 尝试逆变赋值 (Animal -> Cat)
// catContainer = animalContainer; // ❌ 错误!

// **为什么不安全 (读)？**
// 1. 如果赋值成功，catContainer 和 animalContainer 指向同一个对象。
// 2. 我们可以合法地读取 catContainer.value。我们期望得到一个 Cat。
// 3. 但 animalContainer 的 value 可能只是一个普通的 Animal (不是 Cat)。
// 4. 当我们试图访问 catContainer.value.meow() 时，程序可能就会崩溃。
```

由于 `Container<T>` 中的 `value` 属性既可以被读取（协变位置），又可以被写入（逆变位置），为了保证类型安全，TypeScript 必须同时满足协变和逆变的要求。但这是矛盾的，所以编译器干脆禁止了任何方向的赋值。这就是**不变性**。

### 总结

| 型变 | 关系 (`Cat` -> `Animal`) | `Complex<T>` 的关系 | 常见位置 | 安全性保证 |
| :--- | :--- | :--- | :--- | :--- |
| **协变 (Covariance)** | `Cat` 是 `Animal` 的子类 | `Complex<Cat>` 是 `Complex<Animal>` 的子类 | 函数返回值, `readonly` 属性 | 保证**输出**的值总是符合预期的超类型 |
| **逆变 (Contravariance)** | `Cat` 是 `Animal` 的子类 | `Complex<Animal>` 是 `Complex<Cat>` 的子类 (反转) | 函数参数 | 保证**输入**的值总能被安全地处理 |
| **不变性 (Invariance)** | `Cat` 是 `Animal` 的子类 | `Complex<Cat>` 和 `Complex<Animal>` 互不兼容 | 可读写的属性、方法参数和返回值都用了 `T` | 保证**读和写**操作都不会破坏类型系统 |

**TS 的不变性检查**，本质上是 TypeScript 编译器为了防止因类型不匹配导致的运行时错误，而实施的一项编译时安全检查。当一个泛型类型参数同时出现在协变和逆变的位置时，编译器就会强制实行不变性，禁止父子类型之间的相互赋值。这是 TypeScript 类型系统强大和安全的重要体现。