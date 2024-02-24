---
title: Hosting
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- 科学研究のホスティングのためのさまざまなオプションについて説明します。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- バージョン管理リポジトリはどこでホストすればよいでしょうか?

::::::::::::::::::::::::::::::::::::::::::::::::::

自分たちの仕事をオープンにしたいグループにとって次に大きな問題は、コードとデータをどこにホストするかです。  1 つのオプションは、研究室、学部、または大学がサーバーを提供し、アカウントやバックアップなどを管理することです。  これの主な利点は、誰が何を所有しているかを明確にすることです。
それは、題材が機密性の高いもの (すなわち、ヒトを対象とする実験に関するもの、または特許出願に使用される可能性のあるもの) である場合に特に重要です。  主な欠点は、サービスの提供コストとその存続期間です。データ収集に 10 年を費やした科学者は、10 年後もデータが利用できることを確認したいと考えますが、それは学術インフラに資金提供されるほとんど助成金の存続期間をはるかに超えています。

もう 1 つのオプションは、ドメインを購入し、それをホストするためにインターネット サービス プロバイダー (ISP) に料金を支払うことです。  これにより、個人またはグループによる制御が強化され、ある機関から別の機関に移動するときに発生する可能性のある問題を回避できます。
ただし、上記のオプションまたは下記のオプションよりもセットアップに多くの時間と労力が必要です。

3 番目のオプションは、
[GitHub](https://github.com)、 [GitLab](https://gitlab.com)、または
[BitBucket](https://bitbucket.org) のようなパブリック ホスティング サービスを使用することです。
これらの各サービスは、コードリポジトリを作成、閲覧、編集できる Web インターフェイスを提供します。  これらのサービスは、イシュートラッキング、Wiki ページ、メール通知、コードレビューなどのコミュニケーションやプロジェクト管理ツールも提供します。  These services benefit from economies of
scale and network effects: it's easier to run one large service well than to run
many smaller services to the same standard.  It's also easier for people to
collaborate.  Using a popular service can help connect your project with
communities already using the same service.

As an example, Software Carpentry [is on GitHub](https://github.com/swcarpentry/) where you can find the source for this
page. Anyone with a GitHub account can suggest changes to this text.

GitHub repositories can also be assigned DOIs, by connecting its releases to
Zenodo. For example,
[`10.5281/zenodo.7908089`](https://zenodo.org/record/7908089) is the DOI that has
been "minted" for this introduction to Git.

Using large, well-established services can also help you quickly take advantage
of powerful tools.  One such tool, continuous integration (CI), can
automatically run software builds and tests whenever code is committed or pull
requests are submitted.  Direct integration of CI with an online hosting service
means this information is present in any pull request, and helps maintain code
integrity and quality standards.  While CI is still available in self-hosted
situations, there is much less setup and maintenance involved with using an
online service.  Furthermore, such tools are often provided free of charge to
open source projects, and are also available for private repositories for a fee.

:::::::::::::::::::::::::::::::::::::::::  callout

## Institutional Barriers

Sharing is the ideal for science,
but many institutions place restrictions on sharing,
for example to protect potentially patentable intellectual property.
If you encounter such restrictions,
it can be productive to inquire about the underlying motivations and
either to request an exception for a specific project or domain,
or to push more broadly for institutional reform to support more open science.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Can My Work Be Public?

Find out whether you are allowed to host your work openly in a public repository.
Can you do this unilaterally,
or do you need permission from someone in your institution?
If so, who?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Where Can I Share My Work?

Does your institution have a repository or repositories that you can
use to share your papers, data and software? How do institutional repositories
differ from services like [arXiV](https://arxiv.org/), [figshare](https://figshare.com/), [GitHub](https://github.com/) or [GitLab](https://about.gitlab.com/)?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Projects can be hosted on university servers, on personal domains, or on a public hosting service.
- Rules regarding intellectual property and storage of sensitive information apply no matter where code and data are hosted.

::::::::::::::::::::::::::::::::::::::::::::::::::
