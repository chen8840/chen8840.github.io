## 一、用key in keyof T能继续原对象的readonly, optional等属性
  错误的做法：
  ```typescript
  interface Todo1 {
    readonly title: string
    description: string
    completed: boolean
  }

  type MyExclude<T, K> = T extends K ? never : T;

  type MyOmit<T, K> = {
    [key in MyExclude<keyof T, K>]: T[key]
  };

  // A的类型是{title: string}而不是{readonly title: string}
  type A = MyOmit<Todo1, 'description' | 'completed'>;
  ```
  正确的做法：
  ```typescript
  type MyOmit<T, K> = {
    [key in keyof T as MyExclude<key, K>]: T[key]
  }

  // A的类型是{readonly title: string}
  type A = MyOmit<Todo1, 'description' | 'completed'>;
  ```

<br>

## 二、当一个映射类型（Mapped Type），被应用于一个原始类型（number, string, boolean, null, undefined, symbol, bigint）时，TypeScript 会直接跳过映射逻辑，并返回原始类型本身。
比如：
```typescript
type TestType<T> = { readonly [P in keyof T]: 1 }

// --- 原始类型 ---
type TestNumber = TestType<123>;         // 结果: 123
type TestString = TestType<'hello'>;     // 结果: "hello"
type TestBoolean = TestType<true>;       // 结果: true
type TestNull = TestType<null>;          // 结果: null
type TestUndefined = TestType<undefined>;  // 结果: undefined

// --- 对象类型 (正常工作) ---
type TestObject = TestType<{ name: string; age: number }>;
// 结果: { readonly name: 1; readonly age: 1; }

type TestArray = TestType<['a', 'b']>;
// 结果: { readonly 0: 1; readonly 1: 1; readonly length: 1; readonly toString: 1; ... 等等数组的方法 }
// 注意：数组是对象，所以映射类型会正常工作，并遍历其所有属性（包括数字索引和方法）。

// 当T[P]为number, string时，返回DeepReadonly<T[P]>，DeepReadonly<T[P]>返回number,string
// 当T[P]为{}，函数时，返回T[P]
// 当T[P]为数组或对象时，进入递归
type DeepReadonly<T> = { readonly [P in keyof T]: keyof T[P] extends never ? T[P] : DeepReadonly<T[P]> }
```

## 三、key in keyof T也能作用于数组