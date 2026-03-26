# Memory Server

セッション横断の長期記憶MCPサーバー。

## 概要

学び・判断・経験を蓄積し、過去の記憶を検索できる。SQLite 1ファイルで外部依存ゼロ。

## 設定

```json
{
  "mcpServers": {
    "memory": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/path/to/mcp/memory",
        "python",
        "memory_server.py"
      ]
    }
  }
}
```

## ツール

| ツール | 用途 |
|---|---|
| `remember` | 記憶を保存（content, category, tags） |
| `recall` | 関連する記憶を検索（FTS5 trigram + ベクトル検索 + 時間減衰） |
| `forget` | 記憶を削除（ID指定 or 経過日数で一括） |
| `memory_stats` | 統計情報 |

## カテゴリ例

- `learning` — 学んだこと
- `decision` — 判断・設計決定
- `experience` — 経験・実践結果
- `insight` — 気づき・洞察

## 技術構成

- FastMCP
- SQLite + FTS5 trigram（日本語対応）
- ONNX Runtime + MiniLM（ベクトル検索、オプション）
- 時間減衰（半減期14日、指数減衰）
