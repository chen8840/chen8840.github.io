当然，在 TypeScript 的类型编程中，判断两种类型是否“完全相等”是一个经典且比看起来要复杂的问题。一个简单 `extends` 检查是不足够的。

最健壮和被社区公认的“黄金标准”方法是利用**条件类型和泛型函数类型的协变与逆变规则**来强制进行一种“不变性”检查。

### 最终的、最可靠的方案

这个方案看起来有些“黑魔法”，但它能正确处理几乎所有的边缘情况，包括 `any`、`never`、`unknown`、`readonly` 修饰符等。

```typescript
type IsEqual<A, B> =
  (<T>() => T extends A ? 1 : 2) extends
  (<T>() => T extends B ? 1 : 2)
    ? true
    : false;
```

---

### 如何使用它

你可以用它来创建新的类型或在类型编程中进行断言。

```typescript
// --- 示例 ---

// 1. 基础类型
type T1 = IsEqual<string, string>;       // true
type T2 = IsEqual<string, number>;       // false

// 2. 特殊类型 any
type T3 = IsEqual<any, string>;         // false
type T4 = IsEqual<string, any>;         // false
type T5 = IsEqual<any, any>;             // true

// 3. 特殊类型 unknown 和 never
type T6 = IsEqual<unknown, any>;         // false
type T7 = IsEqual<never, never>;         // true
type T8 = IsEqual<never, string>;        // false

// 4. 对象和接口
type T9 = IsEqual<{ a: 1 }, { a: 1 }>;   // true
type T10 = IsEqual<{ a: 1 }, { a: number }>; // false
type T11 = IsEqual<{ a: 1; b: 2 }, { a: 1 }>; // false

// 5. readonly 修饰符
type T12 = IsEqual<string[], readonly string[]>; // false
type T13 = IsEqual<readonly string[], readonly string[]>; // true

// 6. 联合类型
type T14 = IsEqual<string | number, number | string>; // true
type T15 = IsEqual<string | number, string>;          // false
```

---

### 深入解析：为什么这个方法有效（以及为什么简单方法会失败）

为了理解为什么上面那个复杂的类型是必要的，我们先看看简单的方法为什么行不通。

#### 1. 失败的尝试一：`A extends B`

这是大家最先想到的方法，但它检查的是**子类型关系 (subtype)**，而不是**相等 (equality)**。

`A extends B` 的意思是：“一个 `A` 类型的值是否可以安全地赋值给一个 `B` 类型的变量？”

```typescript
// { a: 1, b: 2 } 是 { a: 1 } 的子类型
type IsSubtype = { a: 1; b: 2 } extends { a: 1 } ? true : false; // true

// 但它们并不相等！
type IsNotEqual = IsEqual<{ a: 1; b: 2 }, { a: 1 }>; // false (使用我们的正确实现)
```

#### 2. 失败的尝试二：双向 `extends`

下一个自然的想法是进行双向检查：`A` 是 `B` 的子类型，并且 `B` 也是 `A` 的子类型。

```typescript
type IsEqualAttempt<A, B> =
  A extends B
    ? (B extends A ? true : false)
    : false;
```

这个方法解决了上面提到的子类型问题，并且在大多数情况下工作得很好。

```typescript
type Test1 = IsEqualAttempt<{ a: 1; b: 2 }, { a: 1 }>; // false, 正确!
type Test2 = IsEqualAttempt<{ a: 1 }, { a: 1 }>; // true, 正确!
```

**但是，它在处理 `any` 时会彻底失败。**

`any` 类型在条件类型中有一个特殊的规则：如果 `extends` 的左边是 `any`，那么条件类型会根据右边是否为 `any` 来返回 `true` 或 `false` 的联合类型 `boolean`。

```typescript
// string extends any => true
// any extends string => boolean
// 所以最终结果是 boolean，而不是我们期望的 false
type AnyTest = IsEqualAttempt<string, any>; // boolean
```
这显然不是我们想要的精确判断。

#### 3. 成功的方案：利用函数类型的不变性 (Invariance)

我们最终的解决方案利用了 TypeScript 类型系统的一个高级特性。

```typescript
(<T>() => T extends A ? 1 : 2)
```

这部分代码创建了一个**泛型函数类型**。这个函数不接受参数，它的返回值类型依赖于一个内部泛型 `T`。

当我们比较两个这样的函数类型时：

`Func<A> extends Func<B> ? true : false`

TypeScript 会进行非常严格的比较。为了使这两个泛型函数类型相等，泛型约束 `A` 和 `B` **必须是完全相同的类型**。这种比较方式被称为**不变性 (invariance)** 检查。

- 如果 `A` 和 `B` 是完全相同的类型（例如 `string` 和 `string`），那么这两个函数类型结构完全相同，`extends` 返回 `true`。
- 如果 `A` 和 `B` 有任何细微差别（例如 `string` 和 `any`，或 `string[]` 和 `readonly string[]`），函数类型结构就会被认为不兼容，`extends` 返回 `false`。

这种方法巧妙地绕过了 `extends` 的子类型检查和 `any` 的特殊规则，实现了一种真正意义上的“类型恒等”判断。