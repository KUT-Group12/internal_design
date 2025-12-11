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
> 注意点として，ソースコードに差分がない場合は，スルーされる仕様になっています．また，ファイル指定の際は拡張子を除いたベース名(パス込み)で指定してください．


特定のファイルのSVGだけ欲しいときは以下のようにしてください．
```bash
make svg sato/hoge
```

PNG だけ欲しいときについても以下のようにしてください．
```bash
make png sato/hoge
```

（svg や png を前に置くと、その形式だけが生成されるよう Makefile が自動的に切り替わります）

また，全dotファイルのSVGファイルを作成したい場合は，以下のようにコマンドを叩くと生成することができます．
```bash
make svg
```
PNGも同様にすることができます．
```bash
make png
```
最後に，全dotファイルにおいてSVGもPNGも両方とも生成したい場合は，
```bash
make
```
とすることで生成することができます．