---
layout: ../../layouts/MarkdownPageLayout.astro
title: "3. 基本の型"
---

# 3. 基本の型（primitive / array / object）

## What

TypeScriptの基礎は「データの形を定義すること」です。
最初に押さえる型は次のとおりです。

- `string`（文字列）
- `number`（数値）
- `boolean`（真偽値）
- 配列（`T[]`）
- オブジェクト型（`{ ... }`）
- `null`/`undefined`

## Why

上記の基本的な型は特に重要です。
後々出てくる `interface` や `Union` も基本的な型の組み合わせだからです。

これらを明示する（または推論される）ことで、変数や関数の値の種類が明確になります。
値の種類が明確になると、間違った処理方法を検出できるようになります。

## How

### 1. primitive型を使う

```ts
const lessonTitle: string = "TypeScript Lesson";
const pageCount: number = 3;
const isPublished: boolean = true;
```

### 2. 配列型を使う

```ts
const tags: string[] = ["typescript", "beginner", "react"];
const userIds: number[] = [1, 2, 3, 4, 5];
```

`string[]` は「文字列だけを入れられる配列」を意味します。
`number[]` は「数値だけを入れられる配列」です。

JavaScriptでも、`string` と `number` が混ざった配列はそうそう使わなかったでしょう。
しかし、本当にそうなのか保証はありませんでした。
TypeScriptでは、単一の型しか含まれないことを保証できるのです。

### 3. オブジェクト型を定義する

```ts
// こっちは型（実行時は存在しない）
type User = {
  id: number;
  name: string;
  isActive: boolean;
  tags: string[];
};

// こっちは値（実行時に存在する）
const user: User = {
  id: 1,
  name: "Akane",
  isActive: true,
  tags: ["beginner", "typescript"],
};
```

`user` 変数は `User` 型だから、`id` という `number` 型のフィールドを持っている、と把握できます（開発者だけでなくエディタなどのツールも把握してくれます）。

### 4. 関数でオブジェクト型を使う

```ts
type Lesson = {
  title: string;
  difficulty: "beginner" | "intermediate"; // Union 型
};

function printLesson(lesson: Lesson): void {
  console.log(`${lesson.title} (${lesson.difficulty})`);
}
```

これにより、関数へ渡すデータ構造を統一できます。
上記の `printLesson` は `Lesson` 型のデータしか受け取れません。

## type を使おう

- 「データの構造に名前を付ける」ために `type` を使う
- 迷ったら、よく使うオブジェクトから型を定義してみましょう
- `type` で型を定義したら、型注釈に使ってみましょう

## このページのまとめ

- TypeScriptの基本は「値」ではなく「データの形」を意識すること
- primitive、配列、オブジェクト型を扱えると多くのコードが書ける
- `type` を使って型に名前を付けると読みやすさと安全性が上がる
