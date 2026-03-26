# Agent Teams

複数のエージェントをチーム構成で並列に動かす機能。

## 有効化

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

## 使い方

自然言語でチーム化したいタスクを説明するだけ。設定ファイルの事前定義は不要。

```
3人のエージェントをスポーンして:
- 1人はセキュリティレビュー
- 1人はパフォーマンス確認
- 1人はテストカバレッジ検証
```

## Teammate Modes

```json
{
  "teammateMode": "in-process"
}
```

| モード | 説明 | 要件 |
|---|---|---|
| `in-process` | メインターミナル内、Shift+Down で切り替え | なし |
| `tmux` | 分割ペイン表示 | tmux or iTerm2 |
| `auto` | 自動選択 | なし |

## Sub-agents との違い

| 側面 | Sub-agents | Agent Teams |
|---|---|---|
| コンテキスト | 独立、結果だけ返す | 独立、直接メッセージング |
| 通信 | メインエージェントにのみ報告 | チームメイト同士が直接通信 |
| 最適用途 | フォーカスしたリサーチ | 相互協力が必要な複雑作業 |
| トークンコスト | 低い | 高い（各チームメイト独立インスタンス） |

## Sub-agents 定義（.claude/agents/）

```markdown
---
name: code-reviewer
description: Reviews code for quality
tools: Read, Glob, Grep
model: sonnet
---

You are a code reviewer...
```

### Frontmatter

| フィールド | 必須 | 説明 |
|---|---|---|
| `name` | Yes | 小文字とハイフンのみ |
| `description` | Yes | 委譲タイミングの判断基準 |
| `tools` | No | ツール allowlist |
| `model` | No | sonnet, opus, haiku, inherit |
| `permissionMode` | No | default, acceptEdits, plan, dontAsk |
| `maxTurns` | No | ターン上限 |
| `isolation` | No | worktree で git worktree に隔離 |

### スコープ階層

1. `--agents` CLI flag（最優先）
2. `.claude/agents/`（プロジェクト）
3. `~/.claude/agents/`（グローバル）
4. Plugin's `agents/`

## 学習ループでのチーム構成例

```
🎙️ インプット → Claude Code（リーダー）
  ├── 🔍 リサーチ — 最新動向をWebで調査
  ├── 📝 ナレッジ — 知識を体系化して書き出し
  ├── 🛠️ 実装 — コンポーネントの改善・追加
  ├── 💼 業務活用 — 本業にどう活かせるか分析
  └── ✍️ アウトプット — ブログ・ポートフォリオへの発信
```

リサーチ系は Sub-agents（軽量・読み取り専用）、書き込み系はリーダーが統合するのが実用的。
