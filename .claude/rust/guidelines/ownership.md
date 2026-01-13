# 所有権と借用

## 所有権の基本ルール

1. 各値は所有者（変数）を1つだけ持つ
2. 所有者がスコープを抜けると値はドロップされる
3. 所有権は移動（move）できる

```rust
// 所有権の移動
let s1 = String::from("hello");
let s2 = s1;  // s1の所有権がs2に移動
// println!("{}", s1);  // エラー: s1は無効

// Copyトレイト実装型は複製される
let x = 5;
let y = x;  // コピー
println!("{}", x);  // OK
```

## 借用（参照）

所有権を移動せずに値を使用。

```rust
// 不変借用
fn calculate_length(s: &String) -> usize {
    s.len()
}

let s = String::from("hello");
let len = calculate_length(&s);  // 借用
println!("{}", s);  // OK: 所有権は維持

// 可変借用
fn append(s: &mut String) {
    s.push_str(" world");
}

let mut s = String::from("hello");
append(&mut s);
```

## 借用のルール

1. 不変参照は複数同時に可能
2. 可変参照は1つのみ
3. 不変参照と可変参照は同時に存在できない

```rust
let mut s = String::from("hello");

// OK: 複数の不変参照
let r1 = &s;
let r2 = &s;
println!("{}, {}", r1, r2);

// OK: 不変参照の使用後に可変参照
let r3 = &mut s;
r3.push_str(" world");

// エラー: 不変参照と可変参照の同時使用
// let r1 = &s;
// let r2 = &mut s;
// println!("{}", r1);
```

## ライフタイム

参照の有効期間を明示。

```rust
// ライフタイム省略可能なケース
fn first_word(s: &str) -> &str {
    &s[..s.find(' ').unwrap_or(s.len())]
}

// 明示が必要なケース
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

// 構造体でのライフタイム
struct Excerpt<'a> {
    part: &'a str,
}
```

## Cloneの適切な使用

パフォーマンスとのトレードオフを考慮。

```rust
// 必要な場合のみClone
let s1 = String::from("hello");
let s2 = s1.clone();  // 明示的なコピー

// 参照で十分な場合はCloneを避ける
fn process(s: &str) {  // &Stringではなく&str
    // ...
}
```
