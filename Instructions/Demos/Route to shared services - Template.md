---
demo:
  title: 'モジュール 02: 共有サービスへのルーティング'
  module: Module 02 - Design and implement hybrid networking
---
これを受講者に示すことを検討してください。 

[クイックスタート: ARM テンプレートを使用して共有サービスにルーティングする - Azure Virtual WAN |Microsoft Docs](https://learn.microsoft.com/azure/virtual-wan/quickstart-route-shared-services-vnet-template)

このクイックスタートでは、Azure Resource Manager テンプレート (ARM テンプレート) を使用して、すべての VNet およびブランチ (VPN、ER、P2S) がアクセスするワークロードがある共有サービス VNet にアクセスするためのルートを設定する方法について説明します。 このような共有ワークロードの例には、ドメイン コントローラーやファイル共有などのサービスが含まれる仮想マシンや、Azure プライベート エンドポイント経由で内部公開される Azure サービスなどがあります。
ARM テンプレートは JavaScript Object Notation (JSON) ファイルであり、プロジェクトのインフラストラクチャと構成が定義されています。 このテンプレートでは、宣言型の構文が使用されています。 宣言型の構文では、デプロイしようとしているものを、デプロイを作成する一連のプログラミング コマンドを記述しなくても記述できます。
環境が前提条件を満たしていて、ARM テンプレートの使用に慣れている場合は、 [Azure へのデプロイ] ボタンを選択します。 Azure portal でテンプレートが開きます。

