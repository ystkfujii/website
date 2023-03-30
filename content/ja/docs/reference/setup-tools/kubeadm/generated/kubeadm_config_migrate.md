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

ファイルからkubeadmの設定APIタイプの古いバージョンを読み、新しいバージョンに似た設定オブジェクトを出力します。

### 概要

このコマンドは、古いバージョンの設定オブジェクトを最新のサポートされたバージョンに変換できます。
これはクラスター内で何かしらの操作を一度もすることなく、CLIツールでローカルに行えます。
このバージョンのkubeadmでは、下記のAPIバージョンがサポートされています。
- kubeadm.k8s.io/v1beta3

さらに、kubeadmは"kubeadm.k8s.io/v1beta3"の設定を書き出すことのみできますが、両方のタイプを読めます。
したがって、stdoutに書き込まれるか--new-configが指定された場合は、--old-configパラメーターに渡すバージョンに関係なく、APIオブジェクトは読み取り、デシリアライズ、デフォルト化、コンバート、検証、再シリアライズされます。

言い換えるとこのコマンドの出力は、このファイルを"kubeadm init"に送付した場合にkubeadmが実際に内部で読み込むものです。

```
kubeadm config migrate [flags]
```

### オプション

   <table style="width: 100%; table-layout: fixed;">
<colgroup>
<col span="1" style="width: 10px;" />
<col span="1" />
</colgroup>
<tbody>

<tr>
<td colspan="2">-h, --help</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>このコマンドのヘルプです。</p></td>
</tr>

<tr>
<td colspan="2">--new-config string</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>新しいAPIバージョンを使用した結果の同等のkubeadmの設定ファイルへのパスです。オプションであり、指定しない場合はSTDOUTに出力が送付されます。</p></td>
</tr>

<tr>
<td colspan="2">--old-config string</td>
</tr>
<tr>
<td></td><td style="line-height: 130%; word-wrap: break-word;"><p>古いAPIバージョンを使用しており、変換する必要のあるkubeadm設定ファイルのパスです。このフラグは必須です。</p></td>
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



