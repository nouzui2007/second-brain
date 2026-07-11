# Case Class

## tupleで生成

case classのunapplyは文脈付きでtupleを返す

```scala
val a = (1, 2, 3)
println(a)

case class CaseSample(a: Int, b: Int, c: Int)

val b = CaseSample.tupled(a)
println(b)

// tupleにする
println(CaseSample.unapply(b).get)
```

tupleとcase classでパターンマッチして、フラットにしてみた

これは親子関係があるEntityクラスを作るとか、case classを合成するとかで使えないかなと思ってやってみた

合成はできるが、tupleを構成している要素を指定しないといけないので、共通部品にはできなさそう

```scala
val c = (a, b)
println(c)

val d = c match { case ((a, b, c), o) => (a, b, c, o)}
println(d)
```

## Case Class to Map

フィールドと値からMapオブジェクトを作成するコードを実装

### 関数として実装

```scala
def toMap(cc: AnyRef) =
  cc.getClass.getDeclaredFields.foldLeft(Map.empty[String, Any]) { (a, f) =>
    f.setAccessible(true)
    a + (f.getName -> f.get(cc))
  }

val f = toMap(b)
println(f)
```

### 抽象クラスにする

```scala
trait Base {
	def toMap =
	  this.getClass.getDeclaredFields.foldLeft(Map.empty[String, Any]) { (a, f) =>
	    f.setAccessible(true)
	    a + (f.getName -> f.get(this))
	  }
}

case class Maped(a: Int, b: String) extends Base
val g = Maped(1000, "hoge")
g.toMap
```

### Implicit Classにする

```scala
implicit class MappedClass[T](o: T) {
  def toMap =
  o.getClass.getDeclaredFields.foldLeft(Map.empty[String, Any]) { (a, f) =>
    f.setAccessible(true)
    a + (f.getName -> f.get(o))
  }
}

case class Maped(a: Int, b: String)
val h = Maped(1000, "hoge")
h.toMap
```

## Mapを使って、Case Class AからCase Class Bを作る

実現できているけど、最後のところで何をやっているか未解析

```scala
implicit class MappedClass[T](o: T) {
  def toMap =
  o.getClass.getDeclaredFields.foldLeft(Map.empty[String, Any]) { (a, f) =>
    f.setAccessible(true)
    a + (f.getName -> f.get(o))
  }
}

case class Maped(a: Int, b: String)

val g = Maped(1000, "hoge")
val gmap = g.toMap
println(gmap)

case class MappedPlus(a: Int, b: String, hoge: Maped)
val hmap = gmap + ("hoge" -> g)
println(hmap)

// 未解析
val h = MappedPlus.getClass.getMethods.find(_.getName == "apply").get.invoke(MappedPlus, hmap.values.toList.map(_.asInstanceOf[AnyRef]):_*).asInstanceOf[MappedPlus]
println(h)
```