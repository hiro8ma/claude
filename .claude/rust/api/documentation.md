# ドキュメンテーション

## クレートレベルドキュメント（C-CRATE-DOC）

RFC 1687に準拠した包括的なドキュメントを提供。

```rust
//! # My Crate
//!
//! `my_crate` provides functionality for ...
//!
//! ## Quick Start
//!
//! ```rust
//! use my_crate::Client;
//!
//! let client = Client::new();
//! let result = client.process()?;
//! ```
//!
//! ## Features
//!
//! - Feature 1
//! - Feature 2
```

## すべての項目に例（C-EXAMPLE）

公開されるすべてのモジュール、トレイト、構造体、関数などに例を付ける。

```rust
/// Computes the factorial of a number.
///
/// # Examples
///
/// ```
/// use my_crate::factorial;
///
/// assert_eq!(factorial(5), 120);
/// assert_eq!(factorial(0), 1);
/// ```
pub fn factorial(n: u64) -> u64 {
    (1..=n).product()
}
```

## ?演算子の使用（C-QUESTION-MARK）

例では`unwrap()`や`try!`ではなく`?`を使用。

```rust
/// # Examples
///
/// ```
/// # fn main() -> Result<(), Box<dyn std::error::Error>> {
/// let config = Config::load("config.toml")?;
/// let server = Server::new(&config)?;
/// server.run()?;
/// # Ok(())
/// # }
/// ```
```

## 失敗条件の記述（C-FAILURE）

### Errorsセクション

```rust
/// Opens a file.
///
/// # Errors
///
/// Returns `Err` if:
/// - The file does not exist
/// - The user lacks permission to read the file
/// - The path is not valid UTF-8
pub fn open(path: &Path) -> Result<File, io::Error> { ... }
```

### Panicsセクション

```rust
/// Divides two numbers.
///
/// # Panics
///
/// Panics if `divisor` is zero.
pub fn divide(dividend: i32, divisor: i32) -> i32 {
    assert!(divisor != 0, "divisor must not be zero");
    dividend / divisor
}
```

### Safetyセクション（unsafe関数）

```rust
/// Dereferences a raw pointer.
///
/// # Safety
///
/// - `ptr` must be non-null and properly aligned
/// - `ptr` must point to a valid instance of `T`
/// - The memory must not be mutated during the lifetime of the reference
pub unsafe fn deref<T>(ptr: *const T) -> &T {
    &*ptr
}
```

## ハイパーリンク（C-LINK）

関連項目へのリンクを含める。

```rust
/// Creates a new [`Config`] with default values.
///
/// See also [`Config::from_file`] for loading from a file.
///
/// [`Config`]: struct.Config.html
/// [`Config::from_file`]: struct.Config.html#method.from_file
pub fn new() -> Config { ... }
```

## Cargo.tomlメタデータ（C-METADATA）

```toml
[package]
name = "my-crate"
version = "0.1.0"
authors = ["Author Name <email@example.com>"]
description = "A short description of the crate."
license = "MIT OR Apache-2.0"
repository = "https://github.com/user/repo"
documentation = "https://docs.rs/my-crate"
homepage = "https://example.com"
readme = "README.md"
keywords = ["keyword1", "keyword2"]
categories = ["category"]
```

## リリースノート（C-RELNOTES）

各リリースの変更内容を記録。

```markdown
# Changelog

## [0.2.0] - 2024-01-15

### Added
- New `Config::reload` method

### Changed
- **Breaking**: `Server::new` now requires `&Config` instead of `Config`

### Fixed
- Memory leak in connection pool

## [0.1.0] - 2024-01-01

Initial release
```

## 実装詳細の非表示（C-HIDDEN）

```rust
// 内部実装を隠す
#[doc(hidden)]
pub mod __private {
    // マクロ用の内部API
}

// クレート内のみ公開
pub(crate) fn internal_helper() { ... }
```
