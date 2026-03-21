---
layout: ../../layouts/MarkdownPageLayout.astro
title: "10. 型アサーションと as const"
---

# 10. 型アサーションと `as const`

## What

型アサーションは、開発者が「この値はこの型として扱える」とTypeScriptへ伝える書き方です。
`as` 演算子で記述します。

```ts
const value = "hello" as string;
```

`as const` は、値をできるだけリテラルとして固定するための特殊なアサーションです。

```ts
let status = "loading" as const; // 型は string ではなく "loading"
```

## Why

型推論だけでは意図を表現しきれない場面があります。
例えば以下のようなケースです。

```ts
const arr = ["a"];

function first<T>(arr: T[]): T | undefined {
  return arr[0];
}

const a = first(arr); // a は string | undefined になってしまう…
const str = a as string; // 確実に string 型なので強制する！
```

ただし、アサーションは「型チェックをすり抜ける」使い方もできるため、乱用は厳禁です。

## How

### 1. 最小限の型アサーションを使う

```ts
const maybeInput = document.getElementById("name") as HTMLInputElement | null;

if (maybeInput) {
  console.log(maybeInput.value);
}
```

`document.getElementById` は `HTMLElement | null` 型を返します。
しかし、取得できるはずの要素に合わせた型に強制すると便利です。

### 2. `as const` でオブジェクトを固定する

```ts
const lessons = {
  beginner: "TypeScript入門",
  advanced: "型システム応用",
} as const;
```

`as const` を使うと次の性質になります。

- プロパティが読み取り専用（`readonly`）
- 値が「Literal型」として保持される

### 3. 配列にも `as const` を使える

```ts
const levels = ["beginner", "intermediate", "advanced"] as const;
type Level = (typeof levels)[number];
```

この書き方で、配列の値からUnion型を安全に作れます。

## このページのまとめ

- 型アサーションは「推論を補助する」道具
- `as const` は値をリテラルとして固定するために有効
- まず型ガードを検討し、必要なときだけアサーションを使う
