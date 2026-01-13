# マクロ設計

## 入力構文は出力を反映（C-EVOCATIVE）

マクロの入力は出力を示唆する。

```rust
// Good - 構造体を生成するなら struct キーワードを使用
macro_rules! make_struct {
    (struct $name:ident { $($field:ident: $ty:ty),* $(,)? }) => {
        struct $name {
            $($field: $ty),*
        }
    };
}

make_struct! {
    struct Point {
        x: i32,
        y: i32,
    }
}

// Good - 定数なら const を使用
macro_rules! define_const {
    (const $name:ident: $ty:ty = $value:expr;) => {
        const $name: $ty = $value;
    };
}

define_const!(const MAX_SIZE: usize = 1024;);
```

## 属性との組み合わせ（C-MACRO-ATTR）

マクロは属性の適用をサポート。

```rust
macro_rules! define_items {
    (
        $(
            $(#[$attr:meta])*
            $vis:vis fn $name:ident() $body:block
        )*
    ) => {
        $(
            $(#[$attr])*
            $vis fn $name() $body
        )*
    };
}

define_items! {
    #[inline]
    pub fn fast_fn() { }

    #[cfg(test)]
    fn test_helper() { }
}
```

## あらゆる位置で動作（C-ANYWHERE）

モジュールレベルと関数スコープ両方で動作。

```rust
macro_rules! define_helper {
    ($name:ident) => {
        fn $name() -> i32 { 42 }
    };
}

// モジュールレベル
define_helper!(module_fn);

fn example() {
    // 関数スコープ
    define_helper!(local_fn);
    assert_eq!(local_fn(), 42);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_both_contexts() {
        // テストで両方のコンテキストを確認
        define_helper!(test_fn);
        assert_eq!(test_fn(), 42);
    }
}
```

## 可視性のサポート（C-MACRO-VIS）

標準的な可視性指定子をサポート。

```rust
macro_rules! define_struct {
    ($vis:vis struct $name:ident { $($field_vis:vis $field:ident: $ty:ty),* $(,)? }) => {
        $vis struct $name {
            $($field_vis $field: $ty),*
        }
    };
}

define_struct! {
    pub struct PublicStruct {
        pub public_field: i32,
        private_field: String,
    }
}

define_struct! {
    struct PrivateStruct {
        field: i32,
    }
}
```

## 柔軟な型フラグメント（C-MACRO-TY）

`$t:ty`は様々な型構文で動作。

```rust
macro_rules! make_vec {
    ($t:ty) => {
        Vec::<$t>::new()
    };
}

// 様々な型で動作
let _: Vec<i32> = make_vec!(i32);                    // プリミティブ
let _: Vec<String> = make_vec!(String);              // 標準型
let _: Vec<std::io::Error> = make_vec!(std::io::Error);  // パス
let _: Vec<Box<dyn Send>> = make_vec!(Box<dyn Send>);    // ジェネリック
let _: Vec<&str> = make_vec!(&str);                  // 参照
let _: Vec<[u8; 4]> = make_vec!([u8; 4]);           // 配列
```

## 宣言的マクロのベストプラクティス

```rust
macro_rules! my_macro {
    // 複数のパターンは具体的なものから
    ($x:expr, $y:expr) => { ... };
    ($x:expr) => { ... };
    () => { ... };
}

// 再帰は末尾に
macro_rules! count {
    () => { 0 };
    ($head:tt $($tail:tt)*) => { 1 + count!($($tail)*) };
}
```

## 手続き的マクロ

```rust
use proc_macro::TokenStream;

#[proc_macro_derive(MyDerive, attributes(my_attr))]
pub fn my_derive(input: TokenStream) -> TokenStream {
    // ...
}

#[proc_macro_attribute]
pub fn my_attribute(attr: TokenStream, item: TokenStream) -> TokenStream {
    // ...
}

#[proc_macro]
pub fn my_macro(input: TokenStream) -> TokenStream {
    // ...
}
```
