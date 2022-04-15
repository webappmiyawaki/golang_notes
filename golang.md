go
    1.8からモジュール管理システム導入？

環境設定：win
    1. golangインストール：https://go.dev/dl/
    2. gitインストール（省略可）：https://git-scm.com/
    3. dockerインストール（省略可）：
    4. godoc（ドキュメント）インストール＜＜無くてもよい
       1. gopath（1.8以降だとgoフォルダがないので自分で作成）C:\Users\miyaw\go>
        ターミナルで実行：go install golang.org/x/tools/cmd/godoc@latest
        godocコマンド、もしくはgodoc fmtでなんか表示
        直アドレス：`http://localhost:6060/pkg/`
    5. 発生するエラー：
        windowsの判定エラー？公式未解決
        
        > このファイルのバージョンは 現在の実行中の Windows のバージョンとは
        互換性がありません コンピュータのシステム情報を確認して x86 (32 ビット)
         または x64 (64 ビット) のどちらのバージョンのプログラムが必要であるかを
         確認してからソフトウェアの発行者に問い合わせてください ...
        
定義
```go
()	丸括弧(まる – )　round brackets
[]	角括弧(かく – )　square bracket
{}　波括弧(なみ – )　curly bracket
<>	山括弧(やま – )　angle bracket

```


### *hello world*

```go
package main		// mainパッケージであることを宣言
import "fmt"		// fmtモジュールをインポート
func main() {		// 最初に実行されるmain()関数を定義
    fmt.Println("hello, world")
}
```

### *import*

```go
//単体
import "fmt"

//複数
import (
    "os"
    "fmt"
)
```

環境変数 GOPATH 配下に外部のパッケージをインストールすることができます


### *package*
自作パッケージの例を下記に示します。
```go
$HOME
 └ go
   └ src
     ├ sample.go
     └ local
       └ mypkg
         └ mypkg.go
```

```go
package mypkg
import "fmt"
func FuncA() {			// 大文字で始まるものは自動的にエクスポートされる
    fmt.Println("FuncA()")
}

func funcB() {			// 小文字で始まるものはエクスポートされない
    fmt.Println("funcB()")
}
```

sample.go ファイルを次の内容で作成します。大文字で始まる FunxA() は公開されているので使用できますが、小文字で始まる funcB() は非公開なので使用することができません。

```go
package main
import "local/mypkg"
mypkg.FuncA()		// 呼び出せる
mypkt.funcB()		// Error
```


### *変数（var）*

```go
var a1 int

//初期値を省略すると 0 や空文字列 "" などで初期化されます。
var a1 int = 123
var a2 = 123

//初期値を指定する場合は、:= を用いると var も省略することができます。
a3 := 123

var(
    a1 int = 123
    a2 int = 456
)

name, age = "Yamada", 26
```

### *定数（const）*

```go
const foo = 100
const (
    foo = 100
    baa = 200
)
```

### *型（数値、文字列、論理値、インターフェース）*

```go
bool				真偽値(true or false)
int8/int16/int32/int64		nビット整数
uint8/uint16/uint32/uint64	nビット非負整数
float32/float64		nビット浮動小数点数
complex64/complex128		nビット虚数
byte				1バイトデータ(uint8と同義)
rune				1文字(int32と同義)
uint				uint32 または uint64
int				int32 または int64
uintptr				ポインタを表現するのに十分な非負整数
string				文字列

//interface型がある<<演算対象にならない（初期値nil）
var x interface()
fmt.Println(x)
```


### *型変換*

```go
var i int64 = 64
fmt.Printf("%T\n", i) //変数の型を表示させる

var a1 int16 = 1234
var a2 uint32 = uint32(a1)

//string型からint型へ
var s string ="100"
fmt.Printf("s=%T\n", s)

//int型からstring型へ
i, _ := strconv.Atoi(s)//strconvは2つ戻り値があるが「_」で受け取っている。（破棄されるということ）＞＞goは変数は全部利用しないといけないので破棄する必要がある
fmt.Println(i)
fmt.Printf("i = $T\n", i)

```

### *演算子*

```go
x + y		加算 (文字列の連結にも利用)
x - y		減算
x * y		乗算
x / y		除算
x % y		除算の余り
x & y		論理積(AND)
x | y		論理和(OR)
x ^ y		排他的論理和(XOR)
x &^ y		x AND (NOT y)
x << y		yビット左にシフト
x >> y		yビット右にシフト
x = y		xにyを代入
x := y		xにyを代入(初期化の使用可能)
x++		x = x + 1 と同義
x--		x = x - 1 と同義
x += y		x = x + y と同義
x -= y		x = x - y と同義
x *= y		x = x * y と同義
x /= y		x = x / y と同義
x %= y		x = x % y と同義
x &= y		x = x & y と同義
x |= y		x = x | y と同義
x ^= y		x = x ^ y と同義
x &^= y		x = x &^ y と同義
x <<= y		x = x << y と同義
x >>= y		x = x >> y と同義
x && y		xかつy(AND)
x || y		xまたはy(OR)
!x		xがtrueの場合false/falseの場合true(NOT)
x == y		xとyが等しければ
x != y		xとyが等しくなければ
x < y		yがxより大きければ
x <= y		yがx以上であれば
x > y		yがxより小さければ
x >= y		yがx以下であれば
ch <- x		chチャネルにxを送信
x =<- ch	chチャネルからxに受信
```

### *配列*
コンパイル時に個数が決まっていて変更不可のものを 配列 と言います。型名の前に [個数] をつけて宣言します。配列のインデックスは 0 からはじまります。途中で個数を変更することはできませんが、メモリ効率や性能の点で優れています。

```go
a1 := [3]string{}
a1[0] = "Red"
a1[1] = "Green"
a1[2] = "Blue"
fmt.Println(a1[0], a1[1], a1[2])

a2 := [3]string{"Red", "Green", "Blue}

a3 := [...]string{"Red", "Green", "Blue"}
```

### *スライス（make,cap）*
メモリ効率や速度は若干落ちますが、個数を変更可能なものを スライス と呼びます。型名の前に [] をつけて宣言します。スライスには append() を用いて要素を追加します。

```go
a1 := []string{}			// スライス。個数不定
a1 = append(a1, "Red")
a1 = append(a1, "Green")
a1 = append(a1, "Blue")
fmt.Println(a1[0], a1[1], a1[2])
```
`len()` は配列やスライスの長さ(length)、
`cap()` は容量(capacity)を求めます。
長さは実際に使用されている数、容量はメモリ上に確保されている数です。
容量を超えると、倍の容量のメモリが別に確保され、既存データがコピーされます。
（メモリ計算）

```go
a := []int{}
for i := 0; i < 10; i++ {
    a = append(a, i)
    fmt.Println(len(a), cap(a))
}
```

スライスの場合、make(スライス型, 初期個数, 初期容量) を用いたメモリの確保ができます。初期容量を省略した場合は初期個数と同じ容量が確保されます。容量をあらかじめ確保しておくことで、容量超過時の再確保を減らして速度を速めることができます。

```go
bufa := make([]byte, 0, 1024)
```

### *map*

```go
// マップを定義する
a1 := map[string]int{
    "x": 100,
    "y": 200,			// 改行する場合はカンマ必須
}

// マップを参照する
fmt.Println(a1["x"])

// マップに要素を加える
a1["z"] = 300

// マップの要素を削除する
delete(a1, "z")

// マップの長さを求める
fmt.Println(len(a1))

// マップに要素が存在するかどうかを調べる
_, ok := a1["z"]
if ok {
    fmt.Println("Exist")
} else {
    fmt.Println("Not exist")
}

// マップをループ処理する
for key, value := range a1 {
    fmt.Printf("%s=%d\n", key, value)
}
```


- バイト
- 関数


戻り値に命名する
```go
package main
import (
    "fmt"
)
func ret()(i int, t string){
    // 関数の戻り値にi,tと名前をつける
    i = 10
    t = "test"

    // returnには何を返すか明記しない
    return
}

func main(){   
    // iとtがreturnされる
    i, t := ret()
    
    // 「10 test」と表示される
    fmt.Println(i,t)
}
```

### *クロージャー*
クロージャとは「ローカル変数を参照している関数内の関数」

```go
package main

import "fmt"

func circleArea(pi float64) func(radius float64) float64 {
	return func(radius float64) float64 {
		return pi * radius * radius
	}
}

func main() {
	c1 := circleArea(3.14)
	fmt.Println(c1(2))

	c2 := circleArea(4)
	fmt.Println(c2(2))
}
```


- 可変長引数

ステートメント

### *if文*

「if 条件 { 処理 }」 は条件が真の時のみ処理を実行します。条件を (...) で囲む必要はありません。
{ ... } の中括弧は必須です。

```go
if x > y {
    return "Big"
} else if x < y {
    return "Small"
} else {
    return "Equal"
}
```


### *for文*
Go言語には while文が無く、繰り返し処理はすべて for を用います。

```go

//x が y よりも小さい間、処理を繰り返します。
x := 0
y := 10
for x < y {
    x++
}

//「for 開始処理; 条件; 後処理 { 処理 }」は、最初に開始処理を行い、条件が真の間、処理と後処理を繰り返し実行します。
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

//条件を省略すると無限ループになります。continue は次のループを繰り返します。break はループを抜けます。
n := 0
for {
    n++
    if n > 10 {
        break
    } else if n % 2 == 1 {
        continue
    } else {
        fmt.Println(n)
    }
}

//配列やスライスなどイテラブルなものに対しては range を用いてループ処理することができます。
colors := [...]string{"Red", "Green", "Blue"}
for i, color := range colors {
    fmt.Printf("%d: %s\n", i, color)
}

```

- range
- switch文
- defer
- log
- エラーハンドリング
- panic,recover

ポインタ

- ポインタ
- new,make
- struct

Structオリエンテッド

- メソッドとポインタレシーバーと値レシーバー
-  コンストラクタ
-  Embedded
-  non-structのメソッド
-  インターフェースとダックタイピング
-  タイプアサーションとswitch type文
-  Stringer
-  カスタムエラー
