# Cargo.toml規約

## 基本構成

```toml
[package]
name = "my-crate"
version = "0.1.0"
edition = "2021"
rust-version = "1.70"
authors = ["Author Name <email@example.com>"]
license = "MIT OR Apache-2.0"
repository = "https://github.com/user/repo"
homepage = "https://example.com"
documentation = "https://docs.rs/my-crate"
readme = "README.md"
keywords = ["keyword1", "keyword2"]
categories = ["category"]
description = "A short description of the crate."

[dependencies]
serde = { version = "1.0", features = ["derive"] }
tokio = { version = "1", features = ["full"] }

[dev-dependencies]
criterion = "0.5"

[build-dependencies]
cc = "1.0"

[features]
default = ["std"]
std = []
async = ["tokio"]

[lints.rust]
unsafe_code = "forbid"

[lints.clippy]
pedantic = "warn"
```

## セクション順序

1. `[package]` - 常に最初
2. `[features]`
3. `[dependencies]`
4. `[dev-dependencies]`
5. `[build-dependencies]`
6. `[lints]`
7. その他

## [package]セクション

### 必須フィールド順

1. `name`
2. `version`
3. その他（アルファベット順）
4. `description`（最後）

```toml
[package]
name = "my-crate"
version = "0.1.0"
edition = "2021"
license = "MIT"
description = "Description goes last."
```

## 依存関係

### バージョン指定

```toml
[dependencies]
# 互換性のあるバージョン
serde = "1.0"           # >=1.0.0, <2.0.0
serde = "1.0.104"       # >=1.0.104, <2.0.0

# 正確なバージョン
serde = "=1.0.104"

# 範囲
serde = ">=1.0, <1.5"

# ワイルドカード
serde = "1.*"
```

### 詳細指定

```toml
[dependencies]
# 機能付き
serde = { version = "1.0", features = ["derive"] }

# オプション依存
tokio = { version = "1", optional = true }

# Git依存
my-lib = { git = "https://github.com/user/repo", branch = "main" }

# パス依存
local-lib = { path = "../local-lib" }

# デフォルト機能無効化
serde = { version = "1.0", default-features = false }
```

### アルファベット順ソート

```toml
[dependencies]
anyhow = "1.0"
chrono = "0.4"
serde = "1.0"
tokio = "1"
```

## 機能（Features）

```toml
[features]
# デフォルト機能
default = ["std", "json"]

# 個別機能
std = []
json = ["serde_json"]
async = ["tokio"]

# 依存関係の機能を有効化
full = ["async", "json"]
```

## Workspace

```toml
[workspace]
members = [
    "crates/core",
    "crates/cli",
    "crates/lib",
]
resolver = "2"

[workspace.package]
version = "0.1.0"
edition = "2021"
license = "MIT"

[workspace.dependencies]
serde = "1.0"
tokio = "1"
```

## メタデータ

### 著者

```toml
authors = ["Full Name <email@example.com>"]
```

### ライセンス

```toml
# SPDX形式
license = "MIT"
license = "Apache-2.0"
license = "MIT OR Apache-2.0"

# ファイル参照
license-file = "LICENSE"
```

### 説明

- 80文字で折り返し
- クレート名で始めない
- 最初の文を要約として単独で成立させる

```toml
description = """
A fast and reliable HTTP client for Rust.
Supports async/await and connection pooling.
"""
```
