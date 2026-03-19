---
layout: ../../layouts/MarkdownPageLayout.astro
title: "7. 関数型（引数・戻り値・コールバック）"
---

# 7. 関数型（引数・戻り値・コールバック）

## What

関数型は「どんな引数を受け取り、何を返すか」を定義する型です。

```ts
// `(a: number, b: number) => number` という型になる
function add(a: number, b: number): number {
  return a + b;
}
```

## Why

関数は、`export` で公開する場面が多くあります。
このときに、型が明示されていることで使用者側が迷いにくく、誤った使い方をしにくい、ということがあります。
引数は何個で、引数の型は何で、戻り値の型は何、ということが決まっていると、チームで開発する際に設計がしやすいのです。
難しめの言葉でいうと「契約」ともいいます。

## How

### 1. 引数と戻り値を明示する

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

### 2. オプショナル引数とデフォルト引数

```ts
function buildMessage(name: string, prefix = "Hi", suffix?: string): string {
  const base = `${prefix}, ${name}`;
  return suffix ? `${base} ${suffix}` : base;
}
```

- `suffix?: string` は省略可能（オプショナル引数）（省略すると`undefined`になります）
- `prefix = "Hi"` は未指定時に既定値が使われる（デフォルト引数）

### 3. 関数シグネチャで型を定義する

```ts
type Calculator = (a: number, b: number) => number;

function multiply(a: number, b: number): number {
  return a * b;
}

function someCalc(a: number, b: number, calc: Calculator): number {
  return calc(a, b); // calc は関数
}
```

アロー演算子で関数型を定義できます。
関数を渡す（コールバック等）とき等に使える書き方です。

別の例

```ts
type OnSuccess = (message: string) => void;

function runTask(onSuccess: OnSuccess): void {
  onSuccess("完了しました");
}
```

## このページのまとめ

- 関数型は呼び出し契約を表現する
- 引数・戻り値・コールバックを型で固定すると安全性が上がる
- 実務では関数シグネチャ（`(argType1, argType2, ...) => returnType`）が特に重要
