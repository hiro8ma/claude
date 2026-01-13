# 式

## ブロック式

```rust
// 単一行（短い場合）
let x = { compute() };

// 複数行
let result = {
    let a = compute_a();
    let b = compute_b();
    a + b
};

// unsafeブロック
let value = unsafe { *ptr };
```

## クロージャ

```rust
// 短いクロージャ
let add = |a, b| a + b;

// 型注釈付き
let add: fn(i32, i32) -> i32 = |a, b| a + b;

// 複数行
let process = |x| {
    let y = transform(x);
    finalize(y)
};

// move
let data = vec![1, 2, 3];
let closure = move || {
    println!("{:?}", data);
};
```

## 構造体リテラル

```rust
// 単一行（小さい場合）
let point = Point { x: 1, y: 2 };

// 複数行
let user = User {
    id: 1,
    name: String::from("alice"),
    email: String::from("alice@example.com"),
};

// フィールド省略記法
let name = String::from("alice");
let user = User { name, ..Default::default() };
```

## 配列・タプル

```rust
// 配列
let arr = [1, 2, 3, 4, 5];
let arr = [0; 10];  // 10個の0

// 複数行
let arr = [
    first_element,
    second_element,
    third_element,
];

// タプル
let tuple = (1, "hello", true);
```

## 演算子

```rust
// 二項演算子の周りにスペース
let sum = a + b;
let result = x * y + z;

// 長い式は演算子の前で改行
let result = first_operand
    + second_operand
    + third_operand;

// 代入演算子は後で改行
let very_long_variable_name =
    compute_something_complex();
```

## 制御フロー

```rust
// if式（余分な括弧を避ける）
if condition {
    do_something();
}

// 値として使用
let value = if condition { a } else { b };

// 複数行条件
if very_long_condition
    && another_condition
{
    do_something();
}
```

## 関数呼び出し

```rust
// 単一行
let result = function(arg1, arg2, arg3);

// 複数行
let result = function(
    first_argument,
    second_argument,
    third_argument,
);

// メソッドチェーン
let result = data
    .iter()
    .filter(|x| x.is_valid())
    .map(|x| x.transform())
    .collect::<Vec<_>>();
```

## match式

```rust
match value {
    Pattern1 => result1,
    Pattern2 => result2,
    Pattern3 => {
        let x = compute();
        finalize(x)
    }
    _ => default_result,
}

// パターンは|で始めない
match value {
    A | B | C => result,  // Good
    // | A | B | C => result,  // Bad
}
```

## 範囲

```rust
// スペースなし
let range = 0..10;
let inclusive = 0..=9;

// 無制限範囲
let from = 5..;
let to = ..10;
let full = ..;
```
