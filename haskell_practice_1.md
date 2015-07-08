# Haskell 入門 (1)


## Haskell とは
関数型言語のオープンな標準として開発された純粋関数型言語

以下のような特徴をもつ
* 純粋 (関数が副作用をもたない)
* 静的型付け (コンパイル時に型のチェックを行う)
* 型推論 (全ての変数の型を明示的に指定しなくて良い)
* 遅延評価 (必要になるまで式を評価しない)

## Haskell をはじめるには

Haskell コンパイラをインストールする<br>
(最も広く使われているコンパイラは、 The Glasgow Haskell Compiler: GHC)

または、Haskell Platform (コンパイラと便利なライブラリのセット) をダウンロード

### Mac
```
$ brew install ghc
```

### Windows
https://www.haskell.org/platform/windows.html

### Linux
https://www.haskell.org/platform/linux.html

### インタプリタの起動
```
$ ghci
GHCi, version 7.10.1: http://www.haskell.org/ghc/  :? for help
Prelude>
```
Prelude は Haskell の基本モジュールのうち、デフォルトで使用できる状態になっているモジュール

独自の関数を定義する場合は、関数を hoge.hs というファイルに保存し `:l hoge` でロードする
```
Prelude> :l hoge
[1 of 1] Compiling Main             ( hoge.hs, interpreted )
Ok, modules loaded: Main.
*Main>
```

## リスト
リストは同じ型の要素を複数個持ったデータ構造<br>
整数のリスト、文字のリストなど
```
Prelude> [1,2,3]
[1,2,3]
Prelude> ['a','b','c']
"abc" -- 文字列は文字のリストとして表される
```

### リストの連結
```
Prelude> [1,2,3] ++ [5,6,7]
[1,2,3,5,6,7]
Prelude> "hello" ++ " " ++ "world"
"hello world"
```

```
Prelude> 'A': " SMALL CAT"
"A SMALL CAT"
Prelude> 5:[1,2,3]
[5,1,2,3]
```
※ リストの要素と同じ型をリストの先頭に追加する<br>
※ `[1,2,3]` は `1:2:3:[]` の構文糖衣


### リストの要素へのアクセス
```
Prelude> "Hello World" !! 6
'W'
Prelude> [1,2,3,4,5] !! 1
2
```

### 基本的なリスト関数
```
Prelude> head [5,4,3,2,1] -- 先頭の要素を返す
5
Prelude> tail [5,4,3,2,1] -- 先頭を除いた残りのリストを返す
[4,3,2,1]
Prelude> last [5,4,3,2,1] -- 末尾の要素を返す
1
Prelude> init [5,4,3,2,1] -- 末尾を除いた残りのリストを返す
[5,4,3,2]

Prelude> length [5,4,3,2,1] -- リストの長さを返す
5

Prelude> null [5,4,3,2,1] -- リストが空かどうか
False
Prelude> null []
True

Prelude> reverse [5,4,3,2,1] -- リストを逆順に変換する
[1,2,3,4,5]

Prelude> take 3 [5,4,3,2,1] -- リストの先頭から指定された数の要素を返す
[5,4,3]
Prelude> drop 3 [5,4,3,2,1] -- 指定された数の要素を先頭から削除したリストを返す
[2,1]
Prelude> elem 4 [5,4,3,2,1] -- 指定した要素が含まれているか
True

Prelude> maximum [5,4,3,2,1] -- リストの中の最大値を返す
5
Prelude> sum [5,4,3,2,1] -- リストの和を返す
15
Prelude> product  [5,4,3,2,1] -- リストの積を返す
120
```

### range
```
Prelude> [1..20]
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
Prelude> ['a'..'z']
"abcdefghijklmnopqrstuvwxyz"

-- 等差数列の場合はステップを指定することもできる
Prelude> [2,4..20]
[2,4,6,8,10,12,14,16,18,20]
Prelude> [3,6..20]
[3,6,9,12,15,18]
```

### リスト内包表記
リストのフィルタリング、変換、組み合わせを行う方法<br>
数学における**集合の内包的記法**の概念に近い

例）「10以下の全ての自然数に対してそれぞれ2倍したリストのうち、12以上のもの」
```
Prelude> [x*2 | x <- [1..10], x*2 >= 12]
[12,14,16,18,20]
```

## タプル
複数の違う型の要素を1つの値として格納する

リストとは以下の点で異なる
* 複数の違う型の要素を格納できる
* サイズが固定

```
Prelude> (3, 'a', "hello")
(3,'a',"hello")
```

### ペア
ペアには標準で便利な関数が用意されている

```
Prelude> fst("Wow", False) -- 1つ目の要素を返す
"Wow"
Prelude> snd("Wow", False) -- 2つ目の要素を返す
False

-- ペアのリストをつくる
Prelude> zip [1..] ["apple", "orange", "cherry", "mango"]
[(1,"apple"),(2,"orange"),(3,"cherry"),(4,"mango")]
```


## 型

式の型は GHCi の `:t` コマンドで調べることができる
```
Prelude> :t 'a'
'a' :: Char
Prelude> :t True
True :: Bool
Prelude> :t "HELLO!"
"HELLO!" :: [Char]
```

自分で関数を定義する場合は、（慣習的に）明示的に型宣言を行う<br>
例）3つの整数を足し合わせる関数
```
addTree :: Int -> Int -> Int -> Int
addTree x y z = x + y + z
```
### 一般的な型

** Int ** : 整数（有界）、最大値、最小値をもつ

** Integer ** : 整数（有界でない）

** Float ** : 単精度小数点数

** Double ** : 倍精度小数点数

** Bool ** : 真偽値

** Char ** : Unicode 文字、シングルクォート (') で囲む

** タプル ** : 要素の数とその型によって決まる


### 型クラス
**型クラス**は、何らかの振る舞いを定義するインターフェース

ある型クラスの**インスタンス**である型は、その型クラスが記述する振る舞いを実装する

ダックタイピング（もしもそれがアヒルのように歩き、アヒルのように鳴くのなら、それはアヒルである）


#### Eq 型クラス
```
Prelude> :i Eq
class Eq a where
  (==) :: a -> a -> Bool
  (/=) :: a -> a -> Bool
```

* 等値性をテストする型クラス
* Eq のインスタンスが実装すべき関数は `==` と `/=`
* Haskell の標準型は Eq のインスタンス

```
Prelude> 5 == 5
True
Prelude> "Hello" /= "Hello"
False
```

#### Ord 型クラス
````
Prelude> :i Ord
class Eq a => Ord a where
  compare :: a -> a -> Ordering
  (<) :: a -> a -> Bool
  (<=) :: a -> a -> Bool
  (>) :: a -> a -> Bool
  (>=) :: a -> a -> Bool
  max :: a -> a -> a
  min :: a -> a -> a
````

* 何らかの順序を付けられる型クラス
* Ord はすべての標準的な大小比較関数 `>`, `<`, `>=`, `<=` をサポートしている
* `compare` 関数は引数を2つ取り、両者の関係性を返す

```
Prelude> 'b' > 'a'
True
Prelude> compare 5 3
GT
```

#### Show 型クラス
* ある値は、その型が Show 型クラスのインスタンスであれば、文字列として表現できる
* Haskell の標準型はすべて Show のインスタンス
* `show` 関数で指定した値を文字列として表示する

```
Prelude> :t show
show :: Show a => a -> String

Prelude> show 3.14
"3.14"
Prelude> show True
"True"
```

#### Read 型クラス
* Show と対をなす型クラス
* `read` 関数は文字列を受け取り、Read のインスタンスの型を返す

```
Prelude> :t read
read :: Read a => String -> a

Prelude> read "True" || False
True
Prelude> read "[1,2,3,4]" ++ [5]
[1,2,3,4,5]
Prelude> read "5" :: Float
5.0
```
※ コンパイラが型推論できない場合は、`::` で型注釈を付ける必要がある

#### Enum 型クラス
```
Prelude> :i Enum
class Enum a where
  succ :: a -> a
  pred :: a -> a
  toEnum :: Int -> a
  fromEnum :: a -> Int
  enumFrom :: a -> [a]
  enumFromThen :: a -> a -> [a]
  enumFromTo :: a -> a -> [a]
  enumFromThenTo :: a -> a -> a -> [a]
```

* 順番に並んだ型、つまり要素の値を列挙できる型
* Enum 型クラスは、その値をレンジの中で使える
* 後者関数 `succ` と前者関数 `pred` が定義される
* Enum のインスタンスには (), Bool, Char, Ordering, Int, Integer, Float, Double などがある

```
Prelude> [LT .. GT]
[LT,EQ,GT]
Prelude> succ 'B'
'C'
```

#### Bounded 型クラス
```
Prelude> :i Bounded
class Bounded a where
  minBound :: a
  maxBound :: a
```

* Bounded 型クラスのインスタンスは、上限と下限を持つ

```
Prelude> minBound ::Int
-9223372036854775808
Prelude> maxBound ::Char
'\1114111'
Prelude> minBound ::Bool
False
```

#### Num 型クラス
```
Prelude> :i Num
class Num a where
  (+) :: a -> a -> a
  (-) :: a -> a -> a
  (*) :: a -> a -> a
  negate :: a -> a
  abs :: a -> a
  signum :: a -> a
  fromInteger :: Integer -> a
```

* Num は数の型クラス
* Int, Integer, Float, Double として振る舞うことができる

#### Floating 型クラス
```
Prelude> :i Floating
class Fractional a => Floating a where
  pi :: a
  exp :: a -> a
  log :: a -> a
  sqrt :: a -> a
  (**) :: a -> a -> a
  logBase :: a -> a -> a
  sin :: a -> a
  cos :: a -> a
  tan :: a -> a
  asin :: a -> a
  acos :: a -> a
  atan :: a -> a
  sinh :: a -> a
  cosh :: a -> a
  tanh :: a -> a
  asinh :: a -> a
  acosh :: a -> a
  atanh :: a -> a
    -- Defined in ‘GHC.Float’
instance Floating Float -- Defined in ‘GHC.Float’
instance Floating Double -- Defined in ‘GHC.Float’
```

* Float と Double を含む

#### Integral 型クラス
```
Prelude> :i Integral
class (Real a, Enum a) => Integral a where
  quot :: a -> a -> a
  rem :: a -> a -> a
  div :: a -> a -> a
  mod :: a -> a -> a
  quotRem :: a -> a -> (a, a)
  divMod :: a -> a -> (a, a)
  toInteger :: a -> Integer
    -- Defined in ‘GHC.Real’
instance Integral Word -- Defined in ‘GHC.Real’
instance Integral Integer -- Defined in ‘GHC.Real’
instance Integral Int -- Defined in ‘GHC.Real’
```

* 整数全体のみ (Int, Integer) を含む


## 関数の構文

### パターンマッチ （データの構造をみる）
パターンマッチは、ある種のデータが従うべきパターンを指定し、そのパターンに従ってデータを分解する

Haskell では、関数を定義する際にこのパターンマッチを使って関数本体を場合分けする

例）渡された数が 7 かどうかを調べる関数
```
lucy :: Int -> String
lucy 7 = "Lucy Number Seven!"
lucy x = "Sorry, You're out of Luck!"
```

* パターンが上から順に評価される
* 渡された値がパターンに合致すると対応する関数本体が使われる
* 残りのパターンは無視される
* 全てに合致するパターンを最後に記述する

#### タプルのパターンマッチ
例）2次元ベクトルの和を求める関数

パターンマッチを使わない場合
```
addVectors :: (Double, Double) -> (Double, Double) -> (Double, Double)
addVectors a b = (fst a + fst b, snd a + snd b)
```

パターンマッチを使った場合
```
addVectors :: (Double, Double) -> (Double, Double) -> (Double, Double)
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)
```
* 引数がタプルであることがわかりやすくなる
* 各要素に適切な名前が付いているので可読性が良くなる

#### リストのパターンマッチ
空のリスト `[]` 、または `:` を含むパターンとマッチさせることができる

例）独自の head 関数 head'
```
head' :: [a] -> a
head' [] = error "Can't call head on an empty list, dummy!"
head' (x:_) = x
```
※ x:xs というパターンは、リストの先頭を x に束縛し、残りを xs に束縛する（[1,2,3] は 1:2:3:[] の構文糖衣であることから）

### ガード （データの性質をみる）
引数の値が満たす性質で場合分けするときにはガードを使用する

例）BMI によって異なる叱り方をする関数
```
bmiTell :: Double -> String
bmiTell bmi
    | bmi <= 18.5 = "You're underweight, you emo, you!"
    | bmi <= 25.0 = "You're supposedly normal. pffft, I bet you're ugly!"
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"
    | otherwise   = "You're whale, congratulations!"
```
* ガードには、パイプ文字 (|) とそれに続く真理値式、さらにその式が True に評価されたときに使われる関数本体が続く
* 最低1つのスペースでインデントする必要がある
* otherwise 句はすべてをキャッチする


### where
where キーワードをガードの後に置いて変数や関数を定義できる
```
bmiTell :: Double -> Double -> String
bmiTell weight height
    | bmi <= skinny = "You're underweight, you emo, you!"
    | bmi <= normal = "You're supposedly normal. pffft, I bet you're ugly!"
    | bmi <= fat = "You're fat! Lose some weight, fatty!"
    | otherwise   = "You're whale, congratulations!"
    where bmi = weight / height ^ 2
          skinny = 18.5
          normal = 25.0
          fat = 30.0
```

* where のスコープは関数内に限られる
* where による束縛は違うパターンの関数の間では共有されない

### let 式
例）円柱の表面積を求める関数
```
cylinder :: Double -> Double -> Double
cylinder r h =
    let sideArea = 2 * pi * r * h
        topArea = pi * r^2
    in sideArea + 2 * topArea
```

* let <bindings> in <expression> の形
* let は式
* let で定義した変数のスコープは let 式全体

### case 式
```
head'' :: [a] -> a
head'' xs = case xs of [] -> error "Can't call head on an empty list, dummy!"
                       (x:_) -> x
```

* case は式
* パターンマッチは case 式の構文糖衣
* 構文
```
case <expression> of <pattern> -> <result>
                     <pattern> -> <result>
                     <pattern> -> <result>
                     ...
```



