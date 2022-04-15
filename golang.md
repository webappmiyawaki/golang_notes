マークダウンのページ内リンク
https://qiita.com/hennin/items/7ee58dd7d7c013a23be7

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
i, _ := strconv.Atoi(s)//strconvは2つ戻り値があるが「_」で受け取っている。（破棄されるということ）＞＞goは変数は全部利用しないといけないので破棄する必要がある
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

[mokuji](#mokuji)
# *non-structのメソッド*

[mokuji](#mokuji)
# *インターフェースとダックタイピング*

[mokuji](#mokuji)
# *タイプアサーションとswitch type文*

[mokuji](#mokuji)
# *Stringer*

[mokuji](#mokuji)
# *カスタムエラー*
