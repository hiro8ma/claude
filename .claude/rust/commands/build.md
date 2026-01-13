# ビルド

## 基本コマンド

```bash
# デバッグビルド
cargo build

# リリースビルド
cargo build --release

# 特定パッケージ（ワークスペース）
cargo build -p package-name

# すべてのターゲット
cargo build --all-targets
```

## チェック（コンパイルのみ）

```bash
# 高速なチェック
cargo check

# すべてのターゲット
cargo check --all-targets

# 特定機能
cargo check --features "feature1,feature2"
```

## クロスコンパイル

```bash
# ターゲット追加
rustup target add x86_64-unknown-linux-gnu
rustup target add aarch64-apple-darwin
rustup target add x86_64-pc-windows-gnu

# クロスビルド
cargo build --target x86_64-unknown-linux-gnu
cargo build --release --target aarch64-apple-darwin
```

## 機能フラグ

```bash
# デフォルト機能のみ
cargo build

# 追加機能
cargo build --features "json,async"

# すべての機能
cargo build --all-features

# デフォルト機能を無効化
cargo build --no-default-features

# 組み合わせ
cargo build --no-default-features --features "minimal"
```

## 最適化

```bash
# リリースビルド
cargo build --release

# プロファイル指定
cargo build --profile release-with-debug
```

`Cargo.toml`でプロファイル設定:

```toml
[profile.dev]
opt-level = 0
debug = true

[profile.release]
opt-level = 3
lto = true
codegen-units = 1
panic = "abort"
strip = true

[profile.release-with-debug]
inherits = "release"
debug = true
```

## 依存関係

```bash
# 依存関係の更新
cargo update

# 特定パッケージの更新
cargo update -p serde

# ロックファイルの確認
cargo verify-project

# 依存関係ツリー
cargo tree

# 重複依存の確認
cargo tree --duplicates
```

## ドキュメント

```bash
# ドキュメント生成
cargo doc

# ブラウザで開く
cargo doc --open

# プライベート項目も含む
cargo doc --document-private-items

# 依存関係なし
cargo doc --no-deps
```

## クリーン

```bash
# ビルド成果物を削除
cargo clean

# 特定パッケージのみ
cargo clean -p package-name

# リリースのみ
cargo clean --release
```

## ビルドスクリプト

`build.rs`:

```rust
fn main() {
    // 再コンパイル条件
    println!("cargo:rerun-if-changed=src/proto/");

    // 環境変数の設定
    println!("cargo:rustc-env=BUILD_TIME={}", chrono::Utc::now());

    // リンカフラグ
    println!("cargo:rustc-link-lib=ssl");
}
```
