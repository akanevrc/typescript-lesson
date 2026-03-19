---
layout: ../../layouts/MarkdownPageLayout.astro
title: "2. 型注釈と型推論"
---

# 2. 型注釈と型推論

## What

TypeScriptで型を記述する方法は、大きく2つあります。

- 型注釈: 開発者が型を明示する
- 型推論: TypeScriptが文脈から型を推定する

### 型注釈

```ts
const name: string = "Akane"; // 明示的に string 型にする
```

### 型推論

```ts
const name = "Akane"; // この場合 "Akane" 型に推論される
```

## Why

型注釈で明示的に書くほうが、型が明確になります。
しかし、毎回書くのは面倒…。
その場合は、型注釈を省略して推論させましょう（推論できない場合もある）。

## How

### 1. 関数には注釈を付けると丁寧

特に引数の型注釈は省略できない場合がほとんどです。
（返り値の型は省略できる場合が多い）

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

この時点で、`greet(100)` のような誤った呼び出しはコンパイル時に検出できます。

### 2. ローカル変数は推論を活用可能

```ts
const userName = "Akane"; // "Akane"（string） と推論される
const message = greet(userName); // greet の戻り値は string と推論される
```

注釈なしでも十分に明確な場合は、推論に任せる方が読みやすいかもしれません。

### 3. 推論が弱くなる場面では注釈を追加する

```ts
const names: string[] = []; // [] だけでは string[] かどうか分からないので注釈をつける
names.push("Akane");
names.push(123); // number は追加できない
```

空配列のように初期値だけでは意図が伝わりにくいときは、注釈を付けます。

## このページのまとめ

- 型注釈は「契約」を明示するために使う
- 型推論は「記述量を減らす」ために使う
