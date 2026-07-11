# Class

## Super Classのメソッド呼び出し

```scala
class FourLeggedAnimal {
    def walk { println("I'm walking") }
    def run { println("I'm running") }
}

class Dog extends FourLeggedAnimal {
    def walkThenRun {
        super.walk
        super.run
    }
}

val suka = new Dog
suka.walkThenRun
```