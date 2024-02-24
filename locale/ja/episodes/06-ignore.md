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
$ mkdir results
$ touch a.csv b.csv c.csv results/a.out results/b.out
```

そして Git が何と言うか見てみましょう：

```bash
$ git status
```

```output
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.csv
	b.csv
	c.csv
	results/

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
*.csv
results/
```

入力したパターンは、 Git に `.dat` で終わるファイル名と`results` ディレクトリ内にあるファイルを無視するように指示しています。
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
$ git commit -m "Ignore data files and the results folder"
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

`.gitignore` を作った事によって、間違えて不要なファイルをリポジトリに追加する事を防ぐことができます：

```bash
$ git add a.csv
```

```output
The following paths are ignored by one of your .gitignore files:
a.csv
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

        a.csv
        b.csv
        c.csv
        results/

nothing to commit, working tree clean
```

:::::::::::::::::::::::::::::::::::::::  challenge

## 埋もれた（ネストされた）ファイルを無視する

以下のようなディレクトリ構造があるとします：

```bash
results/data
results/plots
```

`results/data` ではなく、`results/plots` のみを無視するにはどうすればいいのでしょう？

:::::::::::::::  solution

## 解答

`results/plots` 内のファイルのみを無視するのであれば、`.gitignore` に `/plots/` サブフォルダを無視するように.gitignore に以下の文を加えれば解決できます：

```output
results/plots/
```

この行によって、`results/plots`の内容だけが無視され、`results/data`の内容は無視されません。

様々なプログラミングの問題と同様に、この無視ルールが守られるようにする回答方法はいくつかありま。
「ネストされたファイルを無視する：バリエーション」の演習は、わずかに異なるディレクトリ構造を持っており、別の解決策を提示しています。「ネストされたファイルを無視する：バリエーション」の演習は、わずかに異なるディレクトリ構造を持っており、別の解決策を提示しています。
Further, the discussion page has more detail on ignore rules.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Including Specific Files

`final.csv`以外の、ルートディレクトリ内にある他の `.data` ファイルを全て無視したい場合はどうすればいいのでしょう？
ヒント： `!` （感嘆符）が何をするのか調べてみましょう。

:::::::::::::::  solution

## 解答

以下二文を .gitignore に加えましょう：

```output
*.data           # 全ての data ファイルを無視する
!final.data      # final.data は対象から除外する
```

感嘆符は、無視してあったファイルを対象から外します。

このレッスンで`.csv`ファイルをコミットしたことがあるので、この新しいルールでは無視されません。 Only future additions
of `.csv` files added to the root directory will be ignored.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ignoring Nested Files: Variation

Given a directory structure that looks similar to the earlier Nested Files
exercise, but with a slightly different directory structure:

```bash
results/data
results/images
results/plots
results/analysis
```

How would you ignore all of the contents in the results folder, but not `results/data`?

Hint: think a bit about how you created an exception with the `!` operator
before.

:::::::::::::::  solution

## 解答

If you want to ignore the contents of
`results/` but not those of `results/data/`, you can change your `.gitignore` to ignore
the contents of results folder, but create an exception for the contents of the
`results/data` subfolder. Your .gitignore would look like this:

```output
results/*               # ignore everything in results folder
!results/data/          # do not ignore results/data/ contents
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ディレクトリ内の全てのデータファイルを無視する

空の.gitignoreファイルがあり、以下のようなディレクトリ構造があるとします：

```bash
results/data/position/gps/a.csv
results/data/position/gps/b.csv
results/data/position/gps/c.csv
results/data/position/gps/info.txt
results/plots
```

`result/data/position/gps` 内にある全ての `.data` ファイルを無視する一番短い`.gitignore`ルールは何でしょう？ `info.txt` ファイルは無視しないでください。

:::::::::::::::  solution

## 解答

`results/data/position/gps/*.data` を使えば `results/data/position/gps` 内にある全ての `.data` ファイルを無視できます。
`results/data/position/gps/info.txt` ファイルは無視されません。

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

- `.gitignore` で無視するファイルを指定する

::::::::::::::::::::::::::::::::::::::::::::::::::
