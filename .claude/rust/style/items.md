# アイテム（関数・構造体等）

## ファイル内の順序

1. `extern crate`（ファイル先頭、アルファベット順）
2. `use`文（`self`/`super`を先頭に）
3. モジュール宣言
4. アイテム定義

```rust
extern crate serde;

use std::collections::HashMap;
use std::io::{self, Read, Write};

use crate::utils;
use super::parent;

mod submodule;

pub struct MyStruct;
```

## 関数定義

```rust
// 単一行
fn short_fn(x: i32) -> i32 { x * 2 }

// 複数行
pub fn longer_function(
    first_arg: i32,
    second_arg: String,
    third_arg: bool,
) -> Result<(), Error> {
    // ...
}

// 修飾子の順序
pub async unsafe fn complex_fn() {}
```

## 構造体

```rust
// 単一行（小さい場合）
struct Point { x: i32, y: i32 }

// 複数行（推奨）
struct User {
    id: u64,
    name: String,
    email: String,
}

// タプル構造体
struct Color(u8, u8, u8);

// ユニット構造体
struct Marker;
```

## 列挙型

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(u8, u8, u8),
}
```

## トレイト

```rust
// 空のトレイト
trait Marker {}

// メソッドあり
trait Animal {
    fn name(&self) -> &str;
    fn speak(&self) {
        println!("...");
    }
}
```

## impl

```rust
// 通常のimpl
impl MyStruct {
    pub fn new() -> Self {
        Self {}
    }
}

// トレイト実装
impl Animal for Dog {
    fn name(&self) -> &str {
        &self.name
    }
}

// ジェネリクス
impl<T: Clone> MyContainer<T> {
    fn clone_contents(&self) -> T {
        self.value.clone()
    }
}
```

## ジェネリクス

```rust
// 単一行
fn process<T: Clone>(value: T) -> T {}

// 複数行
fn complex_generic<
    T: Clone + Debug,
    U: Default,
>(
    first: T,
    second: U,
) -> Result<T, Error> {
    // ...
}
```

## where句

```rust
fn process<T, U>(x: T, y: U) -> i32
where
    T: Clone + Debug,
    U: Default + Display,
{
    // ...
}
```

## use文

```rust
// グループ化
use std::{
    collections::HashMap,
    io::{self, Read, Write},
    sync::Arc,
};

// ワイルドカードは避ける
// use std::collections::*;  // 避ける
use std::collections::HashMap;  // 明示的に
```
