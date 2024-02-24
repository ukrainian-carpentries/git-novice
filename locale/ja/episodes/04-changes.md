---
title: 変更内容の記録
teaching: 20
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- 一つ以上のファイルにおいて、「変更・追加・コミット」の作業を行いましょう。
- 「変更・追加・コミット」を行っている際、情報がどこに保管されているのか説明しましょう。
- わかりやすいコミットメッセージと、そうでないメッセージの違いを区別できるようになりましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- どうやって変更点を Git で記録出来ますか？
- どうやってリポジトリの状態をチェック出来ますか？
- 何を変更して、なぜ変えたのかをメモするにはどうすればよいですか？

::::::::::::::::::::::::::::::::::::::::::::::::::

まずはじめに、正しいディレクトリにいるかどうか確かめましょう。
`planets` ディレクトリに入っているはずです。

```bash
$ cd ~/Desktop/planets
```

赤い惑星（火星）が基地に最適かどうかについてのノートを書くために`mars.txt` というファイルを作成しましょう。
`nano` （もしくは好きなテキストエディタ）でファイルを編集しましょう。
以前 `core.editor` で設定したエディタとは別のエディタで編集しても大丈夫です。 ですが、新しくファイルを作成・編集するコマンドはエディタによって違うということを覚えておいてください（`nano` ではないかもしれません）。 テキストエディタについて復習したい方は、[Unix シェル](https://swcarpentry.github.io/shell-novice/) の [どのエディタ？](https://swcarpentry.github.io/shell-novice/03-create/) のレッスンを見てみてください。

```bash
$ nano mars.txt
```

以下の文を `mars.txt` に記入してください：

```output
Cold and dry, but everything is my favorite color
```

まず、listコマンド（`ls`）を実行して、ファイルが正しく作成されたことを確認しましょう：

```bash
$ ls
```

```output
mars.txt
```

これで `mars.txt` は一文だけ入ってる状態になりました。確認してみましょう：

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
```

プロジェクトの状態をもう一度確認してみると、
Git は新しいファイルがあることに気づきます：

```bash
$ git status
```

```output
On branch main

No commits yet

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	mars.txt

nothing added to commit but untracked files present (use "git add" to track)
```

"untracked files" （追跡されていないファイル）というメッセージは、
Git が追跡していないファイルがこのディレクトリに存在していることを意味します。
`git add` を使って、Git にこのファイルを追跡してもらいましょう：
`git add` を使って、Git にこのファイルを追跡してもらいましょう：

```bash
$ git add mars.txt
```

そして、正しく動作したか確認してみましょう：

```bash
$ git status
```

```output
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   mars.txt

```

これで Git は `mars.txt` を追跡するように設定することができました。
ですが、まだ変更点をコミットとして記録していません。
これをするには、もう一つコマンドを使う必要があります：

```bash
$ git commit -m "Start notes on Mars as a base"
```

```output
[main (root-commit) f22b25e] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
```

`git commit` を使うと、
Git は `git add` によって一時的に保存されたデータを
`.git` ディレクトリにコピー、そして永久保存します。
永久保存されたコピーを [コミット](../learners/reference.md#commit)
（または [リビジョン](../learners/reference.md#revision))）と呼び、`f22b25e` はコミットの省略されたIDです。 あなたのコミットには別のIDが使われているかもしれません。

`-m` フラグ（「メッセージ」の略）を使い、何を変えて、なぜ変えたのかが後からでも分かるように、簡潔で分かりやすいメッセージを書きましょう。
`-m` を使わずに `git commit` を使った場合、Git は `nano` （または `core.editor` として設定したエディタ）を開き、もっと長いメッセージを書くことができます。

[良いコミットメッセージ][commit-messages] は、どのような変更が行われたのかが分かる短い文（50字以下）
で始まります。 大抵のメッセージは、「このコミットを適用すると・・<commit message here>」という文が完全文になるように書かれます。
もっと詳しく書きたい場合は、このメッセージの後に改行してからノートを加えましょう。 ここには、なぜこのような変更をしたのか、この変更によって何が影響を受けるかなどといった事を書くといいでしょう。

今の状態で `git status` を走らせると：

```bash
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

といったように、最新の状態であることを教えてくれます。
つい先程、何をしたのかを見たい場合は `git log` を使って、 Git にプロジェクトの履歴を表示してもらいましょう：

```bash
$ git log
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
```

`git log` は、リポジトリに施された全てのコミットを最新のものから順に表示します。
各コミットには、省略されていないコミットID（以前 `git commit` で表示された省略IDと同じ字列で始まります）、コミットの著者、作成日時、そして、コミットが作られた時につけられたログメッセージが含まれています。

:::::::::::::::::::::::::::::::::::::::::  callout

## 変更点は何処にあるの？

ここで `ls` を使用すると、`mars.txt` ファイル一つしかありません。
なぜかと言うと、Git はファイルの変更履歴を、以前触れた `.git` という特別なディレクトリに保存します。これをする事によって、ファイルシステムが散らからないようにし、うっかり古いバージョンのファイルを変更、または削除できないようにしています。

::::::::::::::::::::::::::::::::::::::::::::::::::

それでは、ドラキュラがファイルに新しい情報を加えたとしましょう。
（以前と同様に、`nano` を使い、`cat` でファイルの中身を表示させます。違うエディタを使ってもいいし、`cat` を使わなくても構いません。）

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
```

`git status` を使うと、追跡しているファイルが変更されたことを知らせてくれます：

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

最後に表示された文が一番重要です："no changes added to commit" （「変更点はコミットに追加されていません」）。
私達はファイルを編集したのですが、まだ Git にこの変更点を保存したいということを（`git add` で）伝えていないし、それを（`git commit` で）保存もしていません。
というわけで、これらをやってみましょう。 変更点を保存する前に見直すのは、習慣づけると良いでしょう。 見直すには `git diff` を使いましょう。
これによって、今のファイルの状態と、一つ前までのファイルの状態との違いを確かめることができます：

```bash
$ git diff
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
```

すると、暗号のようなメッセージが出力されます。
これらのメッセージは、エディタや `patch` といったプログラムで使われる
一連のコマンドで、ファイルをどうやって再構成するのかが書かれています。
分かりやすいように、一文ずつ見てみましょう：

1. 最初の文は、Git が古いバージョンと新しいバージョンのファイルをUnix の`diff`コマンドと同じように、二つのファイルを比べていることを表しています。
2. 次に、Git が比べているファイルの正確なバージョンを表示しています。`df0654a` と `315bf3a` はコンピュータが作成した、各バージョン"につけられたIDです。
3. 3・4つ目の文章は、変更されているファイルの名前を表示しています。
4. 残りの文章が一番重要です。ここに、何が変わり、どの行が変更されたのかが記されています。
   特に、この`+` マークは、どこに文章が加えられたのかを表しています。

変更点を確認したら、コミットしましょう：

```bash
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

おっと、
初めに `git add` を使わなかったので、 Git はコミットさせてくれませんでした。
直しましょう：

```bash
$ git add mars.txt
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
```

```output
[main 34961b1] Add concerns about effects of Mars' moons on Wolfman
 1 file changed, 1 insertion(+)
```

Git は変更をコミットする前に、コミットしたいファイルを
追加するように要求してきます。 これによって、変更点をこまめに、そして論理的に分割して保存ができ、大きな変更をひとまとめに保存しなくてもよくなります。
例えば、論文にいくつか引用源を加えるとします。
これらの引用源、そして使われた参考文献の目録を、まだ書き終わっていない結論とは_別に_保存したい場合はどうすればいいのでしょう。

Git には、まだコミットされていない[changeset](../learners/reference.md#changeset)（一連の変更点）を一時的に保存・把握するために使われる、_staging area_（ステージング・エリア）と呼ばれる特殊な場所が存在します。

:::::::::::::::::::::::::::::::::::::::::  callout

## ステージングエリア

仮にGit を、プロジェクトの一生の間に起こった変更内容を「スナップショット」として保存するものとして考えると、`git add` は_何が_スナップショットに含まれる（ステージングエリアに入れる）のかを指定して、`git commit` は_実際にスナップショットを撮り_、コミットとして永久保存します。
`git commit` と入力した際にステージングエリアに何もなかった場合、Git は`git commit -a` もしくは `git commit --all` を入力するように言ってきます。このコマンドは、写真を撮る時の「全員集合！」のようなもので、全ての変更点を強制的にコミットできます。
ですが、大抵は、コミットしたい変更点のみをステージングエリアに入れるほうが良いでしょう。これによって、不要な変更点を間違えてコミットすることもありません。 （集合写真の例に例えると、メイクの不完全なエキストラがステージの上を横断しているのを一緒に撮ってしまった、みたいなものです。）
ですので、ステージングエリアにどの変更点を入れるかは自分で管理しましょう。さもないと、必要以上に「git コミットやり直す」で検索をかけることになるでしょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

![](fig/git-staging-area.svg){alt='Gitのステージング・エリア'}

それでは、ファイルの変更点がエディタからステージングエリア、そして長期保存に移る過程を見てみましょう。
初めに、新しい文章をファイルに加えましょう：

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
```

```bash
$ git diff
```

```output
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
```

いい感じです。
これでファイルの最後に新しく文章を足すことができました。
（左にある `+` マークで記されています。）
それでは変更点をステージングエリアに入れて
`git diff` がなにを表示するのかを見てみましょう：

```bash
$ git add mars.txt
$ git diff
```

何も表示されませんでした。Git が見た限りでは、保存したい変更点と今ディレクトリ内にあるファイルには、これといった違いは無いということになります。
ですが、こう入力すると：

```bash
$ git diff --staged
```

```output
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
```

以前コミットされた状態のファイルとステージングエリアにある変更点の違いが表示されます。
変更内容を保存しましょう：

```bash
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
```

```output
[main 005937f] Discuss concerns about Mars' climate for Mummy
 1 file changed, 1 insertion(+)
```

状態をチェックしましょう：

```bash
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

そして、今までの変更履歴も見てみましょう：

```bash
$ git log
```

```output
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Discuss concerns about Mars' climate for Mummy

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add concerns about effects of Mars' moons on Wolfman

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
```

:::::::::::::::::::::::::::::::::::::::::  callout

## 文字ごとの違いを表示する

時折、例えばテキストドキュメントなど、列ごとの違いを表示するのは大雑把すぎる場合があります。 こういう時は、`git diff` の `--color-words` オプションを使えば、色で文字ごとの違いを表示してくれるので、大変便利です。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ログをめくる

`git log` の出力がスクリーンよりも長いと、`git` はスクリーンに収まるようにログを分割して表示するプログラムを使います。
この "pager" （ページャ）というプログラムが開くと、一番下の列がプロンプトではなく、`:` になります。

- ページャを閉じるには <kbd>Q</kbd> を押してください。
- 次のページを表示するには <kbd>Spacebar</kbd>を押してください。
- ある `<文字>` を全ページの中から検索する時は、<kbd>/</kbd> を押し、`<文字>` を入力してください。 <kbd>N</kbd>を押すと次の一致場所に行けます。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ログの出力を制限する

`git log` の出力がスクリーン全体を埋めないようにするために、Gitが表示するコミットの数を `-N` で変えることができます。この `N`は、表示したいコミットの数を表しています。 例えば、最新のコミットの情報だけを表示したい場合は、こう入力します：

```bash
$ git log -1
```

```output
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

   Discuss concerns about Mars' climate for Mummy
```

`--oneline` オプションを使うことによって、表示する情報を制限する事ができます：

```bash
$ git log --oneline
```

```output
005937f (HEAD -> main) Discuss concerns about Mars' climate for Mummy
34961b1 Add concerns about effects of Mars' moons on Wolfman
f22b25e Start notes on Mars as a base
```

`--oneline` オプションを他のオプションと組み合わせることもできます。 便利な組み合わせの一つとして、 `--graph` を追加すると、コミット履歴をテキストベースのグラフとして表示し、どのコミットが現在の `HEAD`、現在のブランチ `main`、あるいは[その他の Git リファレンス][git-references]に関連しているかを示すことができます：

```bash
$ git log --oneline --graph
```

```output
* 005937f (HEAD -> main) Discuss concerns about Mars' climate for Mummy
* 34961b1 Add concerns about effects of Mars' moons on Wolfman
* f22b25e Start notes on Mars as a base
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ディレクトリ

Git でディレクトリを使用する際は、以下の二点を覚えておきましょう。

1. Git はディレクトリ単体を追跡することはなく、ディレクトリ内のファイルのみを追跡します。
   自分で試してみてください：

```bash
$ mkdir spaceships
$ git status
$ git add spaceships
$ git status
```

注目してほしいのは、新しく作った `directory` ディレクトリは `git add` でリポジトリに追加したにも関わらず、追跡されてないファイルのリストに表示されていません。 たまに `.gitkeep` ファイルが空のディレクトリ内にあるのは、このためです。 `.gitignore`とは違って、このファイルは特別でも何でもなく、空のディレクトリを Git のリポジトリに追加、そして追跡させるためだけに置いてあるだけです。 なので、別の名前のファイルでも同じことができます。

2. Git リポジトリ内でディレクトリを作成し、複数のファイルをそのディレクトリに入れたい場合、ディレクトリ内のファイルをひとまとめに追加する事ができます：

```bash
git add <directory-with-files>
```

自分で試してみてください：

```bash
$ touch spaceships/apollo-11 spaceships/sputnik-1
$ git status
$ git add spaceships
$ git status
```

次に進む前に、これらの変更をコミットしましょう。

```bash
$ git commit -m "Add some initial thoughts on spaceships"
```

::::::::::::::::::::::::::::::::::::::::::::::::::

まとめると、変更内容をリポジトリに追加したい時は`git add` で変更点をステージングエリアに移してから、`git commit` でステージングエリアの変更点をリポジトリに保存します：

![](fig/git-committing.svg){alt='git commitのワークフロー'}

:::::::::::::::::::::::::::::::::::::::  challenge

## コミットメッセージを決める

以下のコミットメッセージの内、最後の `mars.txt` のコミットに最適なメッセージはどれでしょう？

1. "Changes"
2. "Added line 'But the Mummy will appreciate the lack of humidity' to mars.txt"
3. "Discuss effects of Mars' climate on the Mummy"

:::::::::::::::  solution

## 解答

１つ目のメッセージは短すぎてコミットの内容が何なのかわかりにくいです。２つ目は`git diff`で何が変わったのかが分かるので、長い割にはあまり意味がありません。３つ目は、短く、簡潔で、分かりやすいメッセージです。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Git に変更点をコミットする

以下の内、`myfile.txt` を Git リポジトリに保存するためのコマンドはどれでしょう？

1. ```bash
   ```

$ git commit -m "my recent changes"

````
2. ```bash
$ git init myfile.txt
$ git commit -m "my recent changes"
````

3. ```bash
   ```

$ git add myfile.txt
$ git commit -m "my recent changes"

````
4. ```bash
$ git commit -m myfile.txt "my recent changes"
````

:::::::::::::::  solution

## 解答

1. ファイルがステージングエリアにない限り、コミットできません。

2. 新しくリポジトリを作ろうとします。

3. 正しい回答です。まずファイルをステージングエリアに追加し、それからコミットします。

4. "my recent changes" というファイルを myfile.txt というメッセージでコミットしようとします。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 複数のファイルをコミットする

一つのコミットに対し、ステージングエリアは複数のファイルの変更点を保持する事ができます。

1. `mars.txt` に、火星ではなく金星に基地を作ることにしたという文章を加えましょう。
2. 新しく `venus.txt` というファイルを作り、金星に基地を置く決断についての感想を書きましょう。
3. 二つのファイルの変更内容をステージングエリアに加えて、コミットしましょう。

:::::::::::::::  solution

## 解答

以下の`cat mars.txt`の出力は、この課題で追加された内容のみを反映しています。 出力は異なる場合があります。

まずは `mars.txt` と `venus.txt` を編集しましょう：

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Maybe I should start with a base on Venus.
```

```bash
$ nano venus.txt
$ cat venus.txt
```

```output
Venus is a nice planet and I definitely should consider it as a base.
```

これで二つのファイルをステージングエリアに追加することができます。 二つのファイルを一気に追加するには：

```bash
$ git add mars.txt venus.txt
```

一つずつ追加するには：

```bash
$ git add mars.txt
$ git add venus.txt
```

これでファイルをコミットする準備ができました。 `git status` でチェックしましょう。 コミットをするには：

```bash
$ git commit -m "Write plans to start a base on Venus"
```

```output
[main cc127c2]
 Write plans to start a base on Venus
 2 files changed, 2 insertions(+)
 create mode 100644 venus.txt
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## `bio` リポジトリ

- `bio` という Git リポジトリ新しく作りましょう。
- `me.txt` というファイルに自分について３文書いて、変更点をコミットしてください。
- すでに書いた文章の内、ひとつだけ編集して、更にもう一文加えてください。
- 編集した後の状態とその前の違いを表示してください。

:::::::::::::::  solution

## 解答

必要であれば、`planets` から出ましょう：

```bash
$ cd ..
```

新しく `bio` というディレクトリを作り、中に移動しましょう：

```bash
$ mkdir bio
$ cd bio
```

リポジトリを作りましょう：

```bash
$ git init
```

`nano` か他のテキストエディタで `me.txt` を作りましょう。
作ったら、変更点を追加してコミットしてください：

```bash
$ git add me.txt
$ git commit -m "Add biography file" 
```

指示通りにファイルを編集してください（一文だけ変えて、もう一文足す）。
オリジナルと編集後のファイルの違いを表示させるために、`git diff` を使います：

```bash
$ git diff me.txt
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

[commit-messages]: https://chris.beams.io/posts/git-commit/

[git-references]: https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-Git%E3%81%AE%E5%8F%82%E7%85%A7

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git status` はリポジトリの状態を表示する。
- ファイルはプロジェクトの作業ディレクトリ、ステージング・エリア（次のコミットに含まれる変更点が蓄積される場所）、そしてローカル・リポジトリ（コミットが永久に記録される場所）に保存される。
- `git add` はファイルをステージング・エリアに移動させる。
- `git commit` はステージされた内容をローカル・リポジトリに保存する。
- コミットメッセージは、変更点がわかりやすいように書きましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::
