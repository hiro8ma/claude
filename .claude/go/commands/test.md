# テスト実行

## 基本コマンド

```bash
# 全テスト実行
go test ./...

# 詳細出力
go test -v ./...

# 特定パッケージのテスト
go test ./pkg/user

# 特定テスト実行
go test -run TestUserCreate ./pkg/user

# 正規表現でテスト指定
go test -run "TestUser.*" ./pkg/user
```

## サブテスト実行

```bash
# 特定サブテスト
go test -run TestUser/Create ./pkg/user

# サブテストを正規表現で
go test -run "TestUser/Create.*" ./pkg/user
```

## レースコンディション検出

```bash
# レース検出付きテスト
go test -race ./...
```

## カバレッジ

```bash
# カバレッジ表示
go test -cover ./...

# カバレッジファイル出力
go test -coverprofile=coverage.out ./...

# HTML形式で表示
go tool cover -html=coverage.out

# 関数別カバレッジ
go tool cover -func=coverage.out
```

## ベンチマーク

```bash
# ベンチマーク実行
go test -bench=. ./...

# 特定ベンチマーク
go test -bench=BenchmarkProcess ./pkg/worker

# メモリ統計付き
go test -bench=. -benchmem ./...

# 複数回実行
go test -bench=. -count=5 ./...
```

## その他オプション

```bash
# タイムアウト指定
go test -timeout 30s ./...

# 並列度指定
go test -parallel 4 ./...

# キャッシュ無効化
go test -count=1 ./...

# ショートモード（長いテストをスキップ）
go test -short ./...
```
