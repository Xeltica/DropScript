# DropScript

DropScriptは、開発中のゲームに採用するイベント定義用スクリプト言語です。

イベント定義を目的としたドメイン固有言語(DSL)であるため、持ちうる機能はそれに特化したものとなりますが、簡易な制御構造を持することからその他の目的でも利用できるかもしれません。

## 進捗

- [x] 字句解析
- [ ] 構文解析
- [ ] インタプリタ
- 言語機能
  - [ ] メッセージ
  - [ ] コマンド呼び出し
  - [ ] メタ文字
  - [ ] 変数
  - [ ] 定数
  - [ ] 分岐
  - [ ] 繰り返し
  - [ ] コマンド定義
  - [ ] 条件式
  - [ ]

## DropScript の構文

### コメント

`#` 以降はコメントと見なされ無視される。

### メッセージ

普通にテキストを書いたらそれがセリフとしてメッセージボックスに表示されます。どのメッセージボックスを出力先とするかは、後述するコマンドで指定します。

例:
```
＊おはよう！　今日も　ビシバシ\n働いて　もらうぞ！
```

#### メタ文字

`\`から始まる特定の文字列は特別な意味を持ちます。強制改行やプロンプト表示、装飾、時間制御などを実現するために使われます。

メタ文字はバックスラッシュ(`\`)から始まります。環境によっては円記号(¥)で表示されますが、DropScriptはどちらも受容します。

一部をを除くメタ文字は、メッセージ以外では無視されます。

|メタ文字|意味|備考|
|-------|----|----|
|\n     |改行||
|\b     |1文字削除します。||
|\c     |全文字削除します||
|\w     |指定したミリ秒時間待機します。|整数値を続けて入れる必要があります|
|\p     |プロンプトを表示します||
|\C     |色を指定します|続いて色コードを指定します。0からfの16進数値|

### コマンド

コマンドを用いるとゲームを制御できます。セリフ表示も内部的にはコマンドです。

**例**

```
+playbgm legend.ogg
+fade out, 3.5
```

### 変数

変数を使うこともできます。変数の正規表現は`$\{[a-zA-Z_][a-zA-Z0-9_]*\}`です。{}の中には変数名が入ります。

いくつかの組み込み変数がある他、自分で変数を定義することもできます。

変数を定義する場合は、変数定義文を用います。以下のようにします。

**例**
```
# 変数定義
@var name = 太郎

# 定数定義
@const name2 = 次郎

俺は　${name}って　言うんだ。\nこいつは　弟の　${name2}！
```

定数は変数と異なり、再代入しようとするとエラーになります。また、変数を定数として再定義することでもエラーとなります。

### 分岐

```
@if 条件式
  文
@else
  文
@end
```

### 繰り返し

```
@for i, 0, 10
  文
@end
```

```
@while 条件式
  文
@end
```

### コマンド定義

```
@def コマンド名, 仮引数...
  文
@end
```

### メッセージキー

ローカライズ用のメッセージプロパティのキーを代わりに指定する構文。メッセージプロパティはDropScriptのプログラムとして解釈されるため、通常のDropScript構文をそのまま含めることができるようになっています。

例: `%event.castle.broken.1`

## DropScriptのデータ型

DropScriptには文字列型と真偽値型しかありません。真偽値型は条件式、フラグ、`${true}` `${false}` 組み込み変数で用いられます。その他は全て文字列型です。
