---
title: ファイルを無視する
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Git で追跡したくないファイルを指定しましょう
- ファイルを無視する利点を理解しましょう

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Git で追跡したくないファイルを指定するにはどうすればよいですか？

::::::::::::::::::::::::::::::::::::::::::::::::::

Git に追跡して欲しくないファイル、例えばエディタが作成したバックアップファイルやデータ解析中に作られた中間ファイルなどは、どう対処すればいいのでしょう？
例として、いくつかファイルを作ってみましょう：

```bash
$ mkdir receipts
$ touch a.png b.png c.png receipts/a.jpg receipts/b.jpg
```

そして Git が何と言うか見てみましょう：

```bash
$ git status
```

```output
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.png
	b.png
	c.png
	receipts/

nothing added to commit but untracked files present (use "git add" to track)
```

これらのファイルをバージョンコントロールで保存するのはディスク容量の無駄になります。
さらに、これら全てが表示されると、本当に必要な変更点に集中できなくなってしまうかもしれないので、
Git にこれらのファイルを無視してもらいましょう。

これをするには、`.gitignore` というファイルをルートディレクトリに作ります：

```bash
$ nano .gitignore
$ cat .gitignore
```

```output
*.png
receipts/
```

These patterns tell Git to ignore any file whose name ends in `.png`
and everything in the `receipts` directory.
（Git がすでに追跡しているファイルは、引き続き追跡されます。）

このファイルを作った後`git status` の出力を見てみると、大分綺麗になっています：

```bash
$ git status
```

```output
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

Git は新しく作られた `.gitignore` ファイルしか表示していません。
このファイルは追跡しなくても良いかと思うでしょうが、リポジトリを共有する際に、他の人達も私達が無視したものを同じように無視したいでしょう。
なので、`.gitignore` を追加してコミットしましょう：

```bash
$ git add .gitignore
$ git commit -m "Ignore png files and the receipts folder."
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

`.gitignore` を作った事によって、間違えて不要なファイルをリポジトリに追加する事を防ぐことができます：

```bash
$ git add a.png
```

```output
The following paths are ignored by one of your .gitignore files:
a.png
Use -f if you really want to add them.
```

この設定を強制的に無視してファイルを追加するには、`git add -f` を使います。 例えば、`git add -f a.csv` と入力します。
もちろん、無視されたファイルの状況はいつでも見ることができます：

```bash
$ git status --ignored
```

```output
On branch main
Ignored files:
 (use "git add -f <file>..." to include in what will be committed)

        a.png
        b.png
        c.png
        receipts/

nothing to commit, working tree clean
```

:::::::::::::::::::::::::::::::::::::::  challenge

## 埋もれた（ネストされた）ファイルを無視する

以下のようなディレクトリ構造があるとします：

```bash
receipts/data
receipts/plots
```

How would you ignore only `receipts/plots` and not `receipts/data`?

:::::::::::::::  solution

## 解答

If you only want to ignore the contents of
`receipts/plots`, you can change your `.gitignore` to ignore
only the `/plots/` subfolder by adding the following line to
your .gitignore:

```output
receipts/plots/
```

This line will ensure only the contents of `receipts/plots` is ignored, and
not the contents of `receipts/data`.

様々なプログラミングの問題と同様に、この無視ルールが守られるようにする回答方法はいくつかありま。
「ネストされたファイルを無視する：バリエーション」の演習は、わずかに異なるディレクトリ構造を持っており、別の解決策を提示しています。「ネストされたファイルを無視する：バリエーション」の演習は、わずかに異なるディレクトリ構造を持っており、別の解決策を提示しています。
Further, the discussion page has more detail on ignore rules.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 無視の対象に特定のファイルを含める

How would you ignore all `.png` files in your root directory except for
`final.png`?
ヒント： `!` （感嘆符）が何をするのか調べてみましょう。

:::::::::::::::  solution

## 解答

以下二文を .gitignore に加えましょう：

```output
*.png           # ignore all png files
!final.png      # except final.png
```

感嘆符は、無視してあったファイルを対象から外します。

Note also that because you've previously committed `.png` files in this
lesson they will not be ignored with this new rule. Only future additions
of `.png` files added to the root directory will be ignored.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ignoring Nested Files: Variation

前の入れ子になったファイルの練習問題と同様のディレクトリ構造ですが、少し異なるディレクトリ構造になっているとしましょう：

```bash
receipts/data
receipts/images
receipts/plots
receipts/analysis
```

How would you ignore all of the contents in the receipts folder, but not `receipts/data`?

ヒント： 以前に `!` 演算子を使って例外を作った方法を少し考えてみてください。

:::::::::::::::  solution

## 解答

If you want to ignore the contents of
`receipts/` but not those of `receipts/data/`, you can change your `.gitignore` to ignore
the contents of receipts folder, but create an exception for the contents of the
`receipts/data` subfolder. あなたの .gitignore は次のようになるでしょう：

```output
receipts/*               # ignore everything in receipts folder
!receipts/data/          # do not ignore receipts/data/ contents
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ディレクトリ内の全てのデータファイルを無視する

空の.gitignoreファイルがあり、以下のようなディレクトリ構造があるとします：

```bash
receipts/data/market_position/gps/a.dat
receipts/data/market_position/gps/b.dat
receipts/data/market_position/gps/c.dat
receipts/data/market_position/gps/info.txt
receipts/plots
```

What's the shortest `.gitignore` rule you could write to ignore all `.dat`
files in `result/data/market_position/gps`? `info.txt` ファイルは無視しないでください。

:::::::::::::::  solution

## 解答

Appending `receipts/data/market_position/gps/*.dat` will match every file in `receipts/data/market_position/gps`
that ends with `.dat`.
The file `receipts/data/market_position/gps/info.txt` will not be ignored.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ignoring all data Files in the repository

Let us assume you have many `.csv` files in different subdirectories of your repository.
For example, you might have:

```bash
results/a.csv
data/experiment_1/b.csv
data/experiment_2/c.csv
data/experiment_2/variation_1/d.csv
```

How do you ignore all the `.csv` files, without explicitly listing the names of the corresponding folders?

:::::::::::::::  solution

## 解答

In the `.gitignore` file, write:

```output
**/*.csv
```

This will ignore all the `.csv` files, regardless of their position in the directory tree.
You can still include some specific exception with the exclamation point operator.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ルールの順番

以下の内容の `.gitignore` ファイルがあるとします：

```bash
*.csv
!*.csv
```

結果的に何が無視されるのでしょうか？

:::::::::::::::  solution

## 解答

感嘆符 `!` は無視してあったファイルを対象から除外する効果があります。
`!*.csv` は、その前に入力されている `.csv` ファイルを対象から外すので、全ての `.csv` ファイルは引き続き追跡されることになります。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ログファイル

仮に `log_01`、 `log_02`、 `log_03`、というように、中間的にログファイルを作成するスクリプトを書いたとします。
これらのログファイルは取っておきたいのですが、`git` で追跡したくありません。

1. `log_01`、 `log_02`、などのファイルを無視するためのルールを**一つだけ** `.gitignore` に入力してください。

2. 入力したパターン正常に動作しているか確認するために `log_01` などのファイルを作成してください。

3. 最終的に `log_01` ファイルがものすごく重要であることが分かりました。`.gitignore` を編集せずに、このファイルを追跡しているファイルに加えてください。

4. 隣の人と、追跡したくないファイルは他にどのようなものがあるのか、そして`.gitignore` に何を入力すればこれらのファイルを無視できるのかを話し合ってください。

:::::::::::::::  solution

## 解答

1. `log_*` もしくは `log*` を .gitignore に加えます。

2. `git add -f log_01` を使って `log_01` を追跡しましょう。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- The .gitignore file is a text file that tells Git which files to track and which to ignore in the repository.
- You can list specific files or folders to be ignored by Git, or you can include files that would normally be ignored.

::::::::::::::::::::::::::::::::::::::::::::::::::
