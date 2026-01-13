# テスト実行

## 基本コマンド

```bash
# 全テスト実行
cargo test

# 特定テスト
cargo test test_name

# 特定モジュール
cargo test module_name::

# 正規表現
cargo test 'test_user.*'
```

## 出力制御

```bash
# 標準出力を表示
cargo test -- --nocapture

# テスト並列度
cargo test -- --test-threads=1

# 成功したテストも表示
cargo test -- --show-output
```

## フィルタリング

```bash
# 無視されたテストを実行
cargo test -- --ignored

# すべてのテスト（無視含む）
cargo test -- --include-ignored

# 特定のテストを除外
cargo test -- --skip expensive_test
```

## 統合テスト

```bash
# 統合テストのみ
cargo test --test integration_test

# testsディレクトリすべて
cargo test --tests
```

## ドキュメントテスト

```bash
# ドキュメントテストのみ
cargo test --doc

# 特定クレートのドキュメントテスト
cargo test --doc -p my-crate
```

## ベンチマーク

```bash
# ベンチマーク実行（nightly）
cargo bench

# 特定ベンチマーク
cargo bench bench_name

# Criterionを使用（stable）
cargo bench --bench my_benchmark
```

`benches/my_benchmark.rs`:

```rust
use criterion::{criterion_group, criterion_main, Criterion};

fn benchmark(c: &mut Criterion) {
    c.bench_function("my_function", |b| {
        b.iter(|| my_function())
    });
}

criterion_group!(benches, benchmark);
criterion_main!(benches);
```

## カバレッジ

```bash
# cargo-llvm-cov
cargo install cargo-llvm-cov

# カバレッジ実行
cargo llvm-cov

# HTML出力
cargo llvm-cov --html

# lcov形式
cargo llvm-cov --lcov --output-path lcov.info
```

## テスト構造

```rust
// ユニットテスト（同一ファイル）
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_basic() {
        assert_eq!(add(2, 3), 5);
    }

    #[test]
    #[should_panic(expected = "division by zero")]
    fn test_panic() {
        divide(1, 0);
    }

    #[test]
    #[ignore]
    fn test_slow() {
        // 時間のかかるテスト
    }
}
```

## 非同期テスト

```rust
#[tokio::test]
async fn test_async() {
    let result = async_function().await;
    assert!(result.is_ok());
}

#[tokio::test(flavor = "multi_thread")]
async fn test_multi_thread() {
    // マルチスレッドランタイム
}
```

## プロパティベーステスト

```rust
use proptest::prelude::*;

proptest! {
    #[test]
    fn test_parse_roundtrip(s in "\\PC*") {
        let parsed = parse(&s);
        let formatted = format(&parsed);
        assert_eq!(s, formatted);
    }
}
```

## CI向けオプション

```bash
# 最初の失敗で停止
cargo test -- --fail-fast

# JSON出力
cargo test -- -Z unstable-options --format json

# タイムアウト設定（nextest）
cargo nextest run --timeout 60
```
