# ビルドコマンド

## TypeScriptコンパイル

```bash
# コンパイル
tsc

# ウォッチモード
tsc --watch

# 型チェックのみ（出力なし）
tsc --noEmit

# 特定の設定ファイルを使用
tsc --project tsconfig.build.json
```

## Bundler

### esbuild

```bash
# インストール
npm install --save-dev esbuild

# ビルド
esbuild src/index.ts --bundle --outfile=dist/bundle.js

# 本番ビルド
esbuild src/index.ts --bundle --minify --outfile=dist/bundle.min.js
```

### Vite

```bash
# 開発サーバー
vite

# 本番ビルド
vite build

# プレビュー
vite preview
```

### webpack

```bash
# 開発ビルド
webpack --mode development

# 本番ビルド
webpack --mode production

# ウォッチモード
webpack --watch
```

## package.json スクリプト例

```json
{
  "scripts": {
    "build": "tsc",
    "build:watch": "tsc --watch",
    "build:prod": "tsc && esbuild dist/index.js --bundle --minify --outfile=dist/bundle.min.js",
    "typecheck": "tsc --noEmit",
    "clean": "rm -rf dist"
  }
}
```

## 出力設定 (tsconfig.json)

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```
