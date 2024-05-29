---
title: リポジトリの作成
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- ローカルのGitリポジトリを作成する。
- `.git` ディレクトリの目的を説明する。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Gitはどこに情報を格納しますか?

::::::::::::::::::::::::::::::::::::::::::::::::::

Gitの設定ができたら、
それを使い始めることができます。

火星に惑星着陸船を送ることが可能かどうかを調査しているウルフマンとドラキュラの話に戻りましょう。

![](fig/motivatingexample.png){alt='motivatingexample'}
[ウルフマン対ドラキュラ](https://www.deviantart.com/b-maze/art/Werewolf-vs-Dracula-124893530)
by [b-maze](https://www.deviantart.com/b-maze) / [Deviant Art](https://www.deviantart.com/).
[火星](https://en.wikipedia.org/wiki/File:OSIRIS_Mars_true_color.jpg) by European Space Agency /
[CC-BY-SA 3.0 IGO](https://creativecommons.org/licenses/by/3.0/deed.en).
[冥王星](https://commons.wikimedia.org/wiki/File:PIA19873-Pluto-NewHorizons-FlyingPastImage-20150714-transparent.png) /
Courtesy NASA/JPL-Caltech.
[ミイラ](https://commons.wikimedia.org/wiki/File:Mummy_icon_-_Noun_Project_4070.svg)
© Gilad Fried / [The Noun Project](https://thenounproject.com/) /
[CC BY 3.0](https://creativecommons.org/licenses/by/3.0/deed.en).
[月](https://commons.wikimedia.org/wiki/File:Lune_ico.png)
© Luc Viatour / [https://lucnix.be](https://lucnix.be/) /
[CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/deed.en).

まず、`Desktop`フォルダーに作業用のディレクトリを作成し、そのディレクトリに移動しましょう:

```bash
$ cd ~/Desktop
$ mkdir planets
$ cd planets
```

次に、Gitに`planets`を[リポジトリ](../learners/reference.md#repository)（Gitがファイルのバージョンを保存できる場所）にするように伝えます。

```bash
$ git init
```

重要なのは、`git init` はサブディレクトリとそのファイルを含むことができるリポジトリを作成するということです。サブディレクトリが最初から存在する場合でも、後から追加された場合でも、`planets` リポジトリの中に入れ子になった別のリポジトリを作成する必要はありません。 また、 `planets`ディレクトリの作成と、リポジトリとしての初期化はまったく別の処理であることに注意してください。

`ls` を使ってディレクトリの内容を表示すると、
何も変更されていないように見えます:

```bash
$ ls
```

ですが `-a` フラグを追加してすべてを表示すると、Git が `.git`という隠しディレクトリを `planets` の中に作ったことがわかります:

```bash
$ ls -a
```

```output
.	..	.git
```

Git はプロジェクトのディレクトリ内にあるすべてのファイルとサブディレクトリを含む、プロジェクトに関するすべての情報を格納するためにこの特別なサブディレクトリを使用します。
`.git` サブディレクトリを削除すると、プロジェクトの履歴を失うことになります。

次に、デフォルトのブランチを `main` という名前に変更します。
あなたの設定やgitのバージョンによっては、これが既にデフォルトになっているかもしれません。
この変更の詳細については、[「セットアップ」](02-setup.md#default-git-branch-naming) を参照してください。

```bash
$ git checkout -b main
```

```output
Switched to a new branch 'main'
```

プロジェクトのステータスをGitに問うことで、すべてが正しく設定されていることを確認できます:

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

使用している`git`のバージョンによって、出力の表現が少し異なるかもしれません。

:::::::::::::::::::::::::::::::::::::::  challenge

## Git リポジトリを作る場所

`planets` （すでに作成したプロジェクト）についての情報を追跡すると共に、 ドラキュラは moons についての情報も追跡したいと考えています。
ウルフマンの心配にもかかわらず、ドラキュラは次の一連のコマンドを使って、彼の `planets`プロジェクト内に `moons` プロジェクトを作ります:

```bash
$ cd ~/Desktop   # Desktop ディレクトリに戻る
$ cd planets     # すでに Git リポジトリである planets ディレクトリに移動する
$ ls -a          # .git サブディレクトリがまだ planets ディレクトリに存在することを確認する
$ mkdir moons    # サブディレクトリ planets/moons を作る
$ cd moons       # moons サブディレクトリに移動する
$ git init       # moons サブディレクトリをGitリポジトリにする
$ ls -a          # .git サブディレクトリが存在し新しいGitリポジトリが作られたと示していることを確認する
```

`git init` コマンドは、`moons` サブディレクトリ内で実行され、`moons` サブディレクトリに保存されているファイルを追跡するために必要でしょうか?

:::::::::::::::  solution

## 解答

いいえ。 ドラキュラは `moons` サブディレクトリを Git リポジトリにする必要はありません。`planets` リポジトリは、`planets` ディレクトリの下のすべてのファイル、サブディレクトリ、およびサブディレクトリファイルを追跡するからです。  従って、`moons` についてのすべての情報を追跡するのは、ドラキュラが `moons` サブディレクトリを`planets` ディレクトリに追加するだけで済みます。

それと、Git リポジトリが「入れ子」にされている場合、Gitリポジトリは互いに干渉する可能性があります：外側のリポジトリは内側のリポジトリのバージョン管理をしようとします。 したがって、新しいGitリポジトリはそれぞれ別のディレクトリに作るのがベストです。 ディレクトリに競合するリポジトリががないことを確認するには、`git status`の出力を確認します。 次のような場合は、上の方で示したように新しいリポジトリを作ることをお勧めします：

```bash
$ git status
```

```output
fatal: Not a git repository (or any of the parent directories): .git
```

:::::::::::::::::::::::::

## `git init` の間違いを修正する

ウルフマンはドラキュラに、「入れ子」状態になっているリポジトリがいかに冗長で、混乱を引き起こす可能性があるかを説明しました。 説明を聞いて、ドラキュラは「入れ子」状態のリポジトリを削除したいと思いました。 `moons`サブディレクトリの最後の`git init`を、ドラキュラはどうやって、元に戻すことができるのでしょうか？

:::::::::::::::  solution

## 解決策 （要注意！）

### 背景

Gitリポジトリからのファイルの削除は、慎重に行う必要があります。 しかし、特定のファイルを追跡するようにGitに指示する方法については、まだ学んでいません（次のエピソードで学びます）。 Gitによって追跡されていないファイルは、他の「普通の」ファイルと同じように、次のようにして簡単に削除できます：

```bash
$ rm filename
```

同様に、`rm -r dirname` または `rm -rf dirname` を使ってディレクトリを削除することができます。
この方法で削除されるファイルやフォルダがGitによって追跡されているなら、次のエピソードで見られるように、それらの削除が、追跡する必要がある別の変更になります。

### 解答

Gitはすべてのファイルを`.git`ディレクトリに保存します。
この小さなミスから立ち直るには、ドラキュラは`planets`ディレクトリの中から次のコマンドを実行して、moonsサブディレクトリの中の`.git`フォルダを削除すれば良い：

```bash
$ rm -rf moons/.git
```

しかし、気をつけてください！ 間違ったディレクトリでこのコマンドを実行すると、残しておきたいプロジェクトのGit履歴がすべて削除されてしまいます。
したがって、常に `pwd` コマンドを使用してカレントディレクトリを確認してください。

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` はリポジトリを初期化する。
- Gitはリポジトリデータのすべてを`.git`ディレクトリに格納する。

::::::::::::::::::::::::::::::::::::::::::::::::::
