---
layout: ../../layouts/MarkdownPageLayout.astro
title: "5. Narrowing（型の絞り込み）"
---

# 5. Narrowing（型の絞り込み）

## What

Narrowing（ナローイング）は、条件分岐によって型をより具体的にする仕組みです。
Union型を使うときに必須の考え方です。

たとえば `string | number` は、そのままでは両方の操作を安全に行えません。
`typeof` などで分岐し、型を絞ってから処理します。
`if` などの分岐を書くことで、型が強制されるのです。

## Why

Union型は便利ですが、型の範囲を大きくしてしまっているため、安全性が落ちます。
それを再び狭い型に落とし込むことができるのがNarrowingです。

## How

### 1. `typeof` で絞り込む

`string | number` のようなLiteralではない型は、`typeof` で実行時の型を取得しましょう。

```ts
function formatValue(value: string | number): string {
  if (typeof value === "string") {
    // この箇所では、value は自動的に string 型に強制されている！
    return value.toUpperCase(); // "akane" -> "AKANE"
  }
  // この箇所では、value は自動的に number 型に強制されている！
  return value.toFixed(2); // 12.3 -> "12.30"
}
```

重要なのは、`if` ブロック内では `value` は `string` 型に強制され、
`if` を抜けた箇所では `number` 型として扱われることです。
これがNarrowingです。
エディタでマウスを当てて確認してみると、コードの場所によって型が違っています！

### 2. `in` でオブジェクトの形を判定する

`in` 演算子は、オブジェクトがそのプロパティキーを持っているかどうかを`true`/`false`で返します。

```ts
type Bird = { fly: () => void };
type Fish = { swim: () => void };

function move(animal: Bird | Fish): void {
  if ("fly" in animal) {
    // この箇所では、animal は自動的に Bird 型に強制されている！
    animal.fly();
    return;
  }
  // この箇所では、animal は自動的に Fish 型に強制されている！
  animal.swim();
}
```

`in` によってメンバーをしらべることでもNarrowingが発動します。

### 3. 判別可能Union（discriminated union）を使う

```ts
type FetchState =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: string[] }
  | { status: "error"; message: string };

function renderMessage(state: FetchState): string {
  switch (state.status) {
    case "idle":
      return "開始待ち";
    case "loading":
      return "読み込み中...";
    case "success":
      return `件数: ${state.data.length}`; // data プロパティが使える！
    case "error":
      return `エラー: ${state.message}`; // message プロパティが使える！
  }
}
```

`state.status` で分岐することでNarrowingが発動しています。
重要なのは、`success`/`error` のときに `data`/`message` が使えるということです（エディタの補完が効く！）。
`status` の値で型が確定するため、Reactの状態管理でも使いやすいパターンです。

### 4. null / undefined の判別（最重要）

```ts
type Message = { content: string };

function logMessage(message: Message | null): void {
  if (message === null) return; // ガード節で null をはじく

  // ここでは message の型が Message 型に強制されている！（null でないことが保証される）
  console.log(message.content);
}
```

上記の関数は、`message` が `null` のときは何もせず、`message` が存在するときのみ `console.log` しています。
ガード節を通り抜けた位置では、`message` 引数の型自体が `null` 型でなくなっています。
エディタでマウスを当てて確認してみましょう。

## このページのまとめ

- NarrowingはUnion型を安全に使うために重要
- `typeof`, `in`, 判別可能Unionをまず押さえる
- Reactの分岐表示やAPIレスポンス処理で特に効果が高い
