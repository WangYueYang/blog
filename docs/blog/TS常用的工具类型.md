## TS 常用的工具类型

源码位置在: typescript/lib/libe.es5.d.ts 里。

### Partial
Partial 接收一个泛型 T ，主要的作用是把 T 里的所有属性都变为可选项

```ts
// Make all properties in T optional
type Partial<T> = {
  [P in keyof T]?: T[P]
}

// 例子
interface Person {
  name: string;
  age: number;
}

type PartialPerson = Partial<Person>;

// {
//     name?: string | undefined;
//     age?: number | undefined;
// }
```

### Required
Required 接收一个泛型 T， 主要的作用是把 T 里的所有属性变为必选项，和 Partial 作用相反

```ts
// Make all properties in T required
type Required<T> = {
  [P in keyof T]-?: T[P]
}

// 例子
interface Person {
  name?: string;
  age?: number;
}

type RequiredPerson = Required<Person>

// {
//   name: string;
//   age: number;
// }
```

### Readonly
Readonly 接收一个泛型 T，主要作用是把 T 里的所有属性都变成 readonly

```ts
// Make all properties in T readonly
type Readonly<T> = {
  readonly [P in T]: T[P]
}

// 例子
interface Person {
  name: string;
  age: number;
}

type ReadonlyPerson = Readonly<Person>

// {
//   readonly name: string;
//   readonly age: number;
// }
```

###  Pick
Pick 接收两个泛型 T, K，主要作用是把 T 中的 K 属性提取出来，形成一个新的类型

```ts
// From T, pick a set of properties whose keys are in the union K
type Pick<T, k extends keyof T> = {
  [P in K]: T[P]
}

// 例子

interface Test {
  a: number;
  b: boolean;
  c: string;
}

type PickTest = Pick<Test, 'b' | 'c'>

// {
//   b: boolean;
//   c: string;
// }
```

### Record
Record 接收两个泛型 K，T，主要作用是把 K 中的属性的类型都变为 T，返回一个新的类型

```ts
// Construct a type with a set of properties K of type T
type Record<K extends keyof any, T> = {
  [P in K]: T
}

// 例子
interface Test {
  a: number;
  b?: string;
}

type RecordTest = Record<'name' | 'age', Test>

// {
//   name: Test;
//   age: Test;
// }
```

### Exclude
Exclude 接收两个泛型 T，U，如果 T 能赋值给 U 类型的话，那么就会返回 never 类型，否则返回 T 类型。最终实现的效果就是将 T 中某些属于 U 的类型移除掉。

```ts
// Exclude from T those types that are assignable to U
type Exclude<T,U> = T extends U ? never : T

// 例子
type ExcludeTest = Exclude<'a', 'a' | 'b' | 'c'> // 'b' | 'c'
type ExcludeTest2 = Exclude<'a' | 'b', 'a' | 'b' | 'c'> // 'c'
type ExcludeTest3 = Exclude<'a', 'a'>  // never
```

### Extract
Extract 接收两个泛型 T，U，他的作用和 Exclude 相反，将 T 中属于 U 的属性保留

```ts
// Extract from T those types that are assignable to U
type Extract<T,U> = T extends U ? T : U

// 例子
type ExtractTest = Extract<'a' | 'b', 'a' | 'c'>  // 'a'
type ExtractTest2 = Extract<'a', 'c'>  // never
```

### Omit
Omit 接收两个泛型 T， K，主要作用是把 T 中除了 K 的属性取出来返回一个新的类型

```ts
// Construct a type with the properties of T except for those in type K.
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>

//  例子z
interface Test {
  a: string;
  b: number;
  c: boolean;
}

type OmitTest = Omit<Test, 'c'>
// {
//     a: string;
//     b: number;
// }
```
### NoNullable
NoNullable 接收一个泛型 T，主要作用是过滤类型中的 null 和 undefined

```ts
// Exclude null and undefined from T
type NoNullable<T> = T extends null | undefined ? never : T[P] = Exclude<T, null | undefined>

// 例子
type NoNullableTest = NoNullable<'a' | null | undefined> // 'a'
type NoNullableTest2 = NoNullable<null | undefined>  // never
```

### Parameters
Parameters 接收一个泛型 T，返回一个函数参数类型组成的元组
```ts
// Obtain the parameters of a function type in a tuple
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never

// 例子
type ParametersTest = Parameters<(a:number, b:string) => void> // [a: "string", b: number]'
let arr: ParametersTest = [123, 'test']
```