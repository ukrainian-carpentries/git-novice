---
title: Gitのインストール
---

## Gitのインストール

いくつかのCarpentriesのレッスンはGitに依存しているので、
[ワークショップテンプレートのこのセクション][workshop-setup] を参照してください。
様々なオペレーティングシステムにGitをインストールする手順があります。

- [Windows でのGit のインストール][workshop-setup]
- [MacOS でのGit のインストール][workshop-setup]
- [Linux でのGit のインストール][workshop-setup]

## GitHub アカウントの作成

このレッスンのエピソード7と8には、 [GitHub](https://github.com)のアカウントが必要です。

1. <https://github.com> にアクセスし、ウィンドウの右上にある「Sign Up」のリンクに従ってください。
2. 指示に従ってアカウントを作成してください。
3. GitHub でメールアドレスを確認する。
4. 多要素認証を設定する（下記参照）。

### 多要素認証

2023年にGitHubは 、セキュリティを強化するためすべてのアカウントに [多要素認証(2FA)](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication)の設定を行うという要件を導入した。
2FAを設定するためにいくつかのオプションがあります。

1. [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en\&co=GENIE.Platform%3DiOS\&oco=0)や [Duo Mobile](https://duo.com/ja-jp) のような認証アプリを既に使っている場合は、 [そのアプリにGitHub を追加](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication) してください。
2. スマートフォンにアクセスできるが、まだ認証アプリを使っていない場合は、認証アプリをインストールし、 [アプリにGitHubを追加](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)してください。
3. スマートフォンにアクセスできない場合、または認証アプリをインストールしたくない場合は、2つの選択肢があります：
   1. [テキストメッセージによる2FAの設定](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)
      ([SMSによる認証がサポートされている国のリスト](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported))、または
   2. [ハードウェアセキュリティキーを使用する](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)
      [YubiKey](https://www.yubico.com/?lang=ja)
      のように、または[Google Titanキー](https://store.google.com/jp/product/titan_security_key?hl=ja)を使用してください。

GitHubのドキュメンテーションに[2FAの設定に関する詳細](https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)があります。

----------------

## 作業ディレクトリの準備

`Desktop`フォルダーで作業を行うので、作業ディレクトリーを`Desktop`フォルダーに変更してください：

```bash
$ cd
$ cd Desktop
```

[workshop-setup]: https://carpentries.github.io/workshop-template/install_instructions/#git
