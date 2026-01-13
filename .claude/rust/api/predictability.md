# 予測可能性

## スマートポインタのメソッド（C-SMART-PTR）

スマートポインタは固有メソッドを追加しない。関連関数として定義。

```rust
impl<T> Box<T> {
    // Good - 関連関数
    pub fn into_raw(b: Box<T>) -> *mut T { ... }

    // Bad - メソッド（Tのメソッドと衝突する可能性）
    // pub fn into_raw(self) -> *mut T { ... }
}

// 呼び出し
let ptr = Box::into_raw(boxed);  // 明確
```

## 変換の配置（C-CONV-SPECIFIC）

変換は最も具体的な型に配置。

```rust
// Good - strに配置（strはより具体的）
impl str {
    pub fn as_bytes(&self) -> &[u8] { ... }
}

// メソッドチェーンが可能
let bytes = my_string.as_bytes();
```

`from_`より`to_`/`as_`/`into_`を優先。

## メソッドの使用（C-METHOD）

明確なレシーバがある関数はメソッドにする。

```rust
// Good - メソッド
impl Vec<T> {
    pub fn len(&self) -> usize { ... }
}

// 利点：
// - 自動借用
// - 自動インポート
// - self記法
```

## 出力パラメータを避ける（C-NO-OUT）

複数値の返却にはタプルや構造体を使用。

```rust
// Bad - 出力パラメータ
fn get_size(out_width: &mut u32, out_height: &mut u32) { ... }

// Good - タプル
fn get_size() -> (u32, u32) { ... }

// Good - 構造体
fn get_size() -> Size { ... }
```

## 演算子オーバーロード（C-OVERLOAD）

演算子は直感的に振る舞う。数学的性質を保つ。

```rust
impl Add for Point {
    type Output = Self;

    fn add(self, other: Self) -> Self {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

// 結合性を保つ: (a + b) + c == a + (b + c)
// 可換性を保つ: a + b == b + a
```

## Deref/DerefMut（C-DEREF）

スマートポインタのみに実装。

```rust
// Good - スマートポインタ
impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &T {
        &self.value
    }
}

// Bad - 継承のシミュレーション
// impl Deref for MyStruct {
//     type Target = BaseStruct;
//     ...
// }
```

## コンストラクタ（C-CTOR）

静的メソッドとして実装。

```rust
impl Config {
    // 基本コンストラクタ
    pub fn new() -> Self { ... }

    // バリエーション
    pub fn with_capacity(cap: usize) -> Self { ... }

    // リソース型
    pub fn open(path: &Path) -> io::Result<Self> { ... }
    pub fn connect(addr: &str) -> io::Result<Self> { ... }
}
```

`Default`も実装する場合:

```rust
impl Default for Config {
    fn default() -> Self {
        Self::new()
    }
}
```
