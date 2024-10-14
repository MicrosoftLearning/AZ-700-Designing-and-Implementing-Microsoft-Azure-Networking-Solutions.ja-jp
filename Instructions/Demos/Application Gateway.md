---
demo:
  title: モジュール 05 - Application Gateway
  module: Module 05 - Load balancing HTTPS traffic
---
## Azure Application Gateway を構成する

このデモでは、Azure Application Gateway を作成する方法について学習します。 

**リファレンス**: [クイック スタート - Azure Application Gateway による Web トラフィックのルーティング - Azure portal](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**参照**: [Azure Application Gateway を使用してネットワーク トラフィックをエンドツーエンドで暗号化する](https://github.com/MicrosoftDocs/mslearn-end-to-end-encryption-with-app-gateway)

**注**: 作業をシンプルにするために、構成を進める際に新しい仮想ネットワークとサブネットを作成します。 

**Azure Application Gateway を作成する**

1. Azure ポータルにアクセスします。

1. **Azure Application Gateway** を検索して選択します。

1. 新しいゲートウェイを**作成**します。

1. **[基本]** タブで、 **[階層]** 、 **[自動スケーリング]** 、 **[インスタンス数]** について検討します。

1. **[フロントエンド]** タブで、IP アドレスの種類について検討します。

1. **[バックエンド]** タブで、空のバックエンド プールを使用するタイミングについて検討します。

1. **[構成]** タブで、ルーティング規則について検討します。 ロード バランサーの規則と比較します。

1. ゲートウェイが作成された後で、バックエンド ターゲットを追加してテストする場合について説明します。 
