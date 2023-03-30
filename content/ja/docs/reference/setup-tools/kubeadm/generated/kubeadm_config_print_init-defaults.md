<!--
The file is auto-generated from the Go source code of the component using a generic
[generator](https://github.com/kubernetes-sigs/reference-docs/). To learn how
to generate the reference documentation, please read
[Contributing to the reference documentation](/docs/contribute/generate-ref-docs/).
To update the reference content, please follow the 
[Contributing upstream](/docs/contribute/generate-ref-docs/contribute-upstream/)
guide. You can file document formatting bugs against the
[reference-docs](https://github.com/kubernetes-sigs/reference-docs/) project.
-->


'kubeadm init'で使用できるデフォルトの初期設定を表示します。

### 概要

このコマンドは、'kubeadm init'で使用できるデフォルトの初期設定のようなオブジェクトを表示します。

ブートストラップトークンフィールドのような気密性の高い値は、トークン作成のための実際の計算を行いませんが、検証を通過するために"abcdef.0123456789abcdef"のような代用の値で置き換えられることに注意してください。

```
kubeadm config print init-defaults [flags]
```

### オプション

   <table style="width: 100%; table-layout: fixed;">
<colgroup>
<col span="1" style="width: 10px;" />
<col span="1" />
</colgroup>
<tbody>

<tr>
<td colspan="2">--component-configs strings</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>デフォルトの値を表示するためのコンポーネント設定APIオブジェクトのカンマ区切りのリストです。利用可能な値はこちらです：[KubeProxyConfiguration KubeletConfiguration]。 このフラグが設定されていない場合、コンポーネント設定は出力されません。</p></td>
</tr>

<tr>
<td colspan="2">-h, --help</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>このコマンドのヘルプです。</p></td>
</tr>

</tbody>
</table>



### 親コマンドから継承したオプション

   <table style="width: 100%; table-layout: fixed;">
<colgroup>
<col span="1" style="width: 10px;" />
<col span="1" />
</colgroup>
<tbody>

<tr>
<td colspan="2">--kubeconfig string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "/etc/kubernetes/admin.conf"</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>クラスターと通信する際に使用するkubeconfigファイルです。このフラグが設定されていない場合、標準的な場所の集合で既存のkubeconfigファイルが検索されます。</p></td>
</tr>

<tr>
<td colspan="2">--rootfs string</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>[試験的な実装] '実際の'ホストルートファイルシステムへのパスです。</p></td>
</tr>

</tbody>
</table>



