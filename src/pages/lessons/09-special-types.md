---
layout: ../../layouts/MarkdownPageLayout.astro
title: "9. unknown / any / never / void"
---

# 9. unknown / any / never / void

## What

このページでは、TypeScriptでよく出てくる特殊な4つの型を扱います。

- `unknown`: 型不明だが、安全に扱うには確認が必要な型
- `any`: 型チェックを無効化する型（推奨されないが時々便利です）
- `never`: 到達しない値・起こりえない値の型
- `void`: 戻り値を返さない関数の型

それぞれ意味がまったく違うため、使い分けが重要です。

## Why

これらの特殊な型にも、TypeScriptをやっているとよく遭遇します。
ここで意味の違いを理解しておきましょう。

## How

### 1. `unknown` は「使う前に確認する」

```ts
function printLength(value: unknown): void {
  if (typeof value === "string") {
    console.log(value.length);
    return;
  }
  console.log("stringではありません");
}
```

`unknown` は安全側の型です。
型を絞り込むまでは、プロパティアクセスやメソッド呼び出しができません。
下記の `any` は何でも呼び出せるので、その点が異なります。

### 2. `any` は最終手段にする

```ts
let value: any = "hello";
value = 123;
value.notExistingMethod(); // コンパイル時に検出されない
```

`any` は便利ですが、TypeScriptの恩恵を失います。
学習段階でも実務でも、基本は `unknown` を優先してください。
JavaScriptは全てが `any` 型のような世界でした。
今考えると恐ろしくありませんか？

### 3. `void` は「戻り値を使わない関数」

```ts
function logMessage(message: string): void {
  console.log(message);
}
```

`void` は「何も返さない」意図を明示するために使います。

### 4. `never` は「到達しない」

```ts
function throwError(message: string): never {
  throw new Error(message);
}
```

常に例外を投げる関数や、無限ループ関数などは `never` になります。
「呼び出したら最後、復帰しない」という意味です。

## このページのまとめ

- `unknown` は安全に不明値を扱うための型
- `any` は型チェックを無効化するため、最小限にする
- `void` は戻り値なし
- `never` は到達不能
