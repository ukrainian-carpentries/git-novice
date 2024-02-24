---
title: 履歴の探索
teaching: 25
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- リポジトリのHEADとは何か、またその使い方を説明出来るようになりましょう。
- Gitのコミット番号を特定して使ってみましょう。
- 追跡調査されるファイルのいろいろなバージョンを比較してみましょう。
- ファイルの古いバージョンを復元してみましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ファイルの古いバージョンを復元するにはどうすればよいでしょうか?
- 変更内容を再調査するにはどうすればよいでしょうか?
- ファイルの古いバージョンを復元するにはどうすればよいでしょうか?

::::::::::::::::::::::::::::::::::::::::::::::::::

前のレッスンで見たように、コミットを識別子で参照できます。  識別子 `HEAD` を使うことで作業ディレクトリの _最新のコミット_ を参照できます。

`mars.txt` に一度に1行ずつ追加しているので、進展を見て確認することは簡単です。それでは `HEAD` を使ってそれを行ってみましょう。  始める前に、 `mars.txt` にもう1行加えることで変更を加えてみましょう。

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
An ill-considered change
```

それでは、何が得られるか見てみましょう。

```bash
$ git diff HEAD mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index b36abfd..0848c8d 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,3 +1,4 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
 But the Mummy will appreciate the lack of humidity
+An ill-considered change.
```

これは、 `HEAD` を省略した場合 (試してみてください) に得られるものと同じです。  これの本当の利点は、以前のコミットを参照できることです。  それを行うには、`HEAD` より前のコミットを参照するために `~1` (「\~」は「チルダ」、発音は [**til**-d_uh_]) を追加します。

```bash
$ git diff HEAD~1 mars.txt
```

古いコミット間の違いを確認したい場合は、`git diff` を再度使用できますが、`HEAD~1`、`HEAD~2` などの表記を使用して、それらを参照するには下記を行います:

```bash
$ git diff HEAD~3 mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

`git diff` を使用して表示されるコミットと作業ディレクトリの _違い_ ではなく、古いコミットで行った変更だけでなくコミットメッセージも表示する `git show`を使用することもできます。

```bash
$ git show HEAD~3 mars.txt
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base

diff --git a/mars.txt b/mars.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/mars.txt
@@ -0,0 +1 @@
+Cold and dry, but everything is my favorite color
```

このようにして、コミットのチェーンを構築できます。
チェーンの最新の終わりは `HEAD` と呼ばれます; `~` 表記を使用して以前のコミットを参照できるため、`HEAD~1` は「以前のコミット」を意味し、`HEAD~123` は現在の場所から123個前のコミットに戻ります。

`git log` が表示する、数字と文字の長い文字列を使用してコミットを参照することもできます。
これらは一個一個の変更に対するユニークなIDであり、「ユニーク」は本当に唯一であることを意味します: どのコンピューターのどのファイルの変更の組み合わせに対しても、ユニークな40文字の ID があります。
最初のコミットにはID `f22b25e3233b4645dabd0d81e651fe074bd8e73b` が与えられたので、これを試してみましょう:

```bash
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

これは正しい答えですが、ランダムな40文字の文字列を入力するのは面倒なので、Gitは最初の数文字だけを使えばよいようにしてくれています:

```bash
$ git diff f22b25e mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

やりました! こんなわけで ファイルへの変更を保存して、何が変更されたかを確認できます。 では、どうすれば古いバージョンのものを復元できるでしょうか?
`mars.txt`への最後の更新（「熟考を欠いた変更」）について気が変わったとしましょう。

`git status` は、ファイルが変更されたことを示しますが、それらの変更はステージングされていません:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git checkout`を使うと、元の状態に戻すことができます:

```bash
$ git checkout HEAD mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
```

その名前から推測できるように、`git checkout` はファイルの古いバージョンをチェックアウト (つまり、復元) します。
この場合、最後に保存されたコミットである `HEAD` に記録されたファイルのバージョン を復元することをGitに伝えています。
さらに戻りたい場合は、代わりにコミット Id を使うことができます:

```bash
$ git checkout f22b25e mars.txt
```

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
```

```bash
$ git status
```

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   mars.txt

```

変更はステージング領域にあることに注意してください。
繰り返しますが、`git checkout` を使うと、元の状態に戻すことができます:

```bash
$ git checkout HEAD mars.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## HEAD を見失わないようにしましょう

上記では下記を使いました

```bash
$ git checkout f22b25e mars.txt
```

`mars.txt` をコミット `f22b25e` 後の状態に戻すためにです。 しかし、気をつけてください！
コマンド `checkout` には他の重要な機能があり、入力が正確でない場合、Gitはあなたの意図を誤解する可能性があります。 たとえば、前のコマンドで `mars.txt` を忘れた場合です。

```bash
$ git checkout f22b25e
```

```error
Note: checking out 'f22b25e'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

 git checkout -b <new-branch-name>

HEAD is now at f22b25e Start notes on Mars as a base
```

ここでの「HEADが切り離された」状態は「見れるが触ってはいけない」ようなものなので、この状態で変更を加えないでください。
リポジトリの過去の状態を調査した後、`git checkout main` で `HEAD`を再接続してください。

::::::::::::::::::::::::::::::::::::::::::::::::::

取り消したい変更の一個**前**のコミット番号を使う必要があることを覚えておくことが重要です。
よくある間違いは、破棄しようとしている変更を行ったコミットの番号を使用することです。
以下の例では、最新のコミットの前 (`HEAD~1`) 、すなわち`f22b25e`から状態を取得したいと考えています:

![](fig/git-checkout.svg){alt='Git Checkout'}

つまり、すべてをまとめると、Gitがどのように機能するかは次の漫画のようになります:

![https://figshare.com/articles/How\_Git\_works\_a\_cartoon/1328266](fig/git_staging.svg)

:::::::::::::::::::::::::::::::::::::::::  callout

## よくあるケースの簡単化

`git status` の出力を注意深く読むと、次のヒントを含んでいることが分かります:

```output
(use "git checkout -- <file>..." to discard changes in working directory)
```

それが言っているように、バージョン識別子のない `git checkout` はファイルを`HEAD`に保存された状態に復元します。
二重のダッシュ `--`はコマンドから復元されるファイルの名前を区別するために必要です：それがないと、Git はファイルの名前をコミット Id として使用しようとします。

::::::::::::::::::::::::::::::::::::::::::::::::::

ファイルを1つずつ元に戻すことができるという事実は
人々が研究を整理する方法を変えることがあります。
すべての変更が1つの大きなドキュメントに含まれている場合、
後で結論に加えられた変更を元に戻さずに、
序論への変更を元に戻すことは困難です(不可能ではありませんが)。
一方、序論と結論を別々のファイルに保存すると、時間を前後に移動するのがはるかに簡単になります。

:::::::::::::::::::::::::::::::::::::::  challenge

## ファイルの古いバージョンの復元

ジェニファーは、数週間取り組んできたPythonスクリプトに変更を加えました。そして今朝行った変更により、スクリプトが "壊れ"、動作しなくなりました。 彼女はそれを修正しようとして約1時間費やしましたが、うまく機能しません...

幸い、彼女はGitを使用してプロジェクトのバージョンを追跡していました! 以下のどのコマンドで、`data_cruncher.py` と呼ばれるPythonスクリプトの最後にコミットされたバージョンを復元できるでしょうか？

1. `$ git checkout HEAD`

2. `$ git checkout HEAD data_cruncher.py`

3. `$ git checkout HEAD~1 data_cruncher.py`

4. `$ git checkout <unique ID of last commit> data_cruncher.py`

5. Both 2 and 4

:::::::::::::::  solution

## 解答

答えは (5) - 2 と 4 の両方です。

The `checkout` command restores files from the repository, overwriting the files in your working
directory. Answers 2 and 4 both restore the _latest_ version _in the repository_ of the file
`data_cruncher.py`. Answer 2 uses `HEAD` to indicate the _latest_, whereas answer 4 uses the
unique ID of the last commit, which is what `HEAD` means.

Answer 3 gets the version of `data_cruncher.py` from the commit _before_ `HEAD`, which is NOT
what we wanted.

Answer 1 can be dangerous! Without a filename, `git checkout` will restore **all files**
in the current directory (and all directories below it) to their state at the commit specified.
This command will restore `data_cruncher.py` to the latest commit version, but it will also
restore _any other files that are changed_ to that version, erasing any changes you may
have made to those files!
As discussed above, you are left in a _detached_ `HEAD` state, and you don't want to be there.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## コミットを戻すことについて

ジェニファーは同僚とPythonスクリプトで共同作業を行っており、グループのリポジトリへの彼女の最新のコミットが間違っていることに気付き、それを元に戻したいと思っています。  ジェニファーは、グループリポジトリのみんなが正しい変更を取得できるように、正しく元に戻す必要があります。  ジェニファーは、グループリポジトリのみんなが正しい変更を取得できるように、正しく元に戻す必要があります。 `git revert [wrong commit ID]` は、ジェニファーの誤ったコミットを元に戻す新しいコミットを作ります。

従って`git revert`は`git checkout [commit ID]`とは異なります。なぜなら `checkout` はグループのリポジトリにはコミットされていないローカルの変更用のコマンドだからです。

以下は、ジェニファーが`git revert`を使用するための正しい手順と説明ですが、不足しているコマンドは何でしょうか？

1. `________ # コミットIDを見つけるために、プロジェクトのgitの 履歴を見ます`

2. そのIDをコピーします (IDの最初の数文字は例えば 0b1d055)。

3. `git revert [commit ID]`

4. 新しいコミットメッセージを入力します。

5. 保存して閉じます。

:::::::::::::::  solution

## 解答

The command `git log` lists project history with commit IDs.

The command `git show HEAD` shows changes made at the latest commit, and lists
the commit ID; however, Jennifer should double-check it is the correct commit, and no one
else has committed changes to the repository.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ワークフローと履歴の理解

最後のコマンドの出力は何でしょうか？

```bash
$ cd planets
$ echo "Venus is beautiful and full of love" > venus.txt
$ git add venus.txt
$ echo "Venus is too hot to be suitable as a base" >> venus.txt
$ git commit -m "Comment on Venus as an unsuitable base"
$ git checkout HEAD venus.txt
$ cat venus.txt #this will print the contents of venus.txt to the screen
```

1. ```output
   ```

Venus is too hot to be suitable as a base

````
2. ```output
Venus is beautiful and full of love
````

3. ```output
   ```

Venus is beautiful and full of love
Venus is too hot to be suitable as a base

````
4. ```output
Error because you have changed venus.txt without committing the changes
````

:::::::::::::::  solution

## 解答

答えは2です。

The command `git add venus.txt` places the current version of `venus.txt` into the staging area.
The changes to the file from the second `echo` command are only applied to the working copy,
not the version in the staging area.

So, when `git commit -m "Comment on Venus as an unsuitable base"` is executed,
the version of `venus.txt` committed to the repository is the one from the staging area and
has only one line.

At this time, the working copy still has the second line (and
`git status` will show that the file is modified). However, `git checkout HEAD venus.txt`
replaces the working copy with the most recently committed version of `venus.txt`.

So, `cat venus.txt` will output

```output
Venus is beautiful and full of love.
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## `git diff` の理解のチェック

このコマンドをよく考えてみてください: `git diff HEAD~9 mars.txt`。 このコマンドを実行したらどうなるだろうと予測しますか? 実行すると何が起こっていますか? またそれはなぜでしょうか?

別のコマンド `git diff [ID] mars.txt` を試しましょう、ここでの [ID] は最新のコミットのユニークな ID で置き換えられます。 あなたは何が起こるだろうと思いますか?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ステージされた変更の除去

`git checkout` は、ステージされていない変更があったときに 以前のコミットを復元するために使用できます。しかしそれはステージされているがコミットされていない変更に対しても機能するでしょうか?
`mars.txt` に変更を用意し、その変更を加え（`git add`を使い）、`git checkout` を使い変更を取り除くことができるかどうか確かめましょう。

:::::::::::::::  solution

## 解答

After adding a change, `git checkout` can not be used directly.
Let's look at the output of `git status`:

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   mars.txt

```

Note that if you don't have the same output
you may either have forgotten to change the file,
or you have added it _and_ committed it.

Using the command `git checkout -- mars.txt` now does not give an error,
but it does not restore the file either.
Git helpfully tells us that we need to use `git reset` first
to unstage the file:

```bash
$ git reset HEAD mars.txt
```

```output
Unstaged changes after reset:
M	mars.txt
```

Now, `git status` gives us:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

This means we can now use `git checkout` to restore the file
to the previous commit:

```bash
$ git checkout -- mars.txt
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 履歴を探索し、要約する

履歴の探索はgitの重要な要素であり、特にそのコミットが数ヶ月前のものである場合は、適切なコミットIDを見つけるのが難しいことがよくあります。

`planets` プロジェクトに50を超えるファイルがあると考えてください。
あなたは `mars.txt` 中の特定のテキストが変更されたコミットを見つけたいとします。
`git log` と入力すると、非常に長いリストが表示されました。
どうやって探す範囲を限定しますか?

`git diff` コマンドを使用すると、1つの特定のファイルを探索できることを思い出してください、 例えば、`git diff mars.txt` 。 ここでも同様のアイデアを適用できます。

```bash
$ git log mars.txt
```

Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history
for you.
Is it possible to combine both? Let's try the following:

```bash
$ git log --patch mars.txt
```

You should get a long list of output, and you should be able to see both commit messages and
the difference between each commit.

Question: What does the following command do?

```bash
$ git log --patch HEAD~9 *.txt
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` は、コミット間の違いを表示します。
- `git checkout` は、ファイルの古いバージョンを復元します。

::::::::::::::::::::::::::::::::::::::::::::::::::
