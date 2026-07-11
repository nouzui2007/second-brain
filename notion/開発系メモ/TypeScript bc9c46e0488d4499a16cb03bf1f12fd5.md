# TypeScript

## 基本の型

### Tuple

```jsx
let tval: [string, number]
tval = ["hello", 100]
console.log(tval)
```

分割代入できる。

```jsx
let input = [1, 2]
let [first, secont] = input
console.log(secont)
```

## 変数宣言

`let` は **レキシカルスコープ** または **ブロックスコープ** と呼ばれるものとなっていて、他の言語と同じように働く変数を宣言する。 `var` で発生していた奇妙なスコープの規則が働かないので、動作が予想しやすくなる。

- 例１ `if` ブロック内で宣言された変数が参照できる例

```jsx
function f(something: boolean) {
	if (something) {
		var x = 10
	}
}

// return 10
f(true)

// return undefined
f(false)
```

- 例２ 実行するタイミングをずらすと期待通りにならない例

```jsx
for (var i = 0; i < 10; i++) {
    setTimeout(function() {console.log(i); }, 100 * i);
}

// 10回 10 が表示される
// これは実行時に i が 10 になっているから
```

例２がわかりやすいので、これをletで書いて実行する。と、期待通りの結果になる。

## インターフェース

### 宣言

```jsx
interfase InterfaseName {
	requiredProperty: string
	optionalProperty?: boolean
}
```

### 利用

`interfase` を実装する宣言をしていなくても、必須のプロパティを持つオブジェクトであればOK。順序も気にしなくて良い。また、任意のプロパティは `?` を末尾につける。

```jsx
interface LabelValue {
  label: string
  color?: string
}

function printLabel(o: LabelValue) {
  console.log(o.label)
  if (o.color) {
    console.log(o.color)
  }
}

let o = {size: 10, label: "hogehoge"}
printLabel(o)

let o2 = {color: "red", label: "Wine"}
printLabel(o2)
```

## 参考サイト

[関数 | TypeScript 日本語ハンドブック | js STUDIO](https://js.studio-kingdom.com/typescript/handbook/functions)

[はじめに](https://book.yyts.org/)

[参考サイト](TypeScript/%E5%8F%82%E8%80%83%E3%82%B5%E3%82%A4%E3%83%88%2093c027a22f07484b9552d73000302e58.md)