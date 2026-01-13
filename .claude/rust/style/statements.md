# 文

## let文

```rust
// 基本形
let x = 5;
let name: String = String::from("hello");

// パターン
let (a, b) = (1, 2);
let Some(value) = optional else { return };

// 長い初期化
let result =
    very_long_function_call_that_returns_something();

// 型注釈が長い場合
let value: SomeVeryLongTypeName<WithGenerics> =
    compute_value();
```

## let-else文

```rust
// 単一行（短い場合）
let Some(x) = opt else { return };

// 複数行
let Some(value) = compute_optional() else {
    log::warn!("Value not found");
    return Err(Error::NotFound);
};

// 初期化が複数行
let Some(value) = some_function(
    first_arg,
    second_arg,
) else {
    return;
};
```

## マクロ呼び出し

文の位置では括弧または角括弧を使用し、セミコロンで終了。

```rust
println!("Hello, world!");
vec![1, 2, 3];
assert_eq!(a, b);

// 複数行
println!(
    "Value: {}, Count: {}",
    value,
    count,
);
```

## 式文

```rust
// セミコロンで終了
compute_something();
x + y;

// ブロック末尾の値はセミコロンなし
fn example() -> i32 {
    let x = 5;
    x + 1  // セミコロンなし = 戻り値
}
```

## 制御フロー文

```rust
// return
return Ok(value);
return;  // 早期リターン

// break/continue
loop {
    if condition {
        break;
    }
    continue;
}

// break with value
let result = loop {
    if let Some(v) = try_get() {
        break v;
    }
};
```

## 代入文

```rust
// 単純な代入
x = 5;

// 複合代入
x += 1;
x *= 2;

// 分割代入
(a, b) = (b, a);
```

## 変数の可変性

```rust
// 不変（デフォルト）
let x = 5;
// x = 6;  // エラー

// 可変
let mut x = 5;
x = 6;  // OK

// シャドーイング
let x = 5;
let x = x + 1;  // 新しい変数
let x = "hello";  // 型も変更可能
```

## 定数とstatic

```rust
// 定数（コンパイル時評価）
const MAX_SIZE: usize = 1024;
const PI: f64 = 3.14159;

// static（実行時にアドレスを持つ）
static COUNTER: AtomicUsize = AtomicUsize::new(0);

// static mut（unsafe必須）
static mut CACHE: Option<Vec<i32>> = None;
```
