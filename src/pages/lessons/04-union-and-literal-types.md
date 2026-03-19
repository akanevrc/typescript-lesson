---
layout: ../../layouts/MarkdownPageLayout.astro
title: "4. Union型とLiteral型"
---

# 4. Union型とLiteral型

## What

Union型は「複数の型のどれか」を表す型です。
`|`（パイプ）でつなげて書きます。

```ts
let id: string | number;
id = "user-1"; // 文字列を入れられる
id = 1001; // 数値も入れられる
```

Literal型は「値そのもの」を型として扱うものです。

```ts
type Theme = "light" | "dark";
let theme: Theme = "light";
theme = "dark";
theme = "rainbow"; // これは入れられない！
```

Literal型とUnion型を使用すると、「この値しか持たない」という保証ができます。

## Why

実務では「取りうる値が限定される」ケースが多くあります。

- ステータス: `"idle" | "loading" | "success" | "error"`
- 権限: `"admin" | "editor" | "viewer"`
- APIモード: `"json" | "text"`

`string` だけで表すと自由すぎて、タイプミスなどを防げません。
Literal型で候補を限定すれば、誤りをコンパイル時に検出できます。

```ts
type Status = "idle" | "loading" | "success" | "error";

const status: Status = "loading";
const badStatus: Status = "lodading"; // スペルミスを検出できる！
```

## How

### 1. まずは Union型を使う

```ts
function printId(id: string | number): void {
  console.log(id);
}
```

これで `printId("abc")` も `printId(123)` も受け取れます。

### 2. 候補値を Literal型で制限する

```ts
type Difficulty = "beginner" | "intermediate" | "advanced";

type Lesson = {
  title: string;
  difficulty: Difficulty; // ここが上記の型
};
```

### 3. オブジェクトの状態設計に使う

```ts
type FetchState =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: string[] }
  | { status: "error"; message: string };
```

これは、「`idle`/`loading`のときは`status`だけ」だけど、「`success`は`data`も持っている」「`error`は`message`も持っている」という型を定義しています！
この設計にしておくと、次の `05. Narrowing` で安全に分岐できます（めちゃくちゃすごい技）。

## よくある注意点

- `string | number` は「どちらか一方」であり「両方」ではない（両方って何？）
- Literal型を使わないと、候補の制約が効かない
- 複雑な型は `type` で名前を付けて管理する

## このページのまとめ

- Union型は「複数候補の型」を表す
- Literal型は「値を型として固定」する
- 状態・モード・権限など、選択肢があるデータの表現に非常に有効
