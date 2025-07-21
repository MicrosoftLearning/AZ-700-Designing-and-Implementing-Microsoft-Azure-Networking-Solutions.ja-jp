---
demo:
  module: Module 01 - Introduction to Azure Virtual Networks
  title: DNS 名前解決 (モジュール 01)
---
## Azure DNS を構成する

このデモでは、Azure DNS について説明します。

**参照**: [チュートリアル: ドメインとサブドメインをホストする - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)

**DNS ゾーンの作成**

1. Azure portal にアクセスします。

1.  **DNS ゾーン** サービスを検索します。

1. **DNS ゾーン**を作成し、そのゾーンの目的を説明します。 名前には、contoso.internal.com を使用できます。

1.  DNS ゾーンが作成されるのを待ちます。 ページを **更新**する必要がある場合があります。 

**DNS レコード セットを追加する**

**参照**: [チュートリアル: ゾーン内のリソース レコードを参照するエイリアス レコードを作成する](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. ゾーンが作成されたら、 **[+ レコード セット]** を選択します。

1.  **[種類]**   ドロップダウンを使用して、さまざまな種類のレコードを表示します。 さまざまなレコードの種類の使用方法を確認します。 選択するレコードの種類によって、レコードの情報がどのように変わるかに注目してください。

1. 例として **A** レコードを作成します。 

