# フォーマット基本

## インデント

- スペース使用（タブ不可）
- 1レベル = 4スペース
- ブロックインデント推奨

```rust
fn example() {
    if condition {
        do_something();
    }
}
```

## 行幅

最大100文字。

```rust
// OK
let result = some_function(arg1, arg2, arg3);

// 長い場合は改行
let result = some_very_long_function_name(
    argument_one,
    argument_two,
    argument_three,
);
```

## 末尾カンマ

複数行のリストには末尾カンマを付ける。

```rust
// 単一行: 末尾カンマなし
let tuple = (1, 2, 3);

// 複数行: 末尾カンマあり
let tuple = (
    first_element,
    second_element,
    third_element,
);
```

## 空行

アイテム間は0行または1行の空行。

```rust
fn first() {
    // ...
}

fn second() {
    // ...
}
```

## 末尾空白

すべての行末から除去。

## コメント

- ブロックコメントより行コメント（`//`）を推奨
- `//`の後にスペース1つ
- 完全な文として大文字で始め句点で終わる

```rust
// This is a good comment.
let x = 5;

/* 避ける: ブロックコメント */
```

## ドキュメントコメント

- `///`を`/** */`より推奨
- 属性の前に配置

```rust
/// Returns the sum of two numbers.
///
/// # Examples
///
/// ```
/// let result = add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

## 属性

各属性は独立した行に配置。`derive`は統合。

```rust
#[derive(Debug, Clone, PartialEq)]
#[serde(rename_all = "camelCase")]
pub struct User {
    name: String,
}
```
