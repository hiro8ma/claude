# エラー処理

## Result型

回復可能なエラーには`Result`を使用。

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_file(path: &str) -> Result<String, io::Error> {
    let mut file = File::open(path)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}
```

## ?演算子

エラーを簡潔に伝播。

```rust
// ?演算子でエラー伝播
fn process() -> Result<(), Error> {
    let data = read_file("input.txt")?;
    let parsed = parse(&data)?;
    save(parsed)?;
    Ok(())
}

// 明示的なmatchと等価
fn process_explicit() -> Result<(), Error> {
    let data = match read_file("input.txt") {
        Ok(d) => d,
        Err(e) => return Err(e.into()),
    };
    // ...
    Ok(())
}
```

## カスタムエラー型

```rust
use std::fmt;

#[derive(Debug)]
pub enum AppError {
    Io(io::Error),
    Parse(String),
    NotFound { id: u64 },
}

impl fmt::Display for AppError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            AppError::Io(e) => write!(f, "IO error: {}", e),
            AppError::Parse(msg) => write!(f, "Parse error: {}", msg),
            AppError::NotFound { id } => write!(f, "Not found: {}", id),
        }
    }
}

impl std::error::Error for AppError {}

impl From<io::Error> for AppError {
    fn from(err: io::Error) -> Self {
        AppError::Io(err)
    }
}
```

## thiserrorクレート

ボイラープレートを削減。

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum AppError {
    #[error("IO error: {0}")]
    Io(#[from] io::Error),

    #[error("Parse error: {0}")]
    Parse(String),

    #[error("Not found: {id}")]
    NotFound { id: u64 },
}
```

## anyhowクレート

アプリケーションコードでの簡易エラー処理。

```rust
use anyhow::{Context, Result};

fn main() -> Result<()> {
    let config = read_config()
        .context("Failed to read config")?;

    process(&config)
        .with_context(|| format!("Failed to process: {:?}", config))?;

    Ok(())
}
```

## panic!の使用

回復不能なエラーのみ。

```rust
// プログラマのミスを示す場合
fn get_item(index: usize) -> &Item {
    self.items.get(index)
        .expect("index out of bounds - this is a bug")
}

// 本番コードではpanicを避ける
// 代わりにResultを返す
fn get_item_safe(index: usize) -> Option<&Item> {
    self.items.get(index)
}
```

## Option型

値の有無を表現。

```rust
fn find_user(id: u64) -> Option<User> {
    users.iter().find(|u| u.id == id).cloned()
}

// パターンマッチ
match find_user(42) {
    Some(user) => println!("Found: {}", user.name),
    None => println!("Not found"),
}

// メソッドチェーン
let name = find_user(42)
    .map(|u| u.name)
    .unwrap_or_else(|| "Unknown".to_string());
```
