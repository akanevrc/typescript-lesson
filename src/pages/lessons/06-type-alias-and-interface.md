---
layout: ../../layouts/MarkdownPageLayout.astro
title: "6. 型エイリアスとinterface"
---

# 6. 型エイリアスとinterface

## What

- `type`: 型エイリアス。型に別名をつけるのに使います
- `interface`: オブジェクト型を新しく定義します

どちらも似ています。

```ts
type UserType = {
  id: number;
  name: string;
};

interface UserInterface {
  id: number;
  name: string;
}
```

## Why

型に名前をつける理由は、同じ形を複数箇所で安全に使うためです。

- 関数の引数・戻り値の型を統一できる
- ReactのProps型を複数ファイルで共有したりもできる

型をインラインで毎回書くより、意味のある名前に切り出した方が保守しやすくなります。
このあたりは、値を変数に入れておくことや、処理を関数にまとめておくことと似ています。

## How

### 1. 基本はどちらでも書ける

```ts
type Lesson1 = {
  title: string;
  level: "beginner" | "intermediate" | "advanced";
};

interface Lesson2 {
  title: string;
  level: "beginner" | "intermediate" | "advanced";
}
```

### 2. `interface` は拡張が自然

```ts
interface Person {
  name: string;
}

interface Student extends Person {
  studentId: string;
}
```

`extends` で継承すると、継承元のプロパティを引き継げます。

### 3. `type` はUnionやIntersectionに強い

```ts
type ApiStatus = "idle" | "loading" | "success" | "error";

type User = {
  id: number;
  name: string;
};

type UserWithMeta = User & { updatedAt: string };
```

Intersection（`&`）で結合すると、両方のプロパティを使える型になります。

## このページのまとめ

- `type` と `interface` はどちらも型の再利用手段
- 基本的にどっちを使ってもいいです（たぶん）
- 細かい違いはあります。知りたい場合は検索すると比較記事がいっぱい出てきます
