---
layout: blog
title: "Windows Operational Readinesの仕様を紹介する"
date: 2024-04-03
author: >
  Jay Vyas (Tesla),
  Amim Knabben (Broadcom),
  Tatenda Zifudzi (AWS)
---

2019年にKubernetes 1.14でWindowsサポートが[安定版に移行](/blog/2019/03/25/kubernetes-1-14-release-announcement/)して以来 、Windowsワークロードを実行する機能がエンドユーザーコミュニティから高く評価されてきました。
Windowsワークロードサポートのレベルと可用性は、大企業が使用するKubernetesディストリビューションにとって常に大きな差別化要因となっています。
しかし、より多くのWindowsワークロードがKubernetesに移行され、新しいWindows機能が継続的にリリースされるにつれ、効果的かつ標準化された方法でWindowsワーカーノードをテストすることが困難になりました。

Kubernetesプロジェクトは、Windowsを提供する意図のない認定ディストリビューションやサービスのクローズドソースライセンスを必要とすることなく、適合性を認定できる機能を重視しています。

SIG Windowsの注目を集めたいくつかの注目すべき例は次のとおりです。

- GitHub Issueの[kubernetes/kubernetes#120033](https://github.com/kubernetes/kubernetes/issues/120033)で説明された、
  Windows Node上でロードバランサーのソースアドレス範囲機能が正しく動作しない問題。
- [microsoft/Windows-Containers#44](https://github.com/microsoft/Windows-Containers/issues/44)で議論された、
  [GMSA](https://learn.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)がcontainerdで動作しないといった、Windows機能に関する機能上のIssuの報告。
- [kubernetes/kubernetes#97751](https://github.com/kubernetes/kubernetes/issues/97751)で説明されているような、
  さまざまなオペレーティングシステム構成にわたるコンテナ・ネットワーク・インターフェイス(CNI)プラグインを客観的に評価できるネットワーク ポリシー テストを開発するという課題。

したがって、SIG Windowsは、Windows Nodeを本番環境へ導入する前に運用準備(Operational Readiness)を整えるためのカスタマイズされたソリューションの必要性を認識しました。
結果として、 [Windows Operational Readinessの仕様](https://kep.k8s.io/2578)を開発するというアイデアが生まれました。

## 公式の適合性テストを実行することはできないのでしょうか?

Kubernetesプロジェクトには、Kubernetesクラスターが必要なKubernetes仕様を満たしていることを確認するために設計された標準化されたテストである[適合テスト](https://www.cncf.io/training/certification/software-conformance/#how)が含まれています。

ただし、これらのテストはもともとLinuxがKubernetesと互換性のある*唯一*のオペレーティング システムだった時代に定義されたものであるため、Windowsで使用できるように簡単に拡張できませんでした。
その重要性にもかかわらず、WindowsワークロードがKubernetesコミュニティで占める割合が小さいことを考えると、Linuxの適合性を証明するために多くのKubernetesディストリビューションが依存している主要な適合性スイートが、
GMSAやマルチオペレーティング システムのkube-proxy動作などのWindows固有の機能や機能強化で障害にならないことが重要でした。

したがって、Windows適合性テストには特殊なニーズがあったため、SIG Windowsは、Windows Operational Readinessの仕様を通じてWindows固有の適合性テストを提供する道を歩みました。

## Kubernetes のエンドツーエンド テスト スイートを実行することはできないでしょうか?

Linuxの世界では、[Sonobuoy](https://sonobuoy.io/)のようなツールが適合性スイートの実行を簡素化し、Kubernetesのコンパイルパスや[Ginkgo](https://onsi.github.io/ginkgo)タグの意味論を意識する必要性からユーザーを解放します。

Kubernetesテストをコンパイルする必要性については、Windowsユーザーも同様に、Kubernetes e2eスイートをゼロからコンパイルして実行するプロセスを好ましくないと感じる可能性があることに気づきました。
そのため、使いやすくてすぐに利用できる"プッシュボタン"ソリューションを提供する必要が明らかにありました。
さらに、Ginkgoのタグに関しても、[Ginkgo](https://onsi.github.io/ginkgo/)タグの集合を通じてWindows Nodeに適合性テストを適用することは、Linux愛好家や経験豊富なWindowsシステム管理者を含め、どのユーザーにとっても負担が大きいでしょう。

ギャップを埋め、ユーザーがクラスターがさまざまな機能をサポートしていることを簡単な方法で確認できるようにするために、Windowsに対するKubernetes SIGは、Windows Operational Readinessのアプリケーションを作成する必要性を感じました。
このGoで書かれたアプリケーションは、必要なWindows固有のテストを実行するプロセスを簡略化し、結果を明確でアクセスしやすい形式で提供します。

この取り組みは、Amazon、Microsoft、SUSE、Broadcomが含まれており、異なるクラウドプロバイダーやプラットフォームからの貢献を含む、協力的な取り組みです。

## Windows Operational Readinessの仕様をより詳しく見る {#specification}

Windows Operational Readinessの仕様は、[Ginkgo](https://onsi.github.io/ginkgo/)タグを単に対象とするよりもユーザーフレンドリーな方法で、Kubernetesリポジトリ内で見つかるテストを対象して実行します。
これは、主要テストと拡張テストの集合に分割された構造化されたテストスイートを導入し、各テスト集合には、ネットワーキングなどの特定のテスト領域をテストするために向けられたカテゴリが含まれています。
主要テストは、Kubernetes仕様で定義されたWindowsノードがサポートすべき基本的で重要な機能を対象とします。
一方、拡張テストは、Active Directoryとの統合など、Windows固有の機能にさらに深く関連したより複雑な機能をカバーします。
これらのテストの目標は、さまざまなワークロードと構成との互換性を確保するためにWindows固有の機能の広範な範囲を網羅し、基本的な要件を超えて拡張されます。
以下は、現在のカテゴリのリストです。

| カテゴリ名               | カテゴリの説明                                                                                                        |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------|
| `Core.Network`           | 最小限のネットワーク機能(PodがPodのIPにアクセスする機能)をテストします。                                                  |
| `Core.Storage`           | 最小限のストレージ機能(hostPathのストレージボリュームをマウントする機能)をテストします。                                   |
| `Core.Scheduling`        | 最小限のスケジューリング機能(CPU制限のあるPodをスケジュールする機能)をテストします。                                       |
| `Core.Concurrent`        | 最小限の同時機能(同時に複数のPodへのトラフィックを処理するためのノードの機能)をテストします。                               |
| `Extend.HostProcess`     | Windowns HostProcess Pod機能に関連した機能をテストします。                                                             |
| `Extend.ActiveDirectory` | Active Directory機能に関連した機能をテストします。                                                                     |
| `Extend.NetworkPolicy`   | Network Policy機能に関連した機能をテストします。                                                                       |
| `Extend.Network`         | 高度なネットワーク機能(IPv6をサポートする機能)をテストします。                                                           |
| `Extend.Worker`          | Windowsワーカーノード機能に関連した機能(同じクラスター内でTCPやUDPサービスにアクセスするためのノードの機能)をテストします。   |

## WindowsノードのOperational Readinessテストを実施するための方法

Windows Operational Readinesテストスイートを実行するために、セットアップ方法と実行方法が記載されているテストスイートの[`README`](https://github.com/kubernetes-sigs/windows-operational-readiness/blob/main/README.md)を参照してください。
テストスイートは、コンパイルされたバイナリかSonobuoyプラグインのどちらかを使用して、テストの実行方法に柔軟性を提供します。
テストスイート全体に対してテストを実行するか、カテゴリのリストを指定してテストを実行するかを選択することもできます。
クラウドプロバイダーは適合性の結果をアップロードすることを選択でき、透明性と信頼性が向上します。

コードをチェックアウトしたら、テストを実行できます。
たとえば、このサンプルコマンドは`Core.Concurrent`カテゴリーのテストを実行します。

```shell
./op-readiness --kubeconfig $KUBE_CONFIG --category Core.Concurrent
```

Kubernetesのコントリビューターとして、Windows Operational Readiness仕様を使用して特定のプルリクエストに対する変更をテストしたい場合は、新しいプルリクエストで次のボットコマンドを使用してください。

```shell
/test operational-tests-capz-windows-2019
```

## 将来を見据えて

Kubernetesリポジトリに新しいテストを追加することや、対象となる既存のテストケースを特定することで、厳選されたWindows固有のテストリストを改善することを目指しています。
仕様の長期的な目標は、Windowsワーカーノードのテストカバレッジを継続的に向上させ、Windowsサポートの堅牢性を向上させ、多様なクラウド環境でシームレスな経験を促進することです。
また、Windows Operational Readinessテストを公式のKubernetes適合性スイートに統合する計画もあります。

私たちの支援に興味がございましたら、ぜひご連絡ください！
1回限りのフィードバックからコードへの貢献、変更の促進を支援してくれる長期的なオーナーの存在まで、あらゆる形式の支援を歓迎します。
Windows Operational Readiness仕様は、SIG Windowsチームが所有しています。
[Kubernetes Slack ワークスペース](https://slack.k8s.io/) **#sig-windows**チャネルでチームに連絡できます。
[Windows Operational Readinessテストスイート](https://github.com/kubernetes-sigs/windows-operational-readiness/#readme)を探索し 、GitHubリポジトリに直接貢献することもできます。

仕様へ多大な貢献をいただいた、Kulwant Singh (AWS)、Pramita Gautam Rana (VMWare)、Xinqi Li (Google)、および Marcio Morales (AWS) に深く感謝します。
さらに、SIG WindowsチームのJames Sturtevant (Microsoft)、Mark Rossetti (Microsoft)、Claudiu Belu (Cloudbase Solutions)、および Aravindh Puthiyaparambil (Softdrive Technologies Group Inc.) の指導とサポートに感謝します。
