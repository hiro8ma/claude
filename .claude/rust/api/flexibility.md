# 柔軟性

## 中間結果の公開（C-INTERMEDIATE）

計算中の有用なデータをAPIで公開。

```rust
// Vec::binary_search - 見つかった位置または挿入位置を返す
match vec.binary_search(&target) {
    Ok(index) => println!("Found at {}", index),
    Err(insert_pos) => println!("Insert at {}", insert_pos),
}

// String::from_utf8 - エラー時に中間結果を返す
match String::from_utf8(bytes) {
    Ok(s) => println!("{}", s),
    Err(e) => {
        let valid_up_to = e.utf8_error().valid_up_to();
        let bytes = e.into_bytes();  // 所有権を取り戻せる
    }
}

// HashMap::insert - 既存値を返す
if let Some(old) = map.insert(key, new_value) {
    println!("Replaced: {:?}", old);
}
```

## 呼び出し側が制御（C-CALLER-CONTROL）

データのコピーと配置を呼び出し側に委ねる。

```rust
// Good - 所有権が必要なら直接取得
fn process(data: Vec<u8>) { ... }

// Good - 所有権が不要なら参照
fn analyze(data: &[u8]) { ... }

// Bad - 参照を受けて内部でクローン
fn store(data: &Vec<u8>) {
    let owned = data.clone();  // 呼び出し側が制御できない
    // ...
}
```

`Copy`は本当に必要な場合のみ実装。

```rust
// Good - 小さく、コピーが安い
#[derive(Copy, Clone)]
struct Point { x: i32, y: i32 }

// Bad - 大きな構造体にCopy
// #[derive(Copy, Clone)]
// struct LargeData { ... }
```

## ジェネリクス（C-GENERIC）

パラメータへの仮定を最小化。

```rust
// Bad - 具体的すぎる
fn sum_values(data: &[i64]) -> i64 {
    data.iter().sum()
}

// Good - ジェネリック
fn sum_values<I, T>(iter: I) -> T
where
    I: IntoIterator<Item = T>,
    T: std::iter::Sum,
{
    iter.into_iter().sum()
}
```

### ジェネリクスの利点

- 再利用性
- 静的ディスパッチ
- インライン最適化
- 型推論

### トレードオフ

- コードサイズ増加
- コンパイル時間
- シグネチャの複雑さ

## トレイトオブジェクト（C-OBJECT）

オブジェクト安全性を考慮して設計。

```rust
pub trait Draw {
    fn draw(&self);

    // オブジェクト安全でないメソッドは除外
    fn clone_box(&self) -> Box<dyn Draw>
    where
        Self: Sized,
    {
        unimplemented!()
    }
}

// 異種コレクション
let shapes: Vec<Box<dyn Draw>> = vec![
    Box::new(Circle::new()),
    Box::new(Rectangle::new()),
];
```

### トレイトオブジェクトの利点

- 異種コレクション対応
- コードサイズ削減

### トレードオフ

- 動的ディスパッチのオーバーヘッド
- ジェネリックメソッド不可
- Self型の制限

## `impl Trait`の使用

戻り値の型を隠蔽。

```rust
// 具体的な型を隠す
pub fn create_iterator() -> impl Iterator<Item = i32> {
    (0..10).filter(|x| x % 2 == 0)
}

// 引数ではジェネリクスと同等
pub fn process(iter: impl Iterator<Item = i32>) { ... }
// 等価: pub fn process<I: Iterator<Item = i32>>(iter: I) { ... }
```
