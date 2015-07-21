# Haskell 入門 (3)

## Hello, World!

`helloworld.hs` というファイル名で以下のコードを作成
```
main = putStrLn "Hello, World"
```

コンパイルする
```
$ ghc --make helloworld
[1 of 1] Compiling Main             ( helloworld.hs, helloworld.o )
Linking helloworld ...
```

実行
```
$ ./helloworld
Hello, World
```

## I/O アクション

関数 `putStrLn` の型を調べる
```
Prelude> :t putStrLn
putStrLn :: String -> IO ()

Prelude> :t putStrLn "Hello, World"
putStrLn "Hello, World" :: IO ()
```

* `putStrLn` 関数は、「文字列を引数に受け取り、空のタプル型を結果とする **I/O アクション**を返す」
* I/O アクションとは、実行されると副作用を含む動作を行って結果を返すような何か
* このことを「 I/O アクションが結果を**生成**する」という
* I/O アクションに main という名前を付けて実行すると、 I/O アクションが実行される
* 複数の I/O アクションをまとめるには、 **do 構文** を使う
```
main = do
    putStrLn "Hello, what's your name?"
    name <- getLine
    putStrLn $ "Hey " ++ name ++ ", you rock!"
```

* I/O アクションの生成した結果を受け取るには、 `<-` を使用する
```
Prelude> :t getLine
getLine :: IO String
```


## I/O 関数

### putStrLn
文字列を引数に取り、その文字列を端末に表示する I/O アクションを返す<br>
文字列を表示した後に改行を出力

### putStr
文字列を引数に取り、その文字列を端末に表示する I/O アクションを返す<br>
文字列を表示した後に改行を出力しない

### putChar
文字を受け取り、その文字を端末に表示する I/O アクションを返す

### print
Show 型クラスのインスタンスを受け取り、show を適用してその文字列を端末に出力する<br>
`show . putStrLn` と同義


## モナド

文脈 (context) を持った値、あるいは値を入れることができる箱のようなものを考える

この箱に関数を適用した時に、文脈 (context) によって異なる結果が返る

### 例）Maybe 型
```
Prelude> :i Maybe
data Maybe a = Nothing | Just a
```

```
Prelude> Just 3
Just 3
Prelude> Nothing
Nothing
Prelude> :t Just 3
Just 3 :: Num a => Maybe a
Prelude> :t Nothing
Nothing :: Maybe a
```

* Maybe a 型は、リスト型 [a] に似ている
* リストが複数（0 個、1 個またはそれ以上）の要素を持てるのに対し、Maybe a 型の値は、0 個か 1 個の要素だけ持てる
* この型は、失敗する可能性があることを表現するのに使用する

例）リストから条件に合致する最初の値を取得する関数
```
Prelude> :m + Data.List
Prelude Data.List> :t find
find :: Foldable t => (a -> Bool) -> t a -> Maybe a
```
※ 条件にあう要素が見つからなければ Nothing を返す


## Functor 型クラス
```
Prelude> :i Functor
class Functor (f :: * -> *) where
  fmap :: (a -> b) -> f a -> f b
```

* Functor は全体を map over できる型クラス
* Functor は fmap 関数を持っている
* **Functor は箱に入っている値に対して関数を適用できる**

* fmap は、「ある型 a から別の型 b への関数」と、「ある型 a に適用されたファンクター値」を受け取り「ある型 b に適用されたファンクター値」を返す関数
* map はリスト型に対して動作する fmap 関数

```
Prelude> (+3) (Just 2)

<interactive>:64:1:
    Non type-variable argument in the constraint: Num (Maybe a)
    (Use FlexibleContexts to permit this)
    When checking that ‘it’ has the inferred type
      it :: forall a. (Num a, Num (Maybe a)) => Maybe a
※ 値が入っている箱に対して直接関数を適用することはできない

Prelude> fmap (+3) (Just 2)
Just 5
※ fmap を使用して関数を適用する
```

### Functor 型クラスのインスタンス
[], Maybe, IO など

## Applicative 型クラス
```
Prelude> :i Applicative
class Functor f => Applicative (f :: * -> *) where
  pure :: a -> f a
  (<*>) :: f (a -> b) -> f a -> f b
```

* ある型が Applicative 型なら、それは Functor にも属している
* pure 関数は、値を取って、内部にその値を持った文脈 (context) で包む
* <*> 関数で、Applicative 値に Applicative の関数を適用する
* **Applicative は箱に入っている値に対して、箱に入っている関数を適用できる**
* Applicative 型クラスでは、<*> を連続して使うことができる

```
Prelude> Just (+3) <*> Just 9
Just 12
Prelude> Just (+3) <*> Nothing
Nothing
Prelude> pure (+) <*> Just 3 <*> Just 9
Just 12
```

### Applicative 型クラスのインスタンス
[], Maybe, IO など

## Monad 型クラス
```
Prelude> :i Monad
class Applicative m => Monad (m :: * -> *) where
  return :: a -> m a
  (>>=) :: m a -> (a -> m b) -> m b
```

* ある型が Monad 型なら、それは Applicative にも属している
* return 関数は、値を取って、内部にその値を持った文脈 (context) で包む
* \>>= 関数で、Monad 値に、普通の値を取って Monad 値を返す関数を適用する
* 関数 \>>= はバインド (bind) と呼ばれる
* **Monad は箱に入っている値に対して、普通の値を取って箱を返す関数を適用できる**
* Monad 型クラスでは、\>>= を連続して使うことができる

```
Prelude> return "HOGE" :: Maybe String
Just "HOGE"
Prelude> Just 9 >>= \x -> return (x*10)
Just 90
Prelude> Nothing >>= \x -> return (x*10)
Nothing
```

### Monad 型クラスのインスタンス
[], Maybe, IO など

## IO モナドの例
`getLine` は引数を取らずユーザの入力を待つ
```
Prelude> :t getLine
getLine :: IO String
```

`readFile` は文字列（ファイルパス）を受け取り、ファイルの中身を返す
```
Prelude> :t readFile
readFile :: FilePath -> IO String
```

`putStrLn` は文字列を受け取り出力する
```
Prelude> :t putStrLn
putStrLn :: String -> IO ()
```

これらの関数を `>>=` でつなぐことができる
```
Prelude> getLine >>= readFile >>= putStrLn
helloworld.hs
main = putStrLn "Hello, World"
```
