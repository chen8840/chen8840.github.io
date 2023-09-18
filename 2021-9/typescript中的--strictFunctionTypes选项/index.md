### typescript中的--strictFunctionTypes选项

什么是协变和逆变

<span style="color: red">原来，在泛型参数上添加了in关键字作为泛型修饰符的话，那么那个泛型参数就只能用作方法的输入参数，或者只写属性的参数，不能作为方法返回值等，总之就是只能是“入”，不能出。out关键字反之。</span>

上面标红的文字中，in表示逆变，out表示协变。用例子看下什么是协变和逆变。

逆变（contravariant）例子

```typescript
class Animal {
  private a: number = 1;
}
class Dog extends Animal{
  private b: number = 1;
}
class Cat extends Animal{
  private c: number = 1;
}

interface Comparer<T> {
  compare: (a: T, b: T) => number;
}

declare let animalComparer: Comparer<Animal>;
declare let dogComparer: Comparer<Dog>;
animalComparer = dogComparer; // Error
dogComparer = animalComparer; // Ok
```

T泛型只作为输入参数，

此时Error的行将dogComparer赋给animalComparer，而dogComparer认为T是Dog类型，故而会调用Dog类型的专用方法，animalComparer的T限定为Animal类型，可能传Animal对象或Cat对象到函数参数，故而会报错。

OK行，则将animalComparer赋给dogComparer，animalComparer认为T是Animal类型，故而只会调用Animal类型的方法，dogComparer的T限定为Dog类型，一定匹配Animal类型，故而不会报错。



协变（covariant）例子

```typescript
class Animal {
  private a: number = 1;
}
class Dog extends Animal{
  private b: number = 1;
}
class Cat extends Animal{
  private c: number = 1;
}

interface Comparer<T> {
  compare: () => T;
}

declare let animalComparer: Comparer<Animal>;
declare let dogComparer: Comparer<Dog>;
animalComparer = dogComparer; // Ok
dogComparer = animalComparer; // Error
```

T类型只作为返回值，

此时OK的行将dogComparer赋给animalComparer，dogComparer的compare函数返回Dog类型，肯定可以赋给animalComparer的compare函数的返回类型Animal。

Error行，则将animalComparer赋给dogComparer，animalComparer的compare函数的返回类型Animal是不能赋给dogComparer的compare函数返回Dog类型的，故报错。



--strictFunctionTypes限定主要关注的逆变赋值，关闭strictFunctionTypes开关后，逆变检测变成了双变检测（bivariantly），故而第一个例子里面的Error不会报错了。
