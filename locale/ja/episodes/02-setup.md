---
title: Gitの設定
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- コンピュータで初めて `git` を使うための設定が出来るようになりましょう。
- `--global` 設定フラグの意味を理解しましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Git を使うために必要な設定は何ですか？

::::::::::::::::::::::::::::::::::::::::::::::::::

Git を新しいパソコンで初めて使う場合、いくつかの設定を変更しなければなりません。 Git を始めるにあたって、私達が変更する設定をいくつか表記します：

- 名前とメールアドレス、
- 使用したいテキストエディタ、
- 以上の設定をグローバル設定として使う（つまり、全てのプロジェクトに反映させる）。

コマンドラインでは、Git コマンドは `git <動詞> <オプション>` と入力します。ここでの「動詞」は、Git に何をさせたいのかを表し、「オプション」はその動詞にとって必要とされる追加の情報です。 ドラキュラが新しいユーザーの場合、以下のようにコンピュータを設定します：

```bash
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
```

ここでは、ドラキュラの代わりに自分の名前とメールアドレスを使いましょう。 ここで入力した名前とメールアドレスは、これから行う Git での作業に関わってきます。というのも、これからのレッスンで[GitHub](https://github.com/)、[BitBucket](https://bitbucket.org/)、[GitLab](https://gitlab.com/)、またはその他のGit をホストするサーバーに変更箇所を「プシュ」した（送った）際に、これらの情報が使われるからです。

これらのレッスンでは、[GitHub](https://github.com/) に接続するので、GitHub アカウントと同じメールアドレスに設定してください。 プライバシーについて気になる方は、[GitHub のメールアドレスをプライベートにするための説明][git-privacy] を参照してください。

:::::::::::::::::::::::::::::::::::::::::  callout

## 電子メールの非公開

If you elect to use a private email address with GitHub, then use GitHub's no-reply email address for the `user.email` value. It looks like `ID+username@users.noreply.github.com`. You can look up your own address in your GitHub [email settings](https://github.com/settings/emails).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## 改行コード

他のキーと同様に、キーボードで <kbd>Enter</kbd> または <kbd>↵</kbd> （またはMacでは <kbd>Return</kbd>）を押すと、コンピュータはそれを文字として入力します。
話が長くなるので詳しい説明は省きますが、行末に使われる文字はオペレーティングシステム（OS）よって違います。
（行末に使われる文字を「改行コード」と呼びます。）
Git は、改行コードを使ってファイルの違いを確かめるため、違うパソコンでファイルを編集した時に思わぬ問題が起こるかもしれません。
このレッスンの範囲外ですが、この問題については
[Pro Git book](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_autocrlf) を読んでください。

Git がどのように改行コードを理解・変換するかは、 `git config` の `core.autocrlf` コマンドを使って変更できます。
以下の設定をおすすめします：

macOS および Linux:

```bash
$ git config --global core.autocrlf input
```

またはWindowsの場合:

```bash
$ git config --global core.autocrlf true
```

::::::::::::::::::::::::::::::::::::::::::::::::::

以下の表を参考に、ドラキュラはテキストエディタも設定しました：

| エディタ                                              | 設定コマンド                                                                                                                           |
| :------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------- |
| Atom                                              | `$ git config --global core.editor "atom --wait"`                                                                                |
| nano                                              | `$ git config --global core.editor "nano -w"`                                                                                    |
| BBEdit（Mac、コマンドラインツール付き）                          | `$ git config --global core.editor "bbedit -w"`                                                                                  |
| Sublime Text (Mac)             | `$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"`                     |
| Sublime Text (Win、32ビットインストール) | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"`                                |
| Sublime Text (Win、64ビットインストール) | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"`                                      |
| メモ帳（Win）                                          | `$ git config --global core.editor "c:/Windows/System32/notepad.exe"`                                                            |
| Notepad++ (Win、 32 ビットインストール)  | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"` |
| Notepad++ (Win、 64 ビットインストール)  | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`       |
| Kate (Linux)                   | `$ git config --global core.editor "kate"`                                                                                       |
| Gedit (Linux)                  | `$ git config --global core.editor "gedit --wait --new-window"`                                                                  |
| Scratch (Linux)                | `$ git config --global core.editor "scratch-text-editor"`                                                                        |
| Emacs                                             | `$ git config --global core.editor "emacs"`                                                                                      |
| Vim                                               | `$ git config --global core.editor "vim"`                                                                                        |
| VS Code                                           | `$ git config --global core.editor "code --wait"`                                                                                |

設定したテキストエディタもいつでも変更することができます。

:::::::::::::::::::::::::::::::::::::::::  callout

## Vim の終了の仕方

多くのソフトの初期設定では、Vim がデフォルトのテキストエディタに設定されています。 保存せずに Vim を終了するには、<kbd>Esc</kbd>を押した後に `:q!` と入力してから<kbd>Enter</kbd>または<kbd>↵</kbd>（Macの場合は<kbd>Return</kbd>）を押してください。
保存してから終了するには、<kbd>Esc</kbd>を押してから `:wq` と入力して<kbd>Enter</kbd>または<kbd>↵</kbd>（Mac の場合は <kbd>Return</kbd>）を押してください。

::::::::::::::::::::::::::::::::::::::::::::::::::

Git (2.28以上) では、新しいリポジトリを初期化したときに作成されるブランチの名前を設定できます。  ドラキュラはその機能を使って、最終的に使うクラウドサービスと一致するように`main`に設定することにします。

```bash
$ git config --global init.defaultBranch main
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Gitブランチ名の設定

ソースファイルのどんな変更でも、ある「ブランチ」に属しています。
このレッスンの新しい学習者にとっては、ブランチが存在し、このレッスンでは1つのブランチを使うことを知っていれば十分です。\
デフォルトでは、`git init` で新しいリポジトリを作成すると、Git が`master`
というブランチを作成します (次のエピソードで説明します)。 この用語は、
人身売買という人種差別的慣習を想起させ、
[ソフトウェア開発コミュニティ](https://github.com/github/renaming) は、
より包括的な言葉を採用するように動いています。

2020年には、ほとんどのGit ホスティングサービスは、`main`をデフォルトの
ブランチとして使うように移行しました。 例として、GitHub やGitLab で新規に開いたリポジトリのデフォルトは
`main`です。  しかし、Gitはまだ同じ変更を行っていません。  その結果、ローカル・リポジトリは、ほとんどのクラウド・サービスと同じブランチ名を手動で設定する必要があります。

2.28より前のバージョンのGitでは、個々のリポジトリで変更が可能です。  そのためのコマンドは次回のエピソードで紹介します。  ローカルのGit の設定でこの値が設定されていない場合、`init.defaultBranch` のデフォルト値は `master` になることに注意しましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

上記の5つのコマンドは、一度実行するだけで十分です。`--global` フラグは Git に、
今使っているパソコン内にある自分のアカウントに関連する全てのプロジェクトに同じ設定をするように指示しています。

さっそくこれらの設定を確認し、`core.editor`をテストしてみましょう：

```bash
$ git config --global --edit
```

追加の変更を加えずにファイルを閉じましょう。  設定ファイルのタイプミスは問題を引き起こすため、以下のように設定を表示する方が安全です。

```bash
$ git config --list
```

また、必要であれば、同じコマンドを使えば、違うエディタやメールアドレスに変えることができます。
これは何度でもできます。

:::::::::::::::::::::::::::::::::::::::::  callout

## プロキシ

ネットワーク環境によっては[プロキシ](https://ja.wikipedia.org/wiki/%E3%83%97%E3%83%AD%E3%82%AD%E3%82%B7) を使わなければならないかもしれません。 この場合、プロキシの設定が必要です：

```bash
$ git config --global http.proxy proxy-url
$ git config --global https.proxy proxy-url
```

プロキシを無効にするには：

```bash
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Git のヘルプとマニュアル

`git` のコマンドを忘れた時は、`-h` を使えばコマンドの一覧を、`--help` を使えばマニュアルを見ることができます：ある `git` コマンドのサブコマンドやオプションを忘れてしまった場合は、 `git <command> -h` とタイプすることによって関連するオプションのリストを見るか、`git <command> --help` とタイプすることによって対応する Git マニュアルを見ることができます：

```bash
$ git config -h
$ git config --help
```

マニュアルを見ている間、`:`はコマンドを待っているプロンプトであり、 <kbd>Q</kbd> を押してマニュアルを終了できることを覚えておいてください。

より一般的には、利用可能な `git` コマンドのリストや、Git マニュアルを入手することができます：

```bash
$ git help
```

::::::::::::::::::::::::::::::::::::::::::::::::::

[git-privacy]: https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git config` と `--global` オプションを使い、ユーザー名、メールアドレス、エディタ、その他の設定を行う。

::::::::::::::::::::::::::::::::::::::::::::::::::
