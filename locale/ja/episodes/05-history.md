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

We've been adding small changes at a time to `guacamole.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `guacamole.md`, adding yet another line.

```bash
$ nano guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
An ill-considered change
```

それでは、何が得られるか見てみましょう。

```bash
$ git diff HEAD guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index b36abfd..0848c8d 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -4,3 +4,4 @@
 * lime
 * salt
 ## Instructions
+An ill-considered change
```

これは、 `HEAD` を省略した場合 (試してみてください) に得られるものと同じです。  これの本当の利点は、以前のコミットを参照できることです。  それを行うには、`HEAD` より前のコミットを参照するために `~1` (「~」は「チルダ」、発音は [**til**-d_uh_]) を追加します。

```bash
$ git diff HEAD~1 guacamole.md
```

古いコミット間の違いを確認したい場合は、`git diff` を再度使用できますが、`HEAD~1`、`HEAD~2` などの表記を使用して、それらを参照するには下記を行います:

```bash
$ git diff HEAD~2 guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..b36abfd 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,6 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
```

`git diff` を使用して表示されるコミットと作業ディレクトリの _違い_ ではなく、古いコミットで行った変更だけでなくコミットメッセージも表示する `git show`を使用することもできます。

```bash
$ git show HEAD~2 guacamole.md
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Create a template for recipe

diff --git a/guacamole.md b/guacamole.md
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/guacamole.md
@@ -0,0 +1,3 @@
+# Guacamole
+## Ingredients
+## Instructions
```

このようにして、コミットのチェーンを構築できます。
チェーンの最新の終わりは `HEAD` と呼ばれます; `~` 表記を使用して以前のコミットを参照できるため、`HEAD~1` は「以前のコミット」を意味し、`HEAD~123` は現在の場所から123個前のコミットに戻ります。

`git log` および `git show` が表示する、数字と文字の長い文字列を使用してコミットを参照することもできます。
これらは一個一個の変更に対するユニークなIDであり、「ユニーク」は本当に唯一であることを意味します: どのコンピューターのどのファイルの変更の組み合わせに対しても、ユニークな40文字の ID があります。
最初のコミットにはID `f22b25e3233b4645dabd0d81e651fe074bd8e73b` が与えられたので、これを試してみましょう:

```bash
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,7 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
+An ill-considered change
```

これは正しい答えですが、ランダムな40文字の文字列を入力するのは面倒なので、Gitは最初の数文字だけを使えばよいようにしてくれています:

```bash
$ git diff f22b25e guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,7 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
+An ill-considered change
```

やりました! こんなわけで ファイルへの変更を保存して、何が変更されたかを確認できます。 では、どうすれば古いバージョンのものを復元できるでしょうか?
Let's suppose we change our mind about the last update to
`guacamole.md` (the "ill-considered change").

`git status` は、ファイルが変更されたことを示しますが、それらの変更はステージングされていません:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

`git restore`を使うと、元の状態に戻すことができます:

```bash
$ git restore guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
```

その名前から推測できるように、`git restore` はファイルの古いバージョンを復元します。
この場合、最後に保存されたコミットである `HEAD` に記録されたファイルのバージョンを復元することをGitに伝えています。
さらに遡りたい場合は、`-s`オプションとコミット Id を使うことができます:

```bash
$ git restore -s f22b25e guacamole.md
```

```bash
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
## Instructions
```

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")

```

Notice that the changes are not currently in the staging area, and have not been committed.
If we wished, we can put things back the way they were at the last commit by using `git restore` to overwrite
the working copy with the last committed version:

```bash
$ git restore guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
```

取り消したい変更の一個**前**のコミット番号を使う必要があることを覚えておくことが重要です。
よくある間違いは、破棄しようとしている変更を行ったコミットの番号を使用することです。
以下の例では、最新のコミットの前 (`HEAD~1`) 、すなわち`f22b25e`から状態を取得したいと考えています: このとき、`.` を使用することで、全てのファイルを対象にします。

![](fig/git-restore.svg){alt='以下の図は、git restoreを使って2つのファイルの前のバージョンを復元する方法を示しています'}

つまり、すべてをまとめると、Gitがどのように機能するかは次の漫画のようになります:

![https://figshare.com/articles/How\_Git\_works\_a\_cartoon/1328266](fig/git_staging.svg){alt='以下の図は、Gitの全体的なワークフローを示しています。ローカルでの変更は、git addを使ってステージングされ、git commitでローカルリポジトリに反映されます。また、git checkoutを使用することで、リポジトリから変更を元に戻すことができます。'}

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

1. `$ git restore`

2. `$ git restore data_cruncher.py`

3. `$ git restore -s HEAD~1 data_cruncher.py`

4. `$ git restore -s <unique ID of last commit> data_cruncher.py`

5. 2番と4番の両方

:::::::::::::::  solution

## 解答

答えは (5) - 2 と 4 の両方です。

`restore` コマンドはリポジトリからファイルを復元し、作業ディレクトリのファイルを上書きします。 2番と4番のコマンドはどちらもリ_ポジトリにある_ `data_cruncher.py` の_最新バージョン_を復元します。 2番は `HEAD` を使って_最新バージョン_を指定し、4番は `HEAD` と同じ意味の最後のコミットIDを使います。

3番のコマンドは、`data_cruncher.py` の `HEAD` _より前_のコミットから復元するため、今回の目的とは異なります。

1番はエラーになります。 復元するファイルを指定する必要があります。 すべてのファイルを復元したい場合は、`git restore .` を使用するべきです。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## コミットを戻すことについて

ジェニファーは同僚とPythonスクリプトで共同作業を行っており、グループのリポジトリへの彼女の最新のコミットが間違っていることに気付き、それを元に戻したいと思っています。  ジェニファーは、グループリポジトリのみんなが正しい変更を取得できるように、正しく元に戻す必要があります。  ジェニファーは、グループリポジトリのみんなが正しい変更を取得できるように、正しく元に戻す必要があります。 `git revert [wrong commit ID]` は、ジェニファーの誤ったコミットを元に戻す新しいコミットを作ります。

`git revert` は `git restore -s [commit ID] .` とは異なります。`git restore` は、ローカルリポジトリ内でまだコミットされていないファイルを以前の状態に戻すために使用されるのに対して、`git revert` はローカルリポジトリおよびプロジェクトリポジトリにコミットされた変更を取り消すために使用されます。

以下は、ジェニファーが`git revert`を使用するための正しい手順と説明ですが、不足しているコマンドは何でしょうか？

1. `________ # コミットIDを見つけるために、プロジェクトのgitの 履歴を見ます`

2. そのIDをコピーします (IDの最初の数文字は例えば 0b1d055)。

3. `git revert [commit ID]`

4. 新しいコミットメッセージを入力します。

5. Save and close.

:::::::::::::::  solution

## 解答

`git log` コマンドは、コミットIDを含むプロジェクトの履歴を一覧表示します。

`git show HEAD` は、最新のコミットで行われた変更を表示し、コミットIDも確認できます。ただし、ジェニファーはこれが正しいコミットであることを再確認し、他の人がリポジトリに変更をコミットしていないか確認する必要があります。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ワークフローと履歴の理解

最後のコマンドの出力は何でしょうか？

```bash
$ cd recipes
$ echo "I like tomatoes, therefore I like ketchup" > ketchup.md
$ git add ketchup.md
$ echo "ketchup enhances pasta dishes" >> ketchup.md
$ git commit -m "My opinions about the red sauce"
$ git restore ketchup.md
$ cat ketchup.md # this will print the content of ketchup.md on screen
```

1. ```output
   ketchup enhances pasta dishes
   ```
2. ```output
   I like tomatoes, therefore I like ketchup
   ```
3. ```output
   I like tomatoes, therefore I like ketchup
   ketchup enhances pasta dishes
   ```
4. ```output
   Error because you have changed ketchup.md without committing the changes
   ```

:::::::::::::::  solution

## Solution

The answer is 2.

The changes to the file from the second `echo` command are only applied to the working copy,
not the version in the staging area.
The command `git add ketchup.md` places the current version of `ketchup.md` into the staging area.

So, when `git commit -m "My opinions about the red sauce"` is executed,
the version of `ketchup.md` committed to the repository is the one from the staging area and
has only one line.

At this time, the working copy still has the second line (and

`git status` will show that the file is modified). However, `git restore ketchup.md`
replaces the working copy with the most recently committed version of `ketchup.md`.
So, `cat ketchup.md` will output

```output
I like tomatoes, therefore I like ketchup
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Checking Understanding of `git diff`

Consider this command: `git diff HEAD~9 guacamole.md`. What do you predict this command
will do if you execute it? What happens when you do execute it? Why?

Try another command, `git diff [ID] guacamole.md`, where [ID] is replaced with
the unique identifier for your most recent commit. What do you think will happen,
and what does happen?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Getting Rid of Staged Changes

`git restore` can be used to restore a previous commit when unstaged changes have
been made, but will it also work for changes that have been staged but not committed?
Make a change to `guacamole.md`, add that change using `git add`,
then use `git restore` to see if you can remove your change.

:::::::::::::::  solution

## Solution

After adding a change, `git restore` can not be used directly.
Let's look at the output of `git status`:

```output
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   guacamole.md

```

Note that if you don't have the same output
you may either have forgotten to change the file,
or you have added it _and_ committed it.

Using the command `git restore guacamole.md` now does not give an error,
but it does not restore the file either.
Git helpfully tells us that we need to use `git restore --staged` first
to unstage the file:

```bash
$ git restore --staged guacamole.md
```

Now, `git status` gives us:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git git restore <file>..." to discard changes in working directory)
        modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

This means we can now use `git restore` to restore the file
to the previous commit:

```bash
$ git restore guacamole.md
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explore and Summarize Histories

Exploring history is an important part of Git, and often it is a challenge to find
the right commit ID, especially if the commit is from several months ago.

Imagine the `recipes` project has more than 50 files.
You would like to find a commit that modifies some specific text in `guacamole.md`.
When you type `git log`, a very long list appeared.
How can you narrow down the search?

Recall that the `git diff` command allows us to explore one specific file,
e.g., `git diff guacamole.md`. We can apply a similar idea here.

```bash
$ git log guacamole.md
```

Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history
for you.
Is it possible to combine both? Let's try the following:

```bash
$ git log --patch guacamole.md
```

You should get a long list of output, and you should be able to see both commit messages and
the difference between each commit.

Question: What does the following command do?

```bash
$ git log --patch HEAD~9 *.md
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` displays differences between commits.
- `git restore` recovers old versions of files.

::::::::::::::::::::::::::::::::::::::::::::::::::
