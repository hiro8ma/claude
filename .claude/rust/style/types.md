# 型

## 基本型

```rust
// プリミティブ
let x: i32 = 42;
let f: f64 = 3.14;
let b: bool = true;
let c: char = 'a';

// 配列とスライス
let arr: [i32; 5] = [1, 2, 3, 4, 5];
let slice: &[i32] = &arr[1..3];

// ポインタ
let ptr: *const i32 = &x;
let mut_ptr: *mut i32 = &mut x;

// 参照
let r: &i32 = &x;
let mr: &mut i32 = &mut x;
```

## 関数型

```rust
// 関数ポインタ
let f: fn(i32) -> i32 = |x| x * 2;

// クロージャ型
fn apply<F>(f: F, x: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(x)
}
```

## パス

```rust
// 完全修飾パス
let map: std::collections::HashMap<String, i32>;

// ジェネリクス付き
let result: Result<String, std::io::Error>;

// トレイトオブジェクト
let writer: &dyn std::io::Write;
```

## ジェネリクス型

```rust
// カンマ後にスペース、末尾カンマなし
let map: HashMap<String, Vec<i32>>;
let result: Result<Option<T>, Error>;

// 角括弧の周りはスペースなし
// HashMap< String, i32 >  // Bad
HashMap<String, i32>       // Good
```

## トレイト境界

```rust
// 単一行
fn process<T: Clone + Debug>(value: T) {}

// +の周りにスペース
T: Clone + Debug + Default

// 複数行
fn complex<T>(value: T)
where
    T: Clone
        + Debug
        + Default
        + Send,
{
}
```

## impl Trait

```rust
// 引数
fn process(iter: impl Iterator<Item = i32>) {}

// 戻り値
fn create() -> impl Iterator<Item = i32> {
    vec![1, 2, 3].into_iter()
}
```

## dyn Trait

```rust
// トレイトオブジェクト
let writer: Box<dyn Write>;
let readers: Vec<Box<dyn Read>>;

// ライフタイム付き
let handler: &dyn Handler<'_>;
```

## ライフタイム

```rust
// 参照型
&'a T
&'a mut T

// 構造体
struct Ref<'a, T> {
    data: &'a T,
}

// 静的ライフタイム
&'static str
```

## 型エイリアス

```rust
// 単純なエイリアス
type Result<T> = std::result::Result<T, Error>;

// 複雑な型の簡略化
type Callback = Box<dyn Fn(i32) -> i32 + Send + 'static>;
```

## タプル型

```rust
// 小さい場合は単一行
type Point = (i32, i32);

// ユニット型
type Unit = ();

// 複数行
type ComplexTuple = (
    FirstType,
    SecondType,
    ThirdType,
);
```
