# TypeScript開発ガイド

Google TypeScript Style Guideに基づくTypeScript開発ガイドライン。

## ディレクトリ構成

```
typescript/
├── style/               # スタイル（書き方）
│   ├── naming.md            # 命名規則
│   └── formatting.md        # フォーマット
│
├── guidelines/          # ガイドライン（コアルール）
│   ├── variables.md         # 変数
│   ├── functions.md         # 関数
│   ├── classes.md           # クラス
│   ├── types.md             # 型システム
│   ├── control-flow.md      # 制御フロー
│   ├── declarations.md      # 型定義ファイル (.d.ts)
│   └── prohibited.md        # 禁止事項
│
├── linting/             # リンティング
│   └── setup.md             # ESLint/Prettier設定
│
└── commands/            # 開発コマンド
    ├── build.md             # ビルド
    └── test.md              # テスト実行
```

## 言語共通アーキテクチャ

[.claude/architecture/](../architecture/) を参照:
- ヘキサゴナル/クリーンアーキテクチャ
- ドメイン駆動設計 (DDD)
- イベント駆動アーキテクチャ
- モノレポ構成

## 参考

- [Google TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)
- [Basarat's TypeScript Style Guide](https://basarat.gitbook.io/typescript/styleguide)
- [TypeScript Declaration Files Do's and Don'ts](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)
