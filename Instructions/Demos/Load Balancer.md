---
demo:
  module: Module 04 - Load balancing non-HTTPS traffic
  title: Load Balancer (モジュール 04)
---
## Azure Load Balancer を構成する

このデモでは、パブリック ロード バランサーを作成する方法について学習します。 

**シミュレーション:** [Azure Load Balancer を作成して構成する](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer)

**リファレンス**: [クイック スタート: Azure portal を使用して、VM の負荷分散を行うパブリック ロード バランサーを作成する](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**注:**  このデモでは、少なくとも 1 つのサブネットを持つ仮想ネットワークが必要です。 

**ポータルで選択ヘルプ機能を表示する**

1. Azure ポータルにアクセスします。

1. **[負荷分散 - 選択に関するヘルプ]** を検索して選択します。

1. ウィザードを使用して、さまざまなシナリオについて確認します。
   
**ロード バランサーを作成する**

1. Azure portal で続行します。

1. **[ロード バランサー]** を検索して選択します。 ロード バランサーを**作成**します。 

1. **[基本]** タブで、 **[SKU]** 、 **[種類]** 、 **[レベル]** について検討します。

1. **[フロントエンド IP 構成]** タブで、パブリック IP アドレスの使い方について検討します。

1. **[バックエンド プール]** タブで、IP アドレス範囲を持つ仮想ネットワークを選択します。

1. **[受信規則]** タブで、負荷分散規則を作成します。 **[プロトコル]** 、 **[ポート]** 、 **[正常性プローブ]** 、 **[セッション永続化]** などのパラメーターについて検討します。 

