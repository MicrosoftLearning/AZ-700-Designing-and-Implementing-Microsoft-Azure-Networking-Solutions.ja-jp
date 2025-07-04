---
Exercise:
  title: M01 - ユニット 8 グローバル仮想ネットワーク ピアリングを使用して 2 つの Azure 仮想ネットワークを接続する
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 - ユニット 8 グローバル仮想ネットワーク ピアリングを使用して 2 つの Azure 仮想ネットワークを接続する

## 演習のシナリオ

このユニットでは、トラフィック フローを許可するピアリングを追加することにより、CoreServicesVnet と ManufacturingVnet の間の接続を構成します。

![仮想ネットワーク ピアリングの図。](../media/8-exercise-connect-two-azure-virtual-networks-global.png)

### 職務スキル

この演習では、次のことを行います。

+ タスク 1:仮想マシンを作成して構成をテストする
+ タスク 2:RDP を使用してテスト VM に接続する
+ タスク 3:VM 間の接続をテストする
+ タスク 4:CoreServicesVnet と ManufacturingVnet の間に VNet ピアリングを作成する
+ タスク 5:VM 間の接続をテストする

### 対話型ラボ シミュレーション

>**注**: 以前提供されていたラボ シミュレーションは廃止されました。

### 推定時間:20 分

## タスク 1: 構成をテストするための仮想マシンを作成する

このセクションでは、VNet 上にテスト VM を作成し、VNet から別の Azure 仮想ネットワーク内のリソースにアクセスできるかどうかをテストします。

### ManufacturingVM を作成する

1. Azure portal で、右上の [Cloud Shell] アイコンを選択します。 必要に応じて、シェルを構成します。  
    + **[PowerShell]** を選択します。
    + **[ストレージ アカウントは必要ありません]** と **[サブスクリプション]** を選択してから、**[適用]** を選択します。
    + ターミナルが作成され、プロンプトが表示されるまで待ちます。 

1. Cloud Shell 画面のツール バーで、**[ファイルの管理]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**ManufacturingVMazuredeploy.json** と **ManufacturingVMazuredeploy.parameters.json** というファイルをアップロードします。

    >**注:** 自分のサブスクリプションで作業している場合、[テンプレート ファイル](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises)は GitHub ラボ リポジトリで入手できます。

1. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   >**注**: 管理者パスワードを入力するように求められます。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

1. 仮想マシンが作成されていることを確認してください。

## タスク 2: RDP を使用してテスト VM に接続する

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

1. **ManufacturingVM** を選択します。

1. ManufacturingVM で、 **[接続] &gt; [RDP]** を選択します。

1. [ManufacturingVM \| 接続] で、**[RDP ファイルのダウンロード]** を選択します。

1. RDP ファイルをデスクトップに保存します。

1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使用して、ManufacturingVM に接続します。

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

1. **TestVM1** を選択します。

1. TestVM1 で、 **[接続] &gt; [RDP]** を選択します。

1. [TestVM1 \| 接続] で、 **[RDP ファイルのダウンロード]** を選択します。

1. RDP ファイルをデスクトップに保存します。

1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使用して、TestVM1 に接続します。

1. 両方の VM で、**[デバイスのプライバシー設定の選択]** の **[同意する]** を選択します。

1. 両方の VM で、**[ネットワーク]** の **[はい]** を選択します。

1. TestVM1 で PowerShell プロンプトを開き、次のコマンドを実行します: ipconfig

1. IPv4 アドレスを記録しておきます。

## タスク 3: VM 間の接続をテストする

1. ManufacturingVM で、PowerShell プロンプトを開きます。

1. 次のコマンドを使用して、CoreServicesVnet 上の TestVM1 への接続がないことを確認します。 TestVM1 には必ず IPv4 アドレスを使用してください。

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. テスト接続が失敗すると、次のような結果が表示されます。![Test-NetConnection 10.20.20.4 -port 3389 が失敗したことを示す PowerShell ウィンドウ](../media/test-netconnection-fail.png)

## タスク 4: CoreServicesVnet と ManufacturingVnet の間に VNet ピアリングを作成する

1. Azure のホーム ページで **[仮想ネットワーク]** を選択し、**CoreServicesVnet** を選択します。

1. CoreServicesVnet の **[設定]** で、**[ピアリング]** を選択します。
   ![CoreServicesVnet のピアリング設定のスクリーンショット](../media/create-peering-on-coreservicesvnet.png)

1. [CoreServicesVnet \| ピアリング] で、**[+ 追加]** を選択します。

1. この情報を使用して、ピアリングを作成します。 完了したら、**[追加]** を選択します。 

   **リモート仮想ネットワークの概要**

   | **オプション**                                    | **Value**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | [Peering link name](ピアリング リンク名)    | `ManufacturingVnet-to-CoreServicesVnet` |
   | 仮想ネットワーク | ManufacturingVnet |

    **リモート仮想ネットワーク ピアリングの設定**
   
   | **オプション**                                    | **Value**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | 'CoreServicesVnet' に対するアクセスを 'ManufacturingVnet' に許可する | Enabled |
   |'CoreServicesVnet' からのトラフィック転送を 'ManufacturingVnet' が受信する | Enabled |
 
    **ローカル仮想ネットワークの概要**

    | **オプション**                                    | **Value**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | [Peering link name](ピアリング リンク名) | `CoreServicesVnet-to-ManufacturingVnet` |
 
    **ローカル仮想ネットワーク ピアリングの設定**
   
    | **オプション**                                    | **Value**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | 'ManufacturingVnet' へのアクセスを 'CoreServicesVnet' に許可する | Enabled
    | 'CoreServicesVnet' が 'ManufacturingVnet' からのトラフィック転送を受信することを許可する | Enabled |
 
1. [CoreServicesVnet \| ピアリング] で、**CoreServicesVnet-to-ManufacturingVnet** ピアリングが**接続済み** であることを確認します。

1. [仮想ネットワーク] で **ManufacturingVnet** を選択し、**ManufacturingVnet-to-CoreServicesVnet** ピアリングが **接続済み** であることを確認します。

## タスク 5: VM 間の接続をテストする

1. ManufacturingVM で、PowerShell プロンプトを開きます。

1. 次のコマンドを使用して、今度は CoreServicesVnet 上の TestVM1 への接続があることを確認します。

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. テスト接続が成功すると、次のような結果が表示されます。![TCP テストが成功したことを示す Test-NetConnection 10.20.20.4 -port 3389 の PowerShell ウィンドウ: true](../media/test-connection-succeeded.png)


## リソースをクリーンアップする

   >**注**:新規に作成し、使用しなくなったすべての Azure リソースを削除することを忘れないでください。 使用していないリソースを削除することで、予期しない料金が発生しなくなります。

1. Azure portal の **[Cloud Shell]** ペイン内で **PowerShell** セッションを開きます。 (必要に応じて、既定の設定を使って Cloud Shell ストレージを作成します)。

1. 次のコマンドを実行して、このモジュールのラボ全体を通して作成したすべてのリソース グループを削除します。

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```
   >**注**:このコマンドは非同期で実行されるため (-AsJob パラメーターによって決定されます)、同じ PowerShell セッション内で直後に別の PowerShell コマンドを実行できますが、リソース グループが実際に削除されるまでに数分かかります。
   
## Copilot を使用して学習を拡張する

Copilot は、Azure スクリプト ツールの使用方法を学習するのに役立ちます。 Copilot は、ラボでは対象外の、またはさらに詳しい情報が必要な領域でも役立ちます。 Edge ブラウザーを開き、Copilot (右上) を選択するか、*copilot.microsoft.com* に移動します。 次のプロンプトを試すには数分かかります。
+ Azure 仮想ネットワーク ピアリングを構成するときの最も一般的なエラーは何ですか?
+ Azure で、Vnet1 と Vnet2 をピアリングして、Vnet2 と Vnet3 をピアリングした場合、Vnet1 は Vnet3 とピアリングされますか?
+ ファイアウォールとゲートウェイは Azure 仮想ネットワーク ピアリングに影響しますか?


## 自習トレーニングでさらに学習する

+ [Azure 仮想ネットワークの概要](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/)。 このモジュールでは、Azure ネットワーク サービスの設計と実装の方法について説明します。 仮想ネットワーク、パブリック IP とプライベート IP、DNS、仮想ネットワーク ピアリング、ルーティング、Azure Virtual Network NAT などを取り上げます。
+ [Azure 仮想ネットワーク全体にサービスを分散させ、仮想ネットワーク ピアリングを使用して統合する](https://learn.microsoft.com/training/modules/integrate-vnets-with-vnet-peering/)。 このモジュールでは、仮想ネットワーク ピアリングを構成する方法について説明します。

## 要点

以上でラボは完了です。 このラボの要点は次のとおりです。 

+ 仮想ネットワーク ピアリングを使用すると、2 つの Azure 仮想ネットワーク間をシームレスに接続できます。 仮想ネットワークは、接続において、見かけ上 1 つのネットワークとして機能します。
+ Azure では、同じ Azure リージョン内および Azure リージョン (グローバル) 間での仮想ネットワークの接続がサポートされています。
+ ピアリングされた仮想ネットワーク内の仮想マシン間のトラフィックは、ゲートウェイやパブリック インターネット経由ではなく、Microsoft のバックボーン インフラストラクチャを通じて直接ルーティングされます。
+ 現在ピアリングされているアドレス空間で、ダウンタイムを発生させずに、ピアリングされている Azure 仮想ネットワークのアドレス空間のサイズを変更することが可能です。
