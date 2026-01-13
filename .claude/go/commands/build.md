# ビルド

## 基本ビルド

```bash
# 全パッケージをビルド
go build ./...

# 特定パッケージをビルド
go build ./cmd/myapp

# 出力ファイル名を指定
go build -o bin/myapp ./cmd/myapp
```

## クロスコンパイル

```bash
# Linux向け
GOOS=linux GOARCH=amd64 go build -o bin/myapp-linux ./cmd/myapp

# Windows向け
GOOS=windows GOARCH=amd64 go build -o bin/myapp.exe ./cmd/myapp

# macOS ARM向け
GOOS=darwin GOARCH=arm64 go build -o bin/myapp-darwin ./cmd/myapp
```

## ビルドフラグ

```bash
# 最適化（デバッグ情報削除）
go build -ldflags="-s -w" ./cmd/myapp

# バージョン情報埋め込み
go build -ldflags="-X main.version=1.0.0" ./cmd/myapp

# レースコンディション検出付き
go build -race ./cmd/myapp
```

## 依存関係

```bash
# 依存関係の取得
go mod download

# 依存関係の整理
go mod tidy

# 依存関係の検証
go mod verify
```
