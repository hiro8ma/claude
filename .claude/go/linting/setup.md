# リンティング設定

## golangci-lint

推奨のリントランナー。複数のリンターを統合。

### インストール

```bash
# macOS
brew install golangci-lint

# Go
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

### 実行

```bash
# 基本実行
golangci-lint run

# 特定ディレクトリ
golangci-lint run ./pkg/...

# 新規コードのみ（CI用）
golangci-lint run --new-from-rev=HEAD~1

# 自動修正
golangci-lint run --fix
```

## 推奨リンター

| リンター | 説明 |
|---------|------|
| errcheck | エラーチェック漏れ検出 |
| goimports | import文の整理・整形 |
| revive | golint後継、スタイルチェック |
| govet | 静的解析 |
| staticcheck | 高度な静的解析 |
| gosimple | コード簡略化の提案 |
| ineffassign | 無効な代入の検出 |
| unused | 未使用コードの検出 |

## 設定ファイル

`.golangci.yml`:

```yaml
run:
  timeout: 5m

linters:
  enable:
    - errcheck
    - goimports
    - revive
    - govet
    - staticcheck
    - gosimple
    - ineffassign
    - unused

linters-settings:
  goimports:
    local-prefixes: github.com/yourorg/yourrepo
  revive:
    rules:
      - name: blank-imports
      - name: context-as-argument
      - name: error-return
      - name: error-strings
      - name: exported

issues:
  exclude-use-default: false
  max-issues-per-linter: 0
  max-same-issues: 0
```

## 個別ツール

```bash
# go vet - 組み込み静的解析
go vet ./...

# goimports - 整形 + import整理
goimports -w .

# gofmt - 整形のみ
gofmt -w .
```

## CI統合

### GitHub Actions

```yaml
- name: golangci-lint
  uses: golangci/golangci-lint-action@v3
  with:
    version: latest
```

### 出力フォーマット

```bash
# GitHub Actions用
golangci-lint run --out-format=github-actions

# JSON出力
golangci-lint run --out-format=json
```

## エディタ設定

### VS Code

```json
{
  "go.lintTool": "golangci-lint",
  "go.lintFlags": ["--fast"]
}
```

### 保存時の自動整形

```json
{
  "editor.formatOnSave": true,
  "[go]": {
    "editor.defaultFormatter": "golang.go"
  }
}
```
