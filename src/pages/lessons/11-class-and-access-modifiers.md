---
layout: ../../layouts/MarkdownPageLayout.astro
title: "11. クラスとアクセス修飾子"
---

# 11. クラスとアクセス修飾子

ここでは説明が少なく分かりにくいかもしれませんが、JS/TSでは `class` は使用頻度低めです。
さらっと流してOKです（オブジェクト指向の詳細はJava等でやりましょう）。

## What

TypeScriptでは、JavaScriptの `class` 構文に型情報を加えて設計できます。
そのとき重要なのがアクセス修飾子です。

- `public`: どこからでもアクセス可能（デフォルト）
- `private`: クラス内部からのみアクセス可能
- `protected`: クラス内部と継承先からアクセス可能
- `readonly`: 初期化後に再代入できない

## Why

クラス設計でアクセス制御を行う理由は、状態の破壊を防ぐためです（カプセル化）。

- 外部から直接変更してほしくない値を守れる
- 変更経路を特定のメソッドに集約できる

## How

### 1. 基本的なクラス定義

```ts
class User {
  public name: string;
  private loginCount: number;

  constructor(name: string) {
    // コンストラクタを覚えていますか？
    this.name = name;
    this.loginCount = 0;
  }

  public login(): void {
    this.loginCount += 1;
  }

  public getLoginCount(): number {
    return this.loginCount;
  }
}
```

`loginCount` は `private` なので、クラス外からは直接変更できません。

### 2. `readonly` を使う

```ts
class Lesson {
  public readonly id: number;
  public title: string;

  constructor(id: number, title: string) {
    this.id = id;
    this.title = title;
  }
}
```

`id` は作成時に決まり、その後は変更不可になります。

### 3. `protected` と継承

```ts
class Animal {
  protected name: string;

  constructor(name: string) {
    this.name = name;
  }
}

class Dog extends Animal {
  bark(): string {
    return `${this.name} が鳴いたよ`; // 継承先ではアクセス可能
  }
}
```

`protected` は「外部からは隠すが、継承先には公開する」範囲です。

## このページのまとめ

- クラスはデータと振る舞いをまとめる手段
- アクセス修飾子で変更可能範囲を制御できる
- `private` / `protected` / `readonly` でカプセル化すると安全な設計に近づく
