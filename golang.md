マークダウンのページ内リンク
https://qiita.com/hennin/items/7ee58dd7d7c013a23be7

Writing Web Applications
https://go.dev/doc/articles/wiki/

# mokuji

[go](#go)
[hello-world](#hello-world)
[import](#import)
[package](#package)
[変数（var）](#変数（var）)
[型（数値、文字列、論理値、インターフェース）](#型（数値、文字列、論理値、インターフェース）)
[型変換](#型変換)
[演算子](#演算子)
[配列](#配列)
[スライス（make,cap）](#スライス（make,cap）)
[map](#map)
[クロージャー](#クロージャー)
[可変長引数](#可変長引数)
[if文](#if文)
[for文](#for文)

[range](#range)
[switch文](#switch文)
[defer](#defer)
[log](#log)
[エラーハンドリング](#エラーハンドリング)
[panic,recover](#panic,recover)

[ポインタ](#ポインタ)
[new,make](#new,make)
[struct](#struct)

[structオリエンテッド](#structオリエンテッド)

[メソッドとポインタレシーバーと値レシーバー](#メソッドとポインタレシーバーと値レシーバー)
[コンストラクタ](#コンストラクタ)
[Embedded](#Embedded)
[non-structのメソッド](#non-structのメソッド)
[インターフェースとダックタイピング](#インターフェースとダックタイピング)
[タイプアサーションとswitch-type文](#タイプアサーションとswitch-type文)
[Stringer](#Stringer)
[カスタムエラー](#カスタムエラー)

[goroutineとsync.WaitGroup](#goroutineとsync.WaitGroup)
[channel](#channel)
[Buffered-Channels](#Buffered-Channels)
[channelのrangeとclose](#channelのrangeとclose)
[producerとconsumer](#producerとconsumer)
[fan-out-fan-in](#fan-out-fan-in)
[channelとselect](#channelとselect)
[Default-Selection-と-for-break](#Default-Selection-と-for-break)
[sync.Mutex](#sync.Mutex)


[package](#package)
[PublicとPrivate](#PublicとPrivate)
[testing](#testing)
[gofmt](#gofmt)
[サードパーティーのpackageのインストール](#サードパーティーのpackageのインストール)
[godoc](#godoc)

[time](#time)
[regex](#regex)
[Sort](#Sort)
[iota](#iota)
[context](#context)
[ioutil](#ioutil)


[webapp1](#webapp1)Web Applications 1 - ioutil
[webapp2](#webapp2)Web Applications 2 - http.ListenAndServer
[webapp3](#webapp3)Web Applications 3 - templateとhttp.ResponseWriterとhttp.Request
[webapp4](#webapp4)Web Applications 4 - http.Redirect
[webapp5](#webapp5)Web Applications 5 - templateのキャッシュとハンドラー

[mokuji](#mokuji)
# *go*
    1.8からモジュール管理システム導入？

>    A Tour of Go
>    https://go-tour-jp.appspot.com/welcome/1

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
()	丸括弧(まる – )　round brackets,parenthesis
[]	角括弧(かく – )　square bracket
{}　波括弧(なみ – )　curly bracket
<>	山括弧(やま – )　angle bracket

```

[mokuji](#mokuji)
# *hello world*

```go
package main		// mainパッケージであることを宣言
import "fmt"		// fmtモジュールをインポート
func main() {		// 最初に実行されるmain()関数を定義
    fmt.Println("hello, world")
}
```

# *import*

```go
//単体
import "fmt"

//複数
import (
	"fmt"
	"os/user"
	"time"
)

//mainより先に実行される
func init() {
	fmt.Println("Init!")
}

func main() {
	fmt.Println("Hello world!", time.Now())
	fmt.Println(user.Current())
}

```

環境変数 GOPATH 配下に外部のパッケージをインストールすることができます

[mokuji](#mokuji)
# *package*
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

同じpackage内でのファイルの呼び出し
mainとsub

`lesson.goファイル`
```go
package main
func main() {
	sub()
}
```

`sub.goファイル`
```go
package main
import "log"
func sub() {
	log.Println("logging!")
	log.Printf("%T %v\n", "test", "test")
}
```


[mokuji](#mokuji)
# *変数（var）*

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

# *定数（const）*

```go
const foo = 100
const (
    foo = 100
    baa = 200
)
```
[mokuji](#mokuji)
# *型（数値、文字列、論理値、インターフェース）*

```go
int        32または64ビット(環境依存)
uint       uintと同じサイズ
uintptr    ポインタの値をそのまま格納するのに充分な大きさの符号なし整数

int8        符号あり  8ビット 整数 (-128 to 127)
int16       符号あり 16ビット 整数 (-32768 to 32767)
int32       符号あり 32ビット 整数 (-2147483648 to 2147483647)
int64       符号あり 64ビット 整数 (-9223372036854775808 to 9223372036854775807)

uint8       符号なし  8ビット 整数 (0 to 255)
uint16      符号なし 16ビット 整数 (0 to 65535)
uint32      符号なし 32ビット 整数 (0 to 4294967295)
uint64      符号なし 64ビット 整数 (0 to 18446744073709551615)

float32     IEEE-754 32ビット 浮動小数値
float64     IEEE-754 64ビット 浮動小数値

complex64   float32の実数部と虚数部を持つ複素数
complex128  float64の実数部と虚数部を持つ複素数

byte        uint8の別名(エイリアス)
rune        int32の別名(エイリアス)
```

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

[mokuji](#mokuji)
# *型変換*

```go
var (
    u8  uint8     = 255
    i8  int8      = 127
    f32 float32   = 0.2
    c64 complex64 = -5 + 12i
)
fmt.Println(u8, i8, f32, c64)
fmt.Printf("type=%T value=%v", u8, u8)

var i int64 = 64
fmt.Printf("%T\n", i) //変数の型を表示させる

var a1 int16 = 1234
var a2 uint32 = uint32(a1)

//string型からint型へ
var s string = "14"
i, _ := strconv.Atoi(s)
fmt.Printf("%T %v\n", i, i)

//int型からstring型へ
i, _ := strconv.Itoa(s)//strconvは2つ戻り値があるが「_」で受け取っている。（破棄されるということ）＞＞goは変数は全部利用しないといけないので破棄する必要がある
fmt.Println(i)
fmt.Printf("i = $T\n", i)

```

[mokuji](#mokuji)
# *演算子*

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

[mokuji](#mokuji)
# *配列*
コンパイル時に個数が決まっていて変更不可のものを 配列 と言います。型名の前に [個数] をつけて宣言します。配列のインデックスは 0 からはじまります。途中で個数を変更することはできませんが、メモリ効率や性能の点で優れています。

```go
a1 := [3]string{}
a1[0] = "Red"
a1[1] = "Green"
a1[2] = "Blue"
fmt.Println(a1[0], a1[1], a1[2])

a2 := [3]string{"Red", "Green", "Blue"}
a3 := [...]string{"Red", "Green", "Blue"}

var b []int = []int{100, 200}
b = append(b, 300)
fmt.Println(b)
```

[mokuji](#mokuji)
# *スライス（make,cap）*
メモリ効率や速度は若干落ちますが、個数を変更可能なものを スライス と呼びます。型名の前に [] をつけて宣言します。スライスには append() を用いて要素を追加します。

```go
a1 := []string{}			// スライス。個数不定
a1 = append(a1, "Red")
a1 = append(a1, "Green")
a1 = append(a1, "Blue")
fmt.Println(a1[0], a1[1], a1[2])

n := make([]int, 3, 5)
fmt.Printf("len=%d cap=%d value=%v\n", len(n), cap(n), n)
n = append(n, 0, 0)
fmt.Printf("len=%d cap=%d value=%v\n", len(n), cap(n), n)
n = append(n, 1, 2, 3, 4, 5)
fmt.Printf("len=%d cap=%d value=%v\n", len(n), cap(n), n)

a := make([]int, 3)
fmt.Printf("len=%d cap=%d value=%v\n", len(a), cap(a), a)

b := make([]int, 0)

var c []int
fmt.Printf("len=%d cap=%d value=%v\n", len(b), cap(b), b)
fmt.Printf("len=%d cap=%d value=%v\n", len(c), cap(c), c)

// c = make([]int, 5)
c = make([]int, 0, 5)
for i := 0; i < 5; i++ {
    c = append(c, i)
    fmt.Println(c)
}
fmt.Println(c)
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

[mokuji](#mokuji)
# *map*
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


var s []int
if s == nil {
    fmt.Println("Nil")
}

```


- バイト

```go
b := []byte{72, 73}
fmt.Println(b)
fmt.Println(string(b))

c := []byte("HI")
fmt.Println(c)
fmt.Println(string(c))
```
- 関数


戻り値に命名する
```go
package main

import (
	"fmt"
)

func ret(i1 int, i2 int) (i int, t string) {
	// 関数の戻り値にi,tと名前をつける
	change := i1
	i1 = i2
	i2 = change
	i = i1
	t = string(i2)

	// returnには何を返すか明記しない
	return
}

func main() {
	// iとtがreturnされる
	i, t := ret(1, 2)

	// 「10 test」と表示される
	fmt.Println(i, t)
}
```

[mokuji](#mokuji)
# *クロージャー*
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

[mokuji](#mokuji)
# *可変長引数*

```go
package main

import "fmt"

func foo(params ...int) {
	fmt.Println(len(params), params)
	for _, param := range params {
		fmt.Println(param)
	}
}

func main() {
	foo()
	foo(10, 20)
	foo(10, 20, 30)

	s := []int{1, 2, 3}
	fmt.Println(s)

	foo(s...)
}
```

ステートメント

[mokuji](#mokuji)
# *if文*

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

[mokuji](#mokuji)
# *for文*
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

[mokuji](#mokuji)
# range

```go
//string配列作成
l := []string{"python", "go", "java"}

//通常for文
for i := 0; i < len(l); i++ {
    fmt.Println(i, l[i])
}

//拡張for文
for i, v := range l {
    fmt.Println(i, v)
}

//値だけ取り出し
for _, v := range l {
    fmt.Println(v)
}


//マップ設定
m := map[string]int{"apple": 100, "banana": 200}

//キーと値取り出し
for k, v := range m {
    fmt.Println(k, v)
}

//キーだけ取り出し
for k := range m {
    fmt.Println(k)
}

//値だけ取り出し
for _, v := range m {
    fmt.Println(v)
}
```

[mokuji](#mokuji)
# *switch文*
「switch { ... }」では、case 分で条件を記述することもできます
他の言語にあるような break 文は必要ありません。逆に、次の条件の処理も実行するには fallthrough を用います。下記の例は dayOfWeek が "Sat" または "Sun" であれば "Horiday" を返します。

```go
package main

import (
	"fmt"
	"time"
)

func getOsName() string {
	return "dafdafad"
}

func main() {
	switch os := getOsName(); os {
	case "mac":
		fmt.Println("Mac!!")
	case "windows":
		fmt.Println("Windows!!")
	default:
		fmt.Println("Default!!", os)
	}

	t := time.Now()
	fmt.Println(t.Hour())
	
    //条件文はcaseに式を書ける
    switch {
	case t.Hour() < 12:
		fmt.Println("Morning")
	case t.Hour() < 17:
		fmt.Println("Afternoon")
	}
}
```

[mokuji](#mokuji)
# *defer*
「defer 処理」は、関数から戻る直前に処理を遅延実行します。リソースを忘れずに解放する際によく用いられます。
ファイルクローズ処理に使える


```go
package main

import (
	"fmt"
	"os"
)

func foo() {
	defer fmt.Println("world foo")
	fmt.Println("hello foo")
}

func main() {
	defer fmt.Println("world")
	foo()
	fmt.Println("hello")

	fmt.Println("run")

    //stockと同じ挙動
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)
	fmt.Println("success")

	file, _ := os.Open("./lesson.go")
	//ファイルオープンと同じ場所においておけば忘れない
    defer file.Close()

    //byte配列でファイルを読み込み
	data := make([]byte, 100)
	file.Read(data)

    //stringで出力
	fmt.Println(string(data))
}
```


[mokuji](#mokuji)
# *log*

```go
log.Println("logging!")
log.Printf("%T %v\n", "test", "test")

//code終了する
log.Fatalln("error!!")
```


ファイルを読み込めなかったらエラーコードを表示
```go
LoggingSettings("test.log")
_, err := os.Open("fdafdsafa")
if err != nil {
    log.Fatalln("Exit", err)
}
```

```go
func LoggingSettings(logFile string) {
	logfile, _ := os.OpenFile(logFile, os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
	multiLogFile := io.MultiWriter(os.Stdout, logfile)
	log.SetFlags(log.Ldate | log.Ltime | log.Llongfile)
	log.SetOutput(multiLogFile)
}


func main() {
	LoggingSettings("test.log")
	_, err := os.Open("fdafdsafa")
	if err != nil {
		log.Fatalln("Exit", err)
	}
	fmt.Println("ok?")
}
```


[mokuji](#mokuji)
# *エラーハンドリング*

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	//err設定
	file, err := os.Open("./lesson.go")

	if err != nil {
		log.Fatalln("Error!")
	}
	defer file.Close()

	data := make([]byte, 100)

	//err設定
	//なぜ設定済みなのに「:=」で再設定できるのか？＞＞
	//式の左辺のどれかが未設定状態からだと上書き可能
	count, err := file.Read(data)
	if err != nil {
		log.Fatalln("Error")
	}
	fmt.Println(count, string(data))

    //Chdir チェンジディレクトリ
	if err = os.Chdir("test"); err != nil {
		log.Fatalln("Error")
	}
}
```

[mokuji](#mokuji)
panic,recover

```go
package main
import "fmt"
func thirdPartyConnectDB() {
	panic("Unable to connect database!")
}

func save() {
	defer func() {
		s := recover()
		fmt.Println(s)
	}()
	thirdPartyConnectDB()
}

func main() {
	save()
	fmt.Println("OK?")
}
```


[mokuji](#mokuji)
# *ポインタ*
ポインタとは、変数が格納されているメモリのアドレスです。C言語と同様に、オブジェクトのポインタを参照するには & を、ポインタの中身を参照するには * を用います。
値渡しと参照渡しに注意

```go
var a1 int		// int型変数a1を定義
var p1 *int;		// intへのポインタ変数p1を定義

p1 = &a1		// p1にa1のポインタを設定
*p1 = 123		// ポインタの中身(つまりa1)に123を代入
fmt.Println(a1)	// => 123
```

変数の値を渡すことを「値渡し」、変数のポインタを渡すことを「参照渡し」と呼びます。値渡しでは値のコピーしか渡していないので元の変数を変更することはできませんが、ポインタ渡しであれば関数の中で変数の値を変更することが可能となります。

```go
func main() {
    var a1 int = 123
    var a2 int = 123
    fn(a1, &a2)		// a1は値渡し、a2は参照渡し
    fmt.Println(a1, a2)	// => 123 456
}

func fn(b1 int, b2 *int) {
    b1 = 456
    *b2 = 456
}
```

```go
package main

import "fmt"

type Vertex struct {
	//先頭大文字＞＞public
	//先頭小文字＞＞private
	X, Y int
	S    string
}

func changeVertex(v Vertex){
	v.X = 1000
}

func changeVertex2(v *Vertex){
	v.X = 1000
}

func main(){
	v := Vertex{1, 2, "test"}
	changeVertex(v)
	fmt.Println(v)

	v2 := &Vertex{1, 2, "test"}
	changeVertex2(v2)
	fmt.Println(*v2)
}


```


[mokuji](#mokuji)
# *new,make*

領域確保(new)
new() を用いて領域を動的に確保し、その領域へのポインタを得ることができます。確保した領域は参照されなくなった後にでガベージコレクションにより自動的に開放されます。

```go
type Book struct {
    title string
}

func main() {
    bookList := []*Book{}

    for i := 0; i < 10; i++ {
        book := new(Book)
        book.title = fmt.Sprintf("Title#%d", i)
        bookList = append(bookLlist, book)
    }
    for _, book := range bookList {
        fmt.Println(book.title)
    }
}
```

[mokuji](#mokuji)
# *struct*
Go言語では、クラス(class)の代わりに 構造体(struct) を使用します。構造体にはメンバ変数のみを定義し、クラスメソッドに相当する関数は関数名の前に (thisに相当する変数 *構造体名) をつけて定義します。
```go
package main

import "fmt"

type Vertex struct {
	//先頭大文字＞＞public
	//先頭小文字＞＞private
	X, Y int
	S    string
}

func main() {
	v := Vertex{X: 1, Y: 2}
	fmt.Println(v)        //{1 2}
	fmt.Println(v.X, v.Y) //1 2

	v.X = 100
	fmt.Println(v.X, v.Y) //100 2

	v2 := Vertex{X: 1}
	fmt.Println(v2) //{1 0 }

	v3 := Vertex{1, 2, "test"} //{1 2 test}
	fmt.Println(v3)

	v4 := Vertex{}
	var v5 = Vertex{}

	fmt.Println(v4) //{0 0 }
	fmt.Println(v5) //{0 0 }

	//領域確保（new）
	v6 := new(Vertex)
	v7 := &Vertex{}	//推奨の書き方

	fmt.Printf("%T %v\n", v6, v6) //&{0 0 }
	fmt.Printf("%T %v\n", v7, v7) //&{0 0 }

}
```


[mokuji](#mokuji)
# *Structオリエンテッド*

[mokuji](#mokuji)
# *メソッドとポインタレシーバーと値レシーバー*
メソッド実装＞＞クラスみたいな？
```go
func (レシーバ　型) 関数名(引数) 戻り値の型 {
    処理コード
}
```


```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

func (v Vertex) Area() int {
	return v.X * v.Y
}

func (v *Vertex) Scale(i int) {
	v.X = v.X * i
	v.Y = v.Y * i
}

func Area(v Vertex) int {
	return v.X * v.Y
}

func main() {
	v := Vertex{3, 4}
	// fmt.Println(Area(v))
	v.Scale(10)
	fmt.Println(v.Area())
}
```


[mokuji](#mokuji)
# *コンストラクタ*
（レシーバ　型）.New() 戻り値の型　＞＞デザインパターン


[mokuji](#mokuji)
# *Embedded*
(継承チック)
```go
package main

import "fmt"

//ベースstruct
type Vertex struct {
	x, y int
}

func (v Vertex) Area() int {
	return v.x * v.y
}

func (v *Vertex) Scale(i int) {
	v.x = v.x * i
	v.y = v.y * i
}

//ベースに追加
type Vertex3D struct {
	Vertex
	z int
}

func (v Vertex3D) Area3D() int {
	return v.x * v.y * v.z
}

func (v *Vertex3D) Scale3D(i int) {
	v.x = v.x * i
	v.y = v.y * i
	v.z = v.z * i
}

func New(x, y, z int) *Vertex3D {
	return &Vertex3D{Vertex{x, y}, z}
}

func main() {
	v := New(3, 4, 5)
	v.Scale3D(10)
	//fmt.Println(v.Area())
	fmt.Println(v.Area3D())
}
```


[mokuji](#mokuji)
# *non-structのメソッド*

```go
package main

import "fmt"

type MyInt int

func (i MyInt) Double() int {
	fmt.Printf("%T %v\n", i, i)
	fmt.Printf("%T %v\n", 1, 1)
	return int(i * 2)
}

func main() {
	myInt := MyInt(10)
	fmt.Println(myInt.Double())
}

```



[mokuji](#mokuji)
# *インターフェースとダックタイピング*
インタフェース(interface) はポリモーフィズムを実装するための機能です。下記の例では構造体 Person も、構造体 Book も ToString() というメソッドと PrintOut() というメソッドを実装しています。

```go
package main

import "fmt"

type Human interface {
	Say() string
}

type Person struct {
	Name string
}

type Dog struct {
	Name string
}

func (p *Person) Say() string {
	p.Name = "Mr." + p.Name
	fmt.Println(p.Name)
	return p.Name
}

func DriveCar(human Human) {
	if human.Say() == "Mr.Mike" {
		fmt.Println("Run")
	} else {
		fmt.Println("Get out")
	}
}

func main() {
	var mike Human = &Person{"Mike"}
	var x Human = &Person{"X"}
	DriveCar(mike)
	DriveCar(x)
	// var dog Dog = Dog{"dog"}
	// DriveCar(dog)
}
```


[mokuji](#mokuji)
# *タイプアサーションとswitch type文*
関数の無いインタフェース interface {} は、any 型のように使用することができます。下記の関数はどんな型の引数でも受け取ることができます。
タイプアサーション＞＞型を変換する作業

```go
package main

import "fmt"

func do(i interface{}) {
	/*
		ii := i.(int)
		// i = ii * 2
		ii *= 2
		fmt.Println(ii)
		ss := i.(string)
		fmt.Println(ss + "!")
	*/
	switch v := i.(type) {
	case int:
		fmt.Println(v * 2)
	case string:
		fmt.Println(v + "!")
	default:
		fmt.Printf("I don't know %T\n", v)
	}
}

func main() {
	do(10)
	do("Mike")
	do(true)

	var i int = 10
	ii := float64(10)
	fmt.Println(i, ii)
}

```

[mokuji](#mokuji)
# *Stringer*
stringer とは定数を定数名の文字列に変換する関数を生成するコマンド
javaでいうToString

```go
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("My name is %v.", p.Name)
}

func main() {
	mike := Person{"Mike", 22}
	fmt.Println(mike)
}
```


[mokuji](#mokuji)
# *カスタムエラー*

```go
package main

import (
	"fmt"
)

type UserNotFound struct {
	Username string
}

//*(ポインタ)つける
func (e *UserNotFound) Error() string {
	return fmt.Sprintf("User not found: %v", e.Username)
}

func myFunc() error {
	// Something wrong
	ok := false
	if ok {
		return nil
	}
	//&(アンパサンド)つける。エラー同士の比較のため
	return &UserNotFound{Username: "mike"}
}

func main() {
	if err := myFunc(); err != nil {
		fmt.Println(err)
	}
}
```


# *goroutineとsync.WaitGroup*
所謂スレッド

```go
package main

import (
	"fmt"
	"sync"
)

func goroutine(s string, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 0; i < 5; i++ {
		//time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func normal(s string) {
	for i := 0; i < 5; i++ {
		//time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	var wg sync.WaitGroup
	wg.Add(1)
	go goroutine("world", &wg)
	normal("hello")
	// time.Sleep(2000 * time.Millisecond)
	wg.Wait()
}
```

# *channel*
スレッド

```go
package main

import "fmt"

func goroutine1(s []int, c chan int) {
	sum := 0
	
	for _, v := range s {
		sum += v
	}

	c <- sum
}

func main() {
	s := []int{1, 2, 3, 4, 5}

	//チャネル
	c := make(chan int) // 15, 15
	go goroutine1(s, c)
	go goroutine1(s, c)
	x := <-c
	fmt.Println(x)
	y := <-c
	fmt.Println(y)
}

```


# *Buffered Channels*
何本スレ立てるか
サイキックバリア

```go
package main

import "fmt"

func main() {
	ch := make(chan int, 2)
	ch <- 100
	fmt.Println(len(ch))
	ch <- 200
	fmt.Println(len(ch))
	
	//チャネルクローズは必須
	close(ch)

	for c := range ch {
		fmt.Println(c)
	}

}

```


# *channelのrangeとclose*
関数ごとにチャネルに流す

```go
package main

import "fmt"

func goroutine1(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
		c <- sum
	}

	//close
	close(c)
}

func main() {
	s := []int{1, 2, 3, 4, 5}
	c := make(chan int, len(s))
	go goroutine1(s, c)
	for i := range c {
		fmt.Println(i)
	}
}

```


# *producerとconsumer*
複数のproducerがchanに処理を集めてchanを通してconsumerへ

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func producer(ch chan int, i int) {
	// Something
	ch <- i * 2
}

func consumer(ch chan int, wg *sync.WaitGroup) {
	for i := range ch {
		//インナーファンクション
		func() {
			defer wg.Done()
			fmt.Println("process", i*1000)
		}()
	}

	//呼び出し元でcloseしてないと呼ばれない
	//チャネルの終了を待ち続ける
	fmt.Println("###################")
}

func main() {
	var wg sync.WaitGroup
	ch := make(chan int)

	// Producer
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go producer(ch, i)
	}

	// Consumer
	go consumer(ch, &wg)
	wg.Wait()
	close(ch)
	time.Sleep(2 * time.Second)
	fmt.Println("Done")
}
```


# *fan-out fan-in*
gorutineのチェーン

```go
package main

import "fmt"

func producer(first chan int) {
	defer close(first)
	for i := 0; i < 10; i++ {
		first <- i
	}
}

//チャネルの矢印は省略できるが見やすいように省略しない
func multi2(first <-chan int, second chan<- int) {
	defer close(second)
	for i := range first {
		second <- i * 2
	}
}

func multi4(second chan int, third chan int) {
	defer close(third)
	for i := range second {
		third <- i * 4
	}
}

func main() {
	first := make(chan int)
	second := make(chan int)
	third := make(chan int)

	go producer(first)
	go multi2(first, second)
	go multi4(second, third)
	for result := range third {
		fmt.Println(result)
	}
}
```


# *channelとselect*
チャネル受信の選択

```go
package main

import (
	"fmt"
	"time"
)

func goroutine1(ch chan string) {
	for {
		ch <- "packet from 1"
		time.Sleep(3 * time.Second)
	}
}

func goroutine2(ch chan int) {
	for {
		ch <- 100
		time.Sleep(1 * time.Second)
	}
}

func main() {
	c1 := make(chan string)
	c2 := make(chan int)
	go goroutine1(c1)
	go goroutine2(c2)

	for {
		select {
		case msg1 := <-c1:
			fmt.Println(msg1)
		case msg2 := <-c2:
			fmt.Println(msg2)
		}
	}
}
```

# *Default Selection と for break*
package main

```go
import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(100 * time.Millisecond)
	boom := time.After(500 * time.Millisecond)
OuterLoop:
	for {
		select {
		case <-tick:
			fmt.Println("tick.")
		case <-boom:
			fmt.Println("BOOM!")
			break OuterLoop
			// return
		default:
			fmt.Println("    .")
			time.Sleep(50 * time.Millisecond)
		}
	}
	fmt.Println("##############")
}
```


# *sync.Mutex*
シンクロミューテックス
goroutineによる並行処理で共有リソースにアクセスしなければならないとき、排他制御が必要になります。ここでは、sync.Mutexを利用した排他制御の実装方法を確認します。(mutexは、mutual exclusionの略です。)

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

type Counter struct {
	v   map[string]int
	mux sync.Mutex
}

func (c *Counter) Inc(key string) {
	c.mux.Lock()
	defer c.mux.Unlock()
	c.v[key]++
}

func (c *Counter) Value(key string) int {
	c.mux.Lock()
	defer c.mux.Unlock()
	return c.v[key]
}

func main() {
	// c := make(map[string]int)
	c := Counter{v: make(map[string]int)}
	go func() {
		for i := 0; i < 10; i++ {
			// c["key"] += 1
			c.Inc("Key")
		}
	}()
	go func() {
		for i := 0; i < 10; i++ {
			// c["key"] += 1
			c.Inc("Key")
		}
	}()
	time.Sleep(1 * time.Second)
	fmt.Println(c, c.Value("Key"))
}
```

```go
package main

import "fmt"

func goroutine(s []string, c chan string) {
	sum := ""
	for _, v := range s {
		sum += v
		c <- sum
	}
	close(c)
}

func main() {
	words := []string{"test1!", "test2!", "test3!", "test4!"}
	c := make(chan string)
	go goroutine(words, c)
	for w := range c {
		fmt.Println(w)
	}
}
```

# package


# PublicとPrivate
関数名の先頭が大文字か小文字か


# testing
設定＞＞testingに✓

テストしたい関数
```go
package mylib

func Average(s []int) int {
	total := 0
	for _, i := range s {
		total += i
	}
	return int(total / len(s))
}

```

テスト
```go
package mylib

import "testing"

func TestAverage(t *testing.T) {
	if Debug{
		t.Skip("Skip Reason")
	}

	v := Average([]int{1, 2, 3, 4, 5})
	if v != 3 {
		t.Error("Expected 3, got", v)
	}
}
```

# gofmt
フォーマット

# サードパーティーのpackageのインストール
talib

# godoc

# time
時間用ライブラリ

```go
/* https://golang.org/pkg/time/

const (
        ANSIC       = "Mon Jan _2 15:04:05 2006"
        UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
        RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
        RFC822      = "02 Jan 06 15:04 MST"
        RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
        RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
        RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
        RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
        RFC3339     = "2006-01-02T15:04:05Z07:00"
        RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
        Kitchen     = "3:04PM"
        // Handy time stamps.
        Stamp      = "Jan _2 15:04:05"
        StampMilli = "Jan _2 15:04:05.000"
        StampMicro = "Jan _2 15:04:05.000000"
        StampNano  = "Jan _2 15:04:05.000000000"
)
*/
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	fmt.Println(t)
	fmt.Println(t.Format(time.RFC3339))
	fmt.Println(t.Year(), t.Month(), t.Day(),
		t.Hour(), t.Minute(), t.Second())
}

```

# regex
正規表現

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	match, _ := regexp.MatchString("a([a-z]+)e", "apple")
	fmt.Println(match)

	r := regexp.MustCompile("a([a-z]+)e")
	ms := r.MatchString("apple")
	fmt.Println(ms)

	r2 := regexp.MustCompile("^/(edit|save|view)/([a-zA-Z0-9]+)$")
	fs := r2.FindString("/view/test")
	fmt.Println(fs)
	fss := r2.FindStringSubmatch("/view/test")
	fmt.Println(fss, fss[0], fss[1], fss[2])
	fss = r2.FindStringSubmatch("/edit/test")
	fmt.Println(fss, fss[0], fss[1], fss[2])
	fss = r2.FindStringSubmatch("/save/test")
	fmt.Println(fss, fss[0], fss[1], fss[2])
}
```

# Sort

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	i := []int{5, 3, 2, 8, 7}
	s := []string{"d", "a", "f"}
	p := []struct {
		Name string
		Age  int
	}{
		{"Nancy", 20},
		{"Vera", 40},
		{"Mike", 30},
		{"Bob", 50},
	}
	fmt.Println(i, s, p)
	sort.Ints(i)
	sort.Strings(s)
	sort.Slice(p, func(i, j int) bool { return p[i].Name < p[j].Name })
	sort.Slice(p, func(i, j int) bool { return p[i].Age < p[j].Age })
	fmt.Println(i, s, p)
}
```

# iota
連番を振る


```go
package main

import "fmt"

const (
	c1 = iota
	c2
	c3
)

const (
	_      = iota

	//10桁シフト
	KB int = 1 << (10 * iota)
	MB
	GB
)

func main() {
	fmt.Println(c1, c2, c3)
	fmt.Println(KB, MB, GB)
}

```

# context

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func longProcess(ctx context.Context, ch chan string) {
	fmt.Println("run")
	time.Sleep(2 * time.Second)
	fmt.Println("finish")
	ch <- "result"
}

func main() {
	ch := make(chan string)
	ctx := context.Background()
	ctx, cancel := context.WithTimeout(ctx, 1 * time.Second)
	//defer cancel()
	//ctx := context.TODO()
	go longProcess(ctx, ch)
	cancel()

CTXLOOP:
	for {
		select {
		case <-ctx.Done():
            fmt.Println("%%%%%%%%%%%%%%%%%%%%%%%%")
			fmt.Println(ctx.Err())
			break CTXLOOP
		case <-ch:
			fmt.Println("success")
			break CTXLOOP
		}
	}
	fmt.Println("################")
}

```
# ioutil
パケット読み込んで、パケットに書き込む等に使える

```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
)

func main() {
	//content, err := ioutil.ReadFile("main.go")
	//if err != nil{
	//	log.Fatalln(err)
	//}
	//fmt.Println(string(content))
	//
	//if err := ioutil.WriteFile("ioutil_temp.go", content, 0666); err != nil{
	//	log.Fatalln(err)
	//}

	r := bytes.NewBuffer([]byte("abc"))
	content, _ := ioutil.ReadAll(r)
	fmt.Println(string(content))
}
```

# http
```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"net/http"
	"net/url"
)

func main() {
	//resp, _ := http.Get("http://example.com")
	//defer resp.Body.Close()
	//body, _ := ioutil.ReadAll(resp.Body)
	//fmt.Println(string(body))

	//URLが正しいかチェック
	base, _ := url.Parse("https://example.com/")
	
	//
	reference, _ := url.Parse("/test?a=1&b=2")
	endpoint := base.ResolveReference(reference).String()
	fmt.Println(endpoint)
	// req, _ := http.NewRequest("GET", endpoint, nil)
	req, _ := http.NewRequest("POST", endpoint, bytes.NewBuffer([]byte("password")))
	
	
	req.Header.Add("If-None-Match", `W/"wyzzy"`)
	q := req.URL.Query()
	q.Add("c", "3&%")
	fmt.Println(q)
	fmt.Println(q.Encode())
	req.URL.RawQuery = q.Encode()

	var client *http.Client = &http.Client{}
	resp, _ := client.Do(req)
	body, _ := ioutil.ReadAll(resp.Body)
	fmt.Println(string(body))

}

```


# json.UnmarshalとMarshalとエンコード

```go
package main

import (
	"encoding/json"
	"fmt"
)

type T struct{}

//structの中にstruct入れる場合はポインタつける。オミットエンプティで消せる
type Person struct {
	Name      string   `json:"name,omitempty"`
	Age       int      `json:"age,omitempty"`
	Nicknames []string `json:"nicknames,omitempty"`
	T		  *T	   `json:"T,omitempty"`
}

func (p *Person) UnmarshalJSON(b []byte) error {
	type Person2 struct {
		Name string
	}
	var p2 Person2
	err := json.Unmarshal(b, &p2)
	if err != nil {
		fmt.Println(err)
	}
	p.Name = p2.Name
	return err
}

//データの加工
func (p Person) MarshalJSON() ([]byte, error) {
	v, err := json.Marshal(&struct {
		Name string
	}{
		Name: "Mr " + p.Name,
	})
	return v, err
}

func main() {
	//データ受け取り
	b := []byte(`{"name":"mike","age":20,"nicknames":["a","b","c"]}`)
	var p Person

	//structへ
	if err := json.Unmarshal(b, &p); err != nil {
		fmt.Println(err)
	}
	fmt.Println(p.Name, p.Age, p.Nicknames)

	//外へ送り返す
	v, _ := json.Marshal(p)
	fmt.Println(string(v))
}

```
# hmacでAPI認証
テンプレ

```go
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"fmt"
)

var DB = map[string]string{
	"User1Key": "User1Secret",
	"User2Key": "User2Secret",
}

func Server(apiKey, sign string, data []byte) {
	apiSecret := DB[apiKey]
	h := hmac.New(sha256.New, []byte(apiSecret))
	h.Write(data)
	expectedHMAC := hex.EncodeToString(h.Sum(nil))
	fmt.Println(sign == expectedHMAC)
}

func main() {
	const apiKey = "User2Key"
	const apiSecret = "User2Secret"

	data := []byte("data")
	h := hmac.New(sha256.New, []byte(apiSecret))
	h.Write(data)
	sign := hex.EncodeToString(h.Sum(nil))

	fmt.Println(sign)

	Server(apiKey, sign, data)
}
```

# webapp1
Web Applications 1 - ioutil

```go
package main

import (
	"fmt"
	"io/ioutil"
)

type Page struct {
	Title string
	Body  []byte
}


//データを保存する
func (p *Page) save() error {
	filename := p.Title + ".txt"
	return ioutil.WriteFile(filename, p.Body, 0600)
}

//データを書き込む
func loadPage(title string) (*Page, error) {
	filename := title + ".txt"
	body, err := ioutil.ReadFile(filename)
	if err != nil {
		return nil, err
	}
	return &Page{Title: title, Body: body}, nil
}

func main() {
	p1 := &Page{Title: "test", Body: []byte("This is a sample Page.")}
	p1.save()

	p2, _ := loadPage(p1.Title)
	fmt.Println(string(p2.Body))
}

```


# webapp2
Web Applications 2 - http.ListenAndServer

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

type Page struct {
	Title string
	Body  []byte
}

func (p *Page) save() error {
	filename := p.Title + ".txt"
	return ioutil.WriteFile(filename, p.Body, 0600)
}

func loadPage(title string) (*Page, error) {
	filename := title + ".txt"
	body, err := ioutil.ReadFile(filename)
	if err != nil {
		return nil, err
	}
	return &Page{Title: title, Body: body}, nil
}

func viewHandler(w http.ResponseWriter, r *http.Request) {
	// /view/test
	title := r.URL.Path[len("/view/"):]
	p, _ := loadPage(title)
	fmt.Fprintf(w, "<h1>%s</h1><div>%s</div>", p.Title, p.Body)
}

func main() {
	http.HandleFunc("/view/", viewHandler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}

```

# webapp3
Web Applications 3 - templateとhttp.ResponseWriterとhttp.Request

# webapp4
Web Applications 4 - http.Redirect

# webapp5
Web Applications 5 - templateのキャッシュとハンドラー