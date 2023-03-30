---
reviewers:
- luxas
- jbeda
title: kubeadm config
content_type: concept
weight: 50
---

<!-- overview -->
`kubeadm init`中に, kubeadmは`kube-system`名前空間内の`kubeadm-config`と呼ばれるConfigMap内のクラスターに`ClusterConfiguration`オブジェクトをアップロードします。
この設定は`kubeadm join`と`kubeadm reset`と`kubeadm upgrade`の際に読まれます。

`kubeadm config print`を使用すると、`kubeadm init`や`kubeadm join`でkubeadmが使用するデフォルトの静的設定を表示することができます。

{{< note >}}
コマンドの出力は例として提供されることを意図しています。
実際にセットアップを行う際には、コマンドの出力を手動で編集する必要があります。
よく分からないフィールドを削除すると、kubeadmはホストを調査して実行時にデフォルトの値にすることを試みます。
{{< /note >}}

`init`や`join`に関する詳細は、[設定ファイルを使ってkubeadm initを使用する](/docs/reference/setup-tools/kubeadm/kubeadm-init/#config-file)または[設定ファイルを使ってkubeadm joinを使用する](/docs/reference/setup-tools/kubeadm/kubeadm-join/#config-file)を参照してください。

kubeadm設定APIの使用に関する詳細は、[kubeadm APIでコンポーネントをカスタマイズする](/docs/setup/production-environment/tools/kubeadm/control-plane-flags)を参照してください。

`kubeadm config migrate`を使用することで、非推奨のAPIバージョンを含んでいる古い設定ファイルを新しいサポートされたAPIバージョンに変換することができます。

`kubeadm config images list`や`kubeadm config images pull`は、kubeadmが必要とするイメージを取得したり、リスト表示するために使用することができます。

<!-- body -->
## kubeadm config print {#cmd-config-print}

{{< include "generated/kubeadm_config_print.md" >}}

## kubeadm config print init-defaults {#cmd-config-print-init-defaults}

{{< include "generated/kubeadm_config_print_init-defaults.md" >}}

## kubeadm config print join-defaults {#cmd-config-print-join-defaults}

{{< include "generated/kubeadm_config_print_join-defaults.md" >}}

## kubeadm config migrate {#cmd-config-migrate}

{{< include "generated/kubeadm_config_migrate.md" >}}

## kubeadm config images list {#cmd-config-images-list}

{{< include "generated/kubeadm_config_images_list.md" >}}

## kubeadm config images pull {#cmd-config-images-pull}

{{< include "generated/kubeadm_config_images_pull.md" >}}

## {{% heading "whatsnext" %}}

* Kubernetesクラスターを新しいバージョンにアップグレードするためには、[kubeadm upgrade](/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/)を参照してください。