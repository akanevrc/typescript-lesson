---
layout: ../../layouts/MarkdownPageLayout.astro
title: "13. APIデータを安全に扱う"
---

# 13. APIデータを安全に扱う

## What

APIから返るデータは「外部入力」です。
TypeScriptの型だけでは、実行時に本当にその形かどうかは保証されません。

そのため、次の流れで扱うのが基本です。

1. 受け取り直後は `unknown` として扱う
2. 型ガードでデータ形状を検証する
3. 検証後に安全に利用する

## Why

APIデータは壊れやすい境界です。

- サーバー側仕様変更
- 一時的な異常レスポンス
- 想定外の `null` や欠損値

型定義だけを信じると、実行時エラーが発生します。
実務では「型定義 + 検証」のセットで守るのが安全です。

## How

### 1. 受け取り時は `unknown`

```ts
type User = {
  id: number;
  name: string;
};

async function fetchRaw(): Promise<unknown> {
  const response = await fetch("/api/user");
  return response.json();
}
```

### 2. 型ガードを作る

```ts
function isUser(value: unknown): value is User {
  if (typeof value !== "object" || value === null) return false;
  const maybeUser = value as Record<string, unknown>;
  return typeof maybeUser.id === "number" && typeof maybeUser.name === "string";
}
```

戻り値 `value is User` になっています。
これは、「`value` が `User` 型であるかどうか」を示す `boolean` であることを表しています。
値としてはただの `boolean` ですが、型システムがこの関数を実行し、Narrowing等に利用できるようにします。

### 3. 検証後だけ利用する

```ts
async function fetchUser(): Promise<User> {
  const raw = await fetchRaw();
  if (!isUser(raw)) {
    // ここでNarrowing
    throw new Error("ユーザーデータの形式が不正です");
  }
  return raw;
}
```

この構成なら、呼び出し側は `User` として安全に扱えます。

### 4. 配列レスポンスの検証例

```ts
function isUserArray(value: unknown): value is User[] {
  return Array.isArray(value) && value.every(isUser);
}
```

これで `value is User[]` 型ガードが作れます。

## このページのまとめ

- APIデータはまず `unknown` として受ける
- 型ガードで検証してから業務ロジックへ渡す
- 「外部入力は疑う」が基本姿勢
