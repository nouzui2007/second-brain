# trait

## 抽象化

```scala
trait TraitA {
  def greet(): Unit
}
```

## 継承して実装

```scala
trait TraitB extends TraitA {
  def greet(): Unit = println("Good morning!")
}

trait TraitC extends TraitA {
  def greet(): Unit = println("Good evening!")
}
```

## 多重継承

多重継承ができる

同じメソッドが存在している時は、 `override` を使ってメソッドを指定する

新たな処理でも良いし、どちらかを指定するでも良い

```scala
class ClassA extends TraitB with TraitC {
  override def greet(): Unit = println("How are you?")
}

class ClassB extends TraitB with TraitC {
  override def greet() = super[TraitB].greet()
}

(new ClassA).greet()
(new ClassB).greet()
```

これはエラーになる

```scala
class ClassC extends TraitB with TraitC
```