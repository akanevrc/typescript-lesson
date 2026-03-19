---
layout: ../../layouts/MarkdownPageLayout.astro
title: "12. ReactでTypeScriptを使う基本"
---

# 12. ReactでTypeScriptを使う基本

## What

ReactでTypeScriptを使うとは、主に次の3つへ型を付けることです。

- Props（親から受け取る値）
- State（コンポーネント内部の状態）
- Event（クリックや入力イベント）

ReactのUIは状態とデータ受け渡しで成り立つため、ここに型を付けると安全性や記述性が上がります。

## Why

型を付けることで、次の問題を防げます。

- Propsの渡し間違い（必須漏れ・型違い）
- `useState` 等の型の不整合
- イベントから取り出す値の誤り

## How

### 1. Propsに型を付ける

```tsx
type UserCardProps = {
  name: string;
  age: number;
  isActive?: boolean;
};

function UserCard({ name, age, isActive = false }: UserCardProps) {
  return (
    <section>
      <h2>{name}</h2>
      <p>{age}歳</p>
      <p>{isActive ? "有効" : "無効"}</p>
    </section>
  );
}
```

`isActive?: boolean` のように `?` を付けると省略可能になります。

ちなみに、上記の引数の書き方を覚えているでしょうか？
構造分解（分割代入）という書き方です。
Reactではよく使います。

### 2. `useState` に型を付ける

```tsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState<number>(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

初期値から推論できる場合もありますが、Union型や `null` を扱うときは明示が有効です。
`null` を使う場合は以下のようになります。

```tsx
const [selectedId, setSelectedId] = useState<number | null>(null);
```

### 3. イベントに型を付ける

```tsx
import type { ChangeEvent } from "react";

function SearchBox() {
  const onChange = (event: ChangeEvent<HTMLInputElement>) => {
    console.log(event.target.value);
  };

  return <input type="text" onChange={onChange} />;
}
```

イベント型は `react` 側で用意されているものがあります。
イベント型を付けると `target` の扱いが安全になります。

## このページのまとめ

- React + TypeScriptの基本は Props / State / Event の型付け
- Props型を先に整えるとコンポーネント設計が安定する
- `useState` とイベント型を適切に明示するとバグを減らせる
