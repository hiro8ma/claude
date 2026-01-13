# リンティング設定

## Clippy

Rustの公式リンター。

### 実行

```bash
# 基本実行
cargo clippy

# すべてのターゲット
cargo clippy --all-targets

# すべての機能
cargo clippy --all-features

# 警告をエラーとして扱う
cargo clippy -- -D warnings

# 特定のリントを許可
cargo clippy -- -A clippy::too_many_arguments
```

### リントレベル

```rust
// ファイル全体に適用
#![allow(clippy::module_name_repetitions)]
#![warn(clippy::pedantic)]
#![deny(clippy::unwrap_used)]

// 特定の項目に適用
#[allow(clippy::needless_return)]
fn example() -> i32 {
    return 42;
}
```

### 推奨設定

`Cargo.toml`:

```toml
[lints.clippy]
# 警告
pedantic = "warn"
nursery = "warn"

# エラー
unwrap_used = "deny"
expect_used = "deny"
panic = "deny"

# 許可
module_name_repetitions = "allow"
must_use_candidate = "allow"
```

または `clippy.toml`:

```toml
msrv = "1.70"
cognitive-complexity-threshold = 25
```

## rustfmt

コードフォーマッター。

### 実行

```bash
# フォーマット実行
cargo fmt

# チェックのみ
cargo fmt --check

# 特定ファイル
rustfmt src/main.rs
```

### 設定

`rustfmt.toml`:

```toml
edition = "2021"
max_width = 100
tab_spaces = 4
use_small_heuristics = "Default"
newline_style = "Auto"

# import整理
imports_granularity = "Crate"
group_imports = "StdExternalCrate"
reorder_imports = true

# その他
format_code_in_doc_comments = true
format_macro_matchers = true
```

## rust-analyzer

IDE統合のための言語サーバー。

### VS Code設定

```json
{
  "rust-analyzer.check.command": "clippy",
  "rust-analyzer.checkOnSave.command": "clippy",
  "rust-analyzer.cargo.features": "all",
  "[rust]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "rust-lang.rust-analyzer"
  }
}
```

## CI統合

### GitHub Actions

```yaml
name: Rust CI

on: [push, pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
        with:
          components: rustfmt, clippy

      - name: Format check
        run: cargo fmt --check

      - name: Clippy
        run: cargo clippy --all-targets -- -D warnings

      - name: Test
        run: cargo test
```

## 一般的なリント

### 推奨（有効化）

- `clippy::pedantic` - 厳格なチェック
- `clippy::nursery` - 新しいリント
- `clippy::cargo` - Cargo.tomlのチェック

### 重要（deny）

- `clippy::unwrap_used` - unwrap使用禁止
- `clippy::expect_used` - expect使用禁止
- `clippy::panic` - panic使用禁止
- `clippy::todo` - todo!使用禁止

### よく許可するもの

- `clippy::module_name_repetitions` - モジュール名の繰り返し
- `clippy::must_use_candidate` - must_use属性の候補
- `clippy::missing_errors_doc` - エラードキュメントの欠落
