---
layout: ../../layouts/MarkdownPageLayout.astro
title: "8. Genericsの基本"
---

# 8. Genericsの基本

## What

Generics（ジェネリクス）は、型を引数として扱う仕組みです。
「処理のロジックは共通だが、扱う型だけを差し替えたい」ときに使います。

```ts
function identity<T>(value: T): T {
  return value;
}
```

`T` は「型変数」で、呼び出し時に具体的な型が決まります。

## Why

ジェネリクスを使う理由は、再利用性と型安全を両立するためです。
例えば、`any` を使わずに汎用関数を作ることができます（型情報が失われない）。

## How

### 1. ジェネリック関数を作る

```ts
function firstItem<T>(items: T[]): T | undefined {
  return items[0]; // items は T[]
}

const firstNumber = firstItem([10, 20, 30]); // number | undefined 型が推論される！
const firstText = firstItem(["a", "b", "c"]); // string | undefined 型が推論される！
```

型変数 `T` が自動推論されます。
`firstNumber` の例は `T = number`、`firstText` の例は `T = string` です。
`T` が決まれば、戻り値の型も決定します（重要）。
そのため、変数の型まで推論されます。

### 2. 複数の型引数を使う

```ts
function pair<K, V>(key: K, value: V): { key: K; value: V } {
  return { key, value };
}

const item = pair("id", 1001); // { key: string; value: number } 型が推論される！
```

### 3. ジェネリック型（type）を定義する

```ts
type ApiResponse<T> = {
  data: T;
  message: string;
};

type User = {
  id: number;
  name: string;
};

const userResponse: ApiResponse<User> = {
  data: { id: 1, name: "Akane" }, // ここが User 型になる
  message: "ok",
};
```

この例では、APIレスポンスが共通構造なので、再利用して User 型に特化させています。

### 4. 制約（extends）を付ける

```ts
function printLength<T extends { length: number }>(value: T): void {
  console.log(value.length); // length が使える
}
```

`extends` で「この性質を持つ型だけ受け取る」と制約できます。

## このページのまとめ

- ジェネリクスは型を引数として扱う仕組みです
- 再利用性と型安全を両立したい場面で有効です
- まずは「ジェネリック関数」と「ジェネリック型」を使えるようになると役立ちます
