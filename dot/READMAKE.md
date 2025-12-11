## Dot言語の使い方
Dot言語には専用のコンパイラが必要です．以下のコマンドを叩くことでDot言語のコンパイラをインストールすることができます．
**Mac版**
```bash
homebrew install graphviz
```
**WSL版**
```bash
sudo apt install doxygen graphviz
```
dot言語の記法については，以下のリンクを参照するか生成AIを用いて調べてください．
[Graphvizとdot言語でグラフを描く方法のまとめ](https://qiita.com/rubytomato@github/items/51779135bc4b77c8c20d)

また，指定したフォントで作業をしたいため，`.dot`ファイルには必ず以下の内容を記載してください．

```dot:hoge.dot
digraph G {
    #include "style.doth"
}
```

## フォントについて
フォントに関して，統一させたいため以下のフォントをダウンロードして活用してください．
https://fonts.google.com/specimen/RocknRoll+One

## コンパイル方法について
```dot:
#include "style.doth"
```
を用いている場合，通常のコンパイルである
```bash
dot -Tsvg hoge.dot -O
```

では，`style.doth`を読み取ってはもらえず，コンパイルエラーが吐かれてしまいます．そのため，代わりに以下のコマンドを叩くことで正常にコンパイルすることができます．

```bash
 cpp -P hoge.dot | dot -Tpng -O
 ```

しかし，この書き方は面倒くさいため，後述の `MakeFile` を用いることで簡単(？)にコンパイルすることができます．

## MakeFileについて

> [!WARNING]
> 注意点として，ソースコードに差分がない場合は，スルーされる仕様になっています．

特定のファイルのみをコンパイルしたい場合は，ファイル名(拡張子を除いた)を入力してください．以下は， `hoge.dot`の場合を示しています．
```bash
make hoge
```

また，全てのファイルをコンパイルしたい場合は，以下のようにmakeのみにすると全てコンパイルされます．
```bash
make
```