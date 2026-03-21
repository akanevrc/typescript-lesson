---
layout: ../../layouts/MarkdownPageLayout.astro
title: "14. tsconfigの最小重要項目"
---

# 14. tsconfigの最小重要項目

## What

`tsconfig.json` は、TypeScriptコンパイラの挙動を決める設定ファイルです。
特に型チェックの厳しさを定義する項目が重要です。

理解しておくと便利な設定は次の3つです。

- `strict`
- `noImplicitAny`
- `strictNullChecks`

## Why

`tsconfig` の設定が曖昧だと、同じコードでもプロジェクトごとに品質が変わります。
最初に基準を揃えることで、チーム全体の開発体験が安定します。

- どこまで厳密に型チェックするかを統一できる
- 暗黙の `any` や `null` 関連バグを減らせる
- CIで同じ基準を機械的に担保できる

## How

### 1. `strict` を有効にする

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

`strict: true` は、複数の厳格設定をまとめて有効化します。
最初にこれをONにして、必要に応じて調整しましょう。

### 2. `noImplicitAny` を理解する

```json
{
  "compilerOptions": {
    "noImplicitAny": true
  }
}
```

型が推論できない箇所で暗黙に `any` になることを防ぎます。
型漏れを早めに見つけるのに効果的です。

### 3. `strictNullChecks` を理解する

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

`null` / `undefined` を明示的に扱う必要が出るため、
「存在しない値」へのアクセスミスを減らせます。
（上記の `strict` を `true` にすると、`strictNullChecks` も自動的に `true` になるはず）

### 4. 最小サンプル

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "strict": true,
    "noImplicitAny": true
  }
}
```

## このページのまとめ

- まずは `strict` / `noImplicitAny` / `strictNullChecks` を押さえる
- その後段階的に理解を進めましょう
