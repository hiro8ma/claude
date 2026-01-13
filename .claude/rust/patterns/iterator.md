# イテレータパターン

## 基本的なイテレータメソッド

```rust
let numbers = vec![1, 2, 3, 4, 5];

// map: 変換
let doubled: Vec<_> = numbers.iter()
    .map(|x| x * 2)
    .collect();

// filter: フィルタリング
let evens: Vec<_> = numbers.iter()
    .filter(|x| *x % 2 == 0)
    .collect();

// find: 最初の一致
let first_even = numbers.iter()
    .find(|x| *x % 2 == 0);

// fold: 畳み込み
let sum: i32 = numbers.iter()
    .fold(0, |acc, x| acc + x);
```

## チェーン

```rust
let result: Vec<String> = data
    .iter()
    .filter(|item| item.is_valid())
    .map(|item| item.transform())
    .filter_map(|item| item.try_convert())
    .take(10)
    .collect();
```

## カスタムイテレータ

```rust
pub struct Counter {
    count: usize,
    max: usize,
}

impl Counter {
    pub fn new(max: usize) -> Self {
        Self { count: 0, max }
    }
}

impl Iterator for Counter {
    type Item = usize;

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < self.max {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

// 使用
for n in Counter::new(5) {
    println!("{}", n);  // 1, 2, 3, 4, 5
}
```

## IntoIteratorの実装

```rust
pub struct MyCollection {
    items: Vec<i32>,
}

impl IntoIterator for MyCollection {
    type Item = i32;
    type IntoIter = std::vec::IntoIter<i32>;

    fn into_iter(self) -> Self::IntoIter {
        self.items.into_iter()
    }
}

impl<'a> IntoIterator for &'a MyCollection {
    type Item = &'a i32;
    type IntoIter = std::slice::Iter<'a, i32>;

    fn into_iter(self) -> Self::IntoIter {
        self.items.iter()
    }
}
```

## 遅延評価

```rust
// イテレータは遅延評価
let iter = (0..1000000)
    .map(|x| {
        println!("Processing {}", x);  // collectまで実行されない
        x * 2
    })
    .filter(|x| x % 4 == 0);

// collectで初めて実行
let result: Vec<_> = iter.take(5).collect();
```

## 便利なメソッド

```rust
// enumerate: インデックス付き
for (i, item) in items.iter().enumerate() {
    println!("{}: {}", i, item);
}

// zip: 2つのイテレータを結合
let pairs: Vec<_> = a.iter().zip(b.iter()).collect();

// chain: 連結
let combined: Vec<_> = first.iter()
    .chain(second.iter())
    .collect();

// flatten: ネストを平坦化
let flat: Vec<_> = nested.iter()
    .flatten()
    .collect();

// partition: 条件で分割
let (evens, odds): (Vec<_>, Vec<_>) = numbers.iter()
    .partition(|x| *x % 2 == 0);

// any/all: 条件チェック
let has_zero = numbers.iter().any(|x| *x == 0);
let all_positive = numbers.iter().all(|x| *x > 0);
```

## パフォーマンス

```rust
// collect前にできるだけ絞り込む
let result: Vec<_> = large_data
    .iter()
    .filter(|x| x.is_relevant())  // 先にフィルタ
    .map(|x| x.expensive_transform())
    .collect();

// サイズヒント
let (lower, upper) = iter.size_hint();
let mut result = Vec::with_capacity(upper.unwrap_or(lower));
result.extend(iter);
```
