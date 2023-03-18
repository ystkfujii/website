---
title: PodからKubernetes APIにアクセスする
content_type: task
weight: 120
---

<!-- overview -->

このガイドはPod内からKubernetes APIにアクセスする方法を説明します。

## {{% heading "prerequisites" %}}

{{< include "task-tutorial-prereqs.md" >}}

<!-- steps -->

## Pod内からAPIにアクセスする

Pod内からAPIにアクセスする場合、APIサーバーの位置確認と認証は、外部クライアントの場合と若干異なります。

PodからKubernetes APIを利用する最も簡単な方法は、公式[クライアントライブラリ](/docs/reference/using-api/client-libraries/)のいずれかを使用することです。
これらのライブラリは、APIサーバーを自動的に検出し、認証することができます。

### 公式クライアントライブラリの使用

Pod内から、Kubernetes APIに接続する方法としては、以下の方法が推奨されています。

- Goクライアントの場合、公式の[Goクライアントライブラリ](https://github.com/kubernetes/client-go/)を使用します。
  `rest.InClusterConfig()`関数がAPIホストのディスカバリーと認証を自動的に処理します。
  [例](https://git.k8s.io/client-go/examples/in-cluster-client-configuration/main.go)を参照してください。

- Pythonクライアントの場合、公式の[Pythonクライアントライブラリ](https://github.com/kubernetes-client/python/)を使用します。
  `config.load_incluster_config()`関数がAPIホストのディスカバリーと認証を自動的に処理します。
  [例](https://github.com/kubernetes-client/python/blob/master/examples/in_cluster_config.py)を参照してください。

- 他にもいくつかの有効なライブラリがあるので、[Client Libraries](/docs/reference/using-api/client-libraries/)ページを参照してください。

いずれの場合も、APIサーバーと安全に通信するために、Podのサービスアカウント認証情報が使用されます。

### REST APIに直接アクセスする

Podで実行中、コンテナは`KUBERNETES_SERVICE_HOST`と`KUBERNETES_SERVICE_PORT_HTTPS`環境変数を取得することで、Kubernetes APIサーバーのHTTPS URLを作成できます。
APIサーバーのクラスタ内アドレスは、`default`名前空間の`kubernetes`という名前のサービスにも公開されるので、PodはローカルAPIサーバーのDNS名として`kubernetes.default.svc`を参照できるようになります。

{{< note >}}
Kubernetesは、APIサーバーがホスト名`kubernetes.default.svc`に対して有効な証明書を持つことを保証しません。
しかし、コントロールプレーン**は**`$KUBERNETES_SERVICE_HOST`が示すホスト名またはIPアドレスに対して有効な証明書を提示することが期待されます。
{{< /note >}}

APIサーバーを認証する推奨方法は、[サービスアカウント](/docs/tasks/configure-pod-container/configure-service-account/)のクレデンシャルを使用することです。
デフォルトでは、Podはサービスアカウントに関連付けられ、そのサービスアカウントのクレデンシャル（トークン）は、そのPod内の各コンテナのファイルシステムツリー、`/var/run/secrets/kubernetes.io/serviceaccount/token`に置かれます。

可能な場合、証明書バンドルが各コンテナのファイルシステムツリー`/var/run/secrets/kubernetes.io/serviceaccount/ca.crt`に配置され、APIサーバーのサービング証明書を検証するために使用される必要があります。

最後に、名前空間スコープのAPI操作に使用するdefault名前空間は、各コンテナの`/var/run/secrets/kubernetes.io/serviceaccount/namespace`のファイルに配置されます。

### kubectlプロキシーを使用する

公式のクライアントライブラリなしでAPIをクエリしたい場合は、Pod内の新しいサイドカーコンテナの[コマンド](/docs/tasks/inject-data-application/define-command-argument-container/)として`kubectl proxy`を実行することができます。
この方法で、`kubectl proxy`がAPIを認証してPodの`localhost`インターフェイスに公開するので、Pod内の他のコンテナはAPIを直接使用できるようになります。

### プロキシーを使用しない

APIサーバーに直接認証トークンを渡すことで、kubectlプロキシーの使用を避けることも可能です。
内部証明書により接続を保護します。

```shell
# 内部APIサーバーのホスト名を指定する
APISERVER=https://kubernetes.default.svc

# サービスアカウントトークンへのパス
SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount

# このPodの名前空間を読む
NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace)

# サービスアカウントのベアラートークンを読む
TOKEN=$(cat ${SERVICEACCOUNT}/token)

# 内部認証局(CA)を参照する
CACERT=${SERVICEACCOUNT}/ca.crt

# TOKENでAPIを探る
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api
```

このような出力になります。

```json
{
  "kind": "APIVersions",
  "versions": ["v1"],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "10.0.1.149:443"
    }
  ]
}
```
