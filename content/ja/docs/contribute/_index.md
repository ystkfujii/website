---
content_type: concept
title: K8sのドキュメントに貢献する
linktitle: 貢献
main_menu: true
no_list: true
weight: 80
card:
  name: 貢献
  weight: 10
  title: K8sへの貢献を始める
---

<!-- overview -->

*Kubernetesは初心者でも経験者でも、全てのコントリビューターからの改善を歓迎しています！*

{{< note >}}
Kubernetesへの貢献について総合的に知りたい場合は、[contributor documentation](https://www.kubernetes.dev/docs/)を参照してください。

Kubernetesへの貢献については
{{< glossary_tooltip text="CNCF" term_id="cncf" >}}
[ページ](https://contribute.cncf.io/contributors/projects/#kubernetes)
にも記載があります。
{{< /note >}}

---

このウェブサイトは[Kubernetes SIG Docs](/docs/contribute/#get-involved-with-sig-docs)が管理しています。

Kubernetesドキュメントコントリビューターは

- 既存のコンテンツを改善します
- 新しいコンテンツを作成します
- ドキュメントを翻訳します
- Kubernetesリリースサイクルの一部としてドキュメントを管理・公開します

<!-- body -->

## はじめに

どなたでも、問題を説明するissueや、ドキュメントの改善を求めるissueを作成し、[`kubernetes/website` GitHub リポジトリ](https://github.com/kubernetes/website)に対するプルリクエスト(PR)を用いて変更に貢献することができます。
Kubernetesコミュニティで効果的に働くためには、[git](https://git-scm.com/)と[GitHub](https://lab.github.com/)を基本的に使いこなせる必要があります。

ドキュメンテーションに関わるには:

1. CNCFの[Contributor License Agreement](https://github.com/kubernetes/community/blob/master/CLA.md)にサインしてください。
2. [ドキュメンテーションのリポジトリー](https://github.com/kubernetes/website)と、ウェブサイトの[静的サイトジェネレーター](https://gohugo.io)に慣れ親しんでください。
3. [プルリクエストのオープン](/docs/contribute/new-content/open-a-pr/)と[変更レビュー](/ja/docs/contribute/review/reviewing-prs/)の基本的なプロセスを理解していることを確認してください。

<!-- この図のライブエディターURLについては、https://github.com/kubernetes/website/issues/28808を見てください -->
<!-- https://mermaid-js.github.io/mermaid-live-editor でMermaid Codeをライブエディターにカット/ペーストして、遊ぶこともできます -->

{{< mermaid >}}
flowchart TB
subgraph third[Open PR]
direction TB
U[ ] -.-
Q[コンテンツを改善する] --- N[コンテンツを作成する]
N --- O[ドキュメントを翻訳する]
O --- P[K8sリリースサイクルの<br>一部のドキュメントを<br>マージ/公開する]

end

subgraph second[Review]
direction TB
   T[ ] -.-
   D[K8s/website<br>リポジトリを<br>見る] --- E[Hugo static site<br>generatorを<br>チェックする]
   E --- F[基本的な<br>GitHubコマンドを<br>理解する]
   F --- G[オープンなPRをレビューし<br>レビュープロセスを変更する]
end

subgraph first[Sign up]
    direction TB
    S[ ] -.-
    B[CNCF Contributor<br>License Agreement<br>にサインする] --- C[sig-docs<br>Slackチャンネル<br>に参加する] 
    C --- V[kubernetes-sig-docs<br>メーリングリスト<br>に参加する]
    V --- M[毎週のsig-docsコール<br>またはslackミーティング<br>に参加する]
end

A([fa:fa-user 新しい<br>コントリビューター]) --> first
A --> second
A --> third
A --> H[質問して!!!]


classDef grey fill:#dddddd,stroke:#ffffff,stroke-width:px,color:#000000, font-size:15px;
classDef white fill:#ffffff,stroke:#000,stroke-width:px,color:#000,font-weight:bold
classDef spacewhite fill:#ffffff,stroke:#fff,stroke-width:0px,color:#000
class A,B,C,D,E,F,G,H,M,Q,N,O,P,V grey
class S,T,U spacewhite
class first,second,third white
{{</ mermaid >}}
図1. 新しいコントリビューターへの入門

図1は新しいコントリビューターへのロードマップの概要を示しています。
`Sign up`と`Review`の一部または全てのステップを踏むことができます。
これで、`Open PR`配下にリストされているどれかで、あなたの貢献目的を達成するためにのPRを開く準備ができました。

一部のタスクでは、Kubernetes organizationで、より多くの信頼とアクセス権限が必要です。
役割と権限についての詳細は、[SIG Docsへの参加](/ja/docs/contribute/participate/)を参照してください。

## はじめての貢献

事前にいくつかのステップを確認することで、はじめての貢献に備えることができます。
図2はステップの概要を示しており、詳細は以下です。

<!-- この図のライブエディターURLについては、https://github.com/kubernetes/website/issues/28808を見てください -->
<!-- https://mermaid-js.github.io/mermaid-live-editor でMermaid Codeをライブエディターにカット/ペーストして、遊ぶこともできます -->

{{< mermaid >}}
flowchart LR
    subgraph second[はじめての貢献]
    direction TB
    S[ ] -.-
    G[他のK8sメンバーからの<br>PRをレビューする] -->
    A[はじめてに良いPRのために<br>K8s/websiteのissueリストを<br>調べる] --> B[PRを開く!!]
    end
    subgraph first[提案された準備]
    direction TB
       T[ ] -.-
       D[貢献の概要を読む] -->E[K8sコンテンツと<br>スタイルガイドを読む]
       E --> F[Hugoのコンテンツタイプと<br>ショートコードについて学ぶ]
    end
    

    first ----> second
     

classDef grey fill:#dddddd,stroke:#ffffff,stroke-width:px,color:#000000, font-size:15px;
classDef white fill:#ffffff,stroke:#000,stroke-width:px,color:#000,font-weight:bold
classDef spacewhite fill:#ffffff,stroke:#fff,stroke-width:0px,color:#000
class A,B,D,E,F,G grey
class S,T spacewhite
class first,second white
{{</ mermaid >}}
図2. はじめの貢献への備え

- 貢献のための複数の方法について学ぶために[貢献の概要](/ja/docs/contribute/new-content/)を読んでください。
- 良い開始地点を探すために[`kubernetes/website` issueリスト](https://github.com/kubernetes/website/issues/)を確認してください。
- 既存のドキュメントに対して[GitHubを使ってプルリクエストをオープン](/docs/contribute/new-content/open-a-pr/#changes-using-github)し、GitHubへのissueの登録について学んでください。
- 正確さと言語の校正のため、他のKubernetesコミュニティメンバーから[プルリクエストのレビュー](/docs/contribute/review/reviewing-prs/)を受けてください。
- 見識のあるコメントを残せるようにするため、Kubernetesの[コンテンツ](/ja/docs/contribute/style/content-guide/)と[スタイルガイド](/docs/contribute/style/style-guide/)を読んでください。
- [ページコンテンツの種類](/docs/contribute/style/page-content-types/)と[Hugoショートコード](/docs/contribute/style/hugo-shortcodes/)について勉強してください。

## 次のステップ

- リポジトリの[ローカルクローンでの作業](/docs/contribute/new-content/open-a-pr/#fork-the-repo)について学んでください。
- [リリース機能](/docs/contribute/new-content/new-features/)について記載してください。
- [SIG Docs](/ja/docs/contribute/participate/)に参加し、[memberやreviewer](/docs/contribute/participate/roles-and-responsibilities/)になってください。
- [国際化](/ja/docs/contribute/localization/)を始めたり、支援したりしてください。

## SIG Docsに参加する

[SIG Docs](/ja/docs/contribute/participate/)はKubernetesのドキュメントとウェブサイトを公開・管理するコントリビューターのグループです。SIG Docsに参加することはKubernetesコントリビューター（機能開発でもそれ以外でも）にとってKubernetesプロジェクトに大きな影響を与える素晴らしい方法の一つです。

SIG Docsは複数の方法でコミュニケーションをとっています。

- [Kubernetes Slackインスタンスの`#sig-docs`に参加してください](https://slack.k8s.io/)。自己紹介を忘れずに！
- [`kubernetes-sig-docs`メーリングリストに参加してください](https://groups.google.com/forum/#!forum/kubernetes-sig-docs)。ここでは幅広い議論が起こり、公式な決定が記録されます。
- 隔週で開催される[SIG Docsビデオミーティング](https://github.com/kubernetes/community/tree/master/sig-docs)に参加してください。ミーティングは `#sig-docs`でアナウンスされ、[Kubernetesコミュニティミーティングカレンダー](https://calendar.google.com/calendar/embed?src=cgnt364vd8s86hr2phapfjc6uk%40group.calendar.google.com&ctz=America/Los_Angeles)に追加されます。[Zoomクライアント](https://zoom.us/download)をダウンロードするか、電話を使って通話する必要があります。
- 対面でのSIG Docsビデオミーティングが行われない週に、SIG Docs async Slackスタンドアップミーティングに参加してください。ミーティングは`#sig-docs`でアナウンスされます。ミーティングのアナウンスから24時間以内はどのスレッドにも貢献できます。

## その他の貢献方法

- [Kubernetesコミュニティサイト](/community/)を訪問してください。TwitterやStack Overflowに参加したり、Kubernetesの集会やイベントについて学んだりしてください。
- 機能開発に貢献したい方は、まずはじめに[Kubernetesコントリビューターチートシート](https://www.kubernetes.dev/docs/contributor-cheatsheet/)を読んでください。
- [Kubernetesコントリビューター](https://www.kubernetes.dev/)や[追加のコントリビューターリソース](https://www.kubernetes.dev/resources/)についての詳細を知るためにはコントリビューターサイトを訪問してください。
- [ブログ記事やケーススタディ](/docs/contribute/new-content/blogs-case-studies/)を投稿してください。
