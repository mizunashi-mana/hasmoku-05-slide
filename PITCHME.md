# Haskell勉強会 ダイジェスト

---

## Haskell勉強会開催中

* 場所: 電気通信大学(東京都調布市)
* 時間: 毎週土曜日14:00 ~
* 僕に声をかけてもらえば、詳細を伝えます

+++

## 現在までの流れ

1. Haskellの型クラスについて
    * 型クラスの三つの見方を紹介
    * それに付随する二つのパッケージを紹介
2. kan-extensionsパッケージの紹介(イマココ)
    * 圏論の基礎知識と、
    * それをHaskellにどう翻訳するかを紹介
    * adjunctions -> kan-extensions
3. 未定
    * extensible-effectとか、
    * levity polymorphismとかやりたい

---

## Haskellの型クラスの三つの見方

* 型クラスとは、ただの型のクラスだよ。何か問題でも？
* 型クラスとは、ただの型制約コンストラクタだよ。何か問題でも？
* 型クラスとは、ただの暗黙的引数だよ。何か問題でも？

---

## 型クラスは、型のクラス

+++

### クラスとは？

ある性質を満たすような、ものの集まり。

* 性質P: P(n) <=> nは自然数
* Pのクラス: N = {0, 1, 2, ...}

+++

### 型クラスSemigroupの性質P

```
class Semigroup (a :: *) where
    -- Should be satisfy: x <> y <> z == x <> (y <> z)
    (<>) :: a -> a -> a
```

* P(a, <>) <=> 任意の型`a`を持つ値`x,y,z`について、`x <> y <> z == x <> (y <> z)`を満たす

+++

### 型クラスSemigroupの性質P'

* Haskellの型クラスは、通常決定的 <=> 型`a`に対して、`(<>)` = f(a)となる関数が決まる

* P'(a) = P(a, f(a))

+++

### 型クラスは、型のクラスだよ

* 型クラスの定義: 性質定義
    * `class A a where ...`: A(a) = A'(a, f1(a), f2(a), ...)という性質を定義
* インスタンスの定義: 性質の証明
    * 型が性質Aを満たすことを、証明する(Haskellに伝える)

---

## 型クラスは、型制約コンストラクタ

+++

### 型制約とは？

型制約種`Constraint`を持つようなもの

* `Monad Maybe :: Constraint`
* `Monoid () :: Constraint`
* `Int ~ Int :: Constraint`

+++

### 型クラスの種

```
>>> class A a
>>> :kind A
A :: * -> Constraint
```

型クラスは、型を受け取り、型制約種を持つもの(型制約)を返す型関数

+++

### 型クラスは、型制約コンストラクタだよ

* データ宣言は、型コンストラクタを作る:
```
>>> data DataA a
>>> :kind DataA
DataA :: * -> *
```

* クラス宣言は、型制約コンストラクタを作る:
```
>>> class ClassA a
>>> :kind ClassA
ClassA :: * -> Constraint
```

---

## 型クラスは、暗黙的引数

+++

### 型クラスは、辞書

```
class A a where
    aMethod :: a -> a

f :: A a => a -> a
f x = aMethod x
```

上を、辞書に変換:

```
data A a = A
  { aMethod :: a -> a
  }

f :: A a -> a -> a
f d x = aMethod d x
```

+++

### 型クラスは、暗黙的引数だよ

```
-- f d x = "f: " ++ show d x
f x = "f: " ++ show x

-- g = f bool_ShowDict True
g = f True
```

* 暗黙的に、以下のような引数があるよ！
    * 型制約がある場所は、単に辞書が渡っていく
    * 具体的な型がある場所は、その型の辞書が一緒に適用される

---

## 型クラスの三つの見方

* 他に、constraintsとreflectionという、パッケージの仕組みを紹介した
    * 一般的な型クラスは、型のクラスの見方
    * constraintsは、型制約コンストラクタの見方に関係
    * reflectionは、暗黙的引数の見方に関係

それ以外にも、見方は色々考えられると思うので、ぜひ考えてみてください

+++

### Haskell勉強会は、参加者募集中です
