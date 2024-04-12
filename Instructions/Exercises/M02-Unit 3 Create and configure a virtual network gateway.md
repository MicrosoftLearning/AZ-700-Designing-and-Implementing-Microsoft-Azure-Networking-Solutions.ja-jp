---
Exercise:
  title: M02 - ユニット 3 仮想ネットワーク ゲートウェイを作成および構成する
  module: Module 02 - Design and implement hybrid networking
---


# M02-ユニット 3 仮想ネットワーク ゲートウェイを作成および構成する

## 演習のシナリオ

この演習では、Contoso Core Services VNet と Manufacturing VNet を接続する仮想ネットワーク ゲートウェイを構成します。

![仮想ネットワーク ゲートウェイの図。](../media/3-exercise-create-configure-local-network-gateway.png)

この演習では、次のことを行います。

+ タスク 1: CoreServicesVnet と ManufacturingVnet を作成する
+ タスク 2:CoreServicesVM を作成する
+ タスク 3:ManufacturingVM を作成する
+ タスク 4:RDP を使用して VM に接続する
+ タスク 5: VM 間の接続をテストする
+ タスク 6: CoreServicesVnet ゲートウェイを作成する
+ タスク 7: ManufacturingVnet ゲートウェイを作成する
+ タスク 8:CoreServicesVnet を ManufacturingVnet に接続する
+ タスク 9: ManufacturingVnet を CoreServicesVnet に接続する
+ タスク 10: 接続が接続されていることを確認する
+ タスク 11: VM 間の接続をテストする

**メモ:** このラボをご自分のペースでクリックして進めることができる、 **[ラボの対話型シミュレーション](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20a%20virtual%20network%20gateway)** が用意されています。 対話型シミュレーションとホストされたラボの間に若干の違いがある場合がありますが、示されている主要な概念とアイデアは同じです。

### 推定時間: 70 分 (最大 45 分のデプロイ待機時間を含む)

## タスク 1: CoreServicesVnet と ManufacturingVnet を作成する

1. Azure portal の **[Cloud Shell]** ペイン内で **PowerShell** セッションを開きます。

 > **注:**  Cloud Shell を開いたのが初めてである場合、ストレージ アカウントを作成するよう求められる場合があります。 **[Create storage](ストレージの作成)** を選択します。

1. [Cloud Shell] ペインのツール バーで、 **[ファイルのアップロード/ダウンロード]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**azuredeploy.json** ファイルと **azuredeploy.parameters.json** ファイルを、ソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします

1. 次の ARM テンプレートをデプロイして、この演習に必要な仮想ネットワークとサブネットを作成します。

   ```powershell
   $RGName = "ContosoResourceGroup"
   #create resource group if it doesnt exist
   New-AzResourceGroup -Name $RGName -Location "eastus"
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```

 > **注:**  現在、西ヨーロッパ リージョンで、ゲートウェイのデプロイに影響する問題が発生しています。 回避策として、このデプロイでは、ManufacturingVnet のリージョンが北ヨーロッパに変更されました。

## タスク 2:CoreServicesVM を作成する

1. Azure portal の **[Cloud Shell]** ペイン内で **PowerShell** セッションを開きます。

1. [Cloud Shell] ペインのツールバーで、 **[ファイルのアップロード/ダウンロード]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**CoreServicesVMazuredeploy.json** ファイルと **CoreServicesVMazuredeploy.parameters.json** ファイルを、ソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします。

1. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   >**注**: 管理者パスワードを入力するように求められます。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
   ```
  
1. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

1. 仮想マシンが作成されていることを確認してください。

## タスク 3:ManufacturingVM を作成する

1. Azure portal の **[Cloud Shell]** ペイン内で **PowerShell** セッションを開きます。

1. [Cloud Shell] ペインのツール バーで、 **[ファイルのアップロード/ダウンロード]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**ManufacturingVMazuredeploy.json** ファイルと **ManufacturingVMazuredeploy.parameters.json** ファイルを、ソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします。

1. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   >**注**: 管理者パスワードを入力するように求められます。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

1. 仮想マシンが作成されていることを確認してください。

## タスク 4: RDP を使って VM に接続する

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。
1. **ManufacturingVM** を選択します。
1. **ManufacturingVM** で、 **[接続] &gt; [RDP]** を選択します。
1. **[ManufacturingVM | 接続]** で、 **[RDP ファイルのダウンロード]** を選択します。
1. RDP ファイルをデスクトップに保存します。
1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使って、**ManufacturingVM** に接続します。 接続後、RDP セッションを最小化します。
1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。
1. **[CoreServicesVM]** を選択します。
1. **[CoreServicesVM]** で **[接続] &gt; [RDP]** を選択します。
1. **[CoreServicesVM | 接続]** で、 **[RDP ファイルのダウンロード]** を選択します。
1. RDP ファイルをデスクトップに保存します。
1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使って、**CoreServicesVM** に接続します。
1. 両方の VM で、**[デバイスのプライバシー設定の選択]** の **[同意する]** を選択します。
1. 両方の VM で、**[ネットワーク]** の **[はい]** を選択します。
1. **CoreServicesVM** で PowerShell を開き、次のコマンドを実行します: ipconfig
1. IPv4 アドレスをメモします。

## タスク 5: VM 間の接続をテストする

1. **ManufacturingVM** で、PowerShell を開きます。

1. 次のコマンドを使用して、CoreServicesVnet に CoreServicesVM への接続がないことを確認します。 CoreServicesVM には必ず IPv4 アドレスを使用してください。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. テスト接続が失敗すると、次のような結果が表示されます。

   ![Test-NetConnection が失敗しました。](../media/test-netconnection-fail.png)

## タスク 6: CoreServicesVnet ゲートウェイを作成する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。
   ![Azure Portal で仮想ネットワーク ゲートウェイを検索します。](../media/virtual-network-gateway-search.png)

1. [仮想ネットワーク ゲートウェイ] で、**[+ 作成]** を選択します。

1. 次の表の情報を使用して、仮想ネットワーク ゲートウェイを作成します。

   | **Tab**         | **セクション**       | **オプション**                                  | **Value**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本          | プロジェクトの詳細   | サブスクリプション                                | 変更は必要ありません          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | インスタンスの詳細  | 名前                                        | CoreServicesVnetGateway      |
   |                 |                   | リージョン                                      | 米国東部                      |
   |                 |                   | ゲートウェイの種類                                | VPN                          |
   |                 |                   | VPN の種類                                    | ルート ベース                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation1                  |
   |                 |                   | 仮想ネットワーク                             | CoreServicesVnet             |
   |                 |                   | Subnet                                      | GatewaySubnet (10.20.0.0/27) |
   |                 |                   | パブリック IP アドレスの種類                      | Standard                     |
   |                 | パブリック IP アドレス | パブリック IP アドレス                           | 新規作成                   |
   |                 |                   | パブリック IP アドレス名                      | CoreServicesVnetGateway-ip   |
   |                 |                   | アクティブ/アクティブ モードの有効化                   | 無効                     |
   |                 |                   | BGP の構成                               | 無効                     |
   | [確認および作成] |                   | 設定を確認し、 **[作成]** を選択します。 |                              |

   > [!NOTE]
   >
   > 仮想ネットワーク ゲートウェイの作成には、最大で 45 分かかる場合があります。

## タスク 7: ManufacturingVnet ゲートウェイを作成する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

1. [仮想ネットワーク ゲートウェイ] で、**[+ 作成]** を選択します。

1. 次の表の情報を使用して、仮想ネットワーク ゲートウェイを作成します。

   | **Tab**         | **セクション**       | **オプション**                                  | **Value**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本          | プロジェクトの詳細   | サブスクリプション                                | 変更は必要ありません          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | インスタンスの詳細  | Name                                        | ManufacturingVnetGateway     |
   |                 |                   | リージョン                                      | 北ヨーロッパ                  |
   |                 |                   | ゲートウェイの種類                                | VPN                          |
   |                 |                   | VPN の種類                                    | ルート ベース                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation1                  |
   |                 |                   | 仮想ネットワーク                             | ManufacturingVnet            |
   |                 |                   | Subnet                                      | GatewaySubnet (10.30.0.0/27) |
   |                 |                   | パブリック IP アドレスの種類                      | Standard                     |
   |                 | パブリック IP アドレス | パブリック IP アドレス                           | 新規作成                   |
   |                 |                   | パブリック IP アドレス名                      | ManufacturingVnetGateway-ip  |
   |                 |                   | アクティブ/アクティブ モードの有効化                   | 無効                     |
   |                 |                   | BGP の構成                               | 無効                     |
   | [確認および作成] |                   | 設定を確認し、 **[作成]** を選択します。 |                              |

   > [!NOTE]
   >
   > 仮想ネットワーク ゲートウェイの作成には、最大で 45 分かかる場合があります。

## タスク 8: CoreServicesVnet から ManufacturingVnet に接続する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

1. [仮想ネットワーク ゲートウェイ] で、**[CoreServicesVnetGateway]** を選択します。

1. CoreServicesGateway で、**[接続]** を選択し、**[+ 追加]** を選択します。

   > [!NOTE]
   >
   >  仮想ネットワーク ゲートウェイが完全にデプロイされるまで、この構成を完了することはできません。

1. 次の表の情報を使用して、接続を作成します。

   | **オプション**                     | **Value**                         |
   | ------------------------------ | --------------------------------- |
   | 名前                           | CoreServicesGW-to-ManufacturingGW |
   | 接続の種類                | VNet 間                      |
   | 最初の仮想ネットワーク ゲートウェイ  | CoreServicesVnetGateway           |
   | 2 番目の仮想ネットワーク ゲートウェイ | ManufacturingVnetGateway          |
   | 共有キー (PSK)               | abc123                            |
   | Azure プライベート IP アドレスを使用します。   | オフ                      |
   | BGP を有効にする                     | オフ                      |
   | IKE プロトコル                   | IKEv2                             |
   | サブスクリプション                   | 変更は必要ありません               |
   | Resource group                 | 変更は必要ありません               |
   | 場所                       | 米国東部                           |

1. 接続を作成するには、 **[OK]** を選択します。

## タスク 9: ManufacturingVnet を CoreServicesVnet に接続する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

1. [仮想ネットワーク ゲートウェイ] で、**[ManufacturingVnetGateway]** を選択します。

1. CoreServicesGateway で、**[接続]** を選択し、**[+ 追加]** を選択します。

1. 次の表の情報を使用して、接続を作成します。

   | **オプション**                     | **Value**                         |
   | ------------------------------ | --------------------------------- |
   | 名前                           | ManufacturingGW-to-CoreServicesGW |
   | 接続の種類                | VNet 間                      |
   | 最初の仮想ネットワーク ゲートウェイ  | ManufacturingVnetGateway          |
   | 2 番目の仮想ネットワーク ゲートウェイ | CoreServicesVnetGateway           |
   | 共有キー (PSK)               | abc123                            |
   | Azure プライベート IP アドレスを使用します。   | オフ                      |
   | BGP を有効にする                     | オフ                      |
   | IKE プロトコル                   | IKEv2                             |
   | サブスクリプション                   | 変更は必要ありません               |
   | Resource group                 | 変更は必要ありません               |
   | 場所                       | 西ヨーロッパ                       |

1. 接続を作成するには、 **[OK]** を選択します。

## タスク 10: 接続が接続されていることを確認する

1. **[リソース、サービス、ドキュメント (G+/) の検索]** で、**[接続]** を入力し、結果から **[接続]** を選択します。

1. 両方の接続の状態が **[接続済み]** になるまで待ちます。 場合によっては、スクリーンを更新する必要があります。

   ![VPN Gateway の接続が正常に作成されました。](../media/connections-status-connected.png)

## タスク 11: VM 間の接続をテストする

1. **ManufacturingVM** で、PowerShell を開きます。

1. 次のコマンドを使用して、CoreServicesVnet に CoreServicesVM への接続が確立されたことを確認します。 CoreServicesVM には必ず IPv4 アドレスを使用してください。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. テスト接続が成功すると、次のような結果が表示されます。

   ![Test-NetConnection が成功しました。](../media/test-connection-succeeded.png)

1. リモート デスクトップ接続のウィンドウを閉じます。

おめでとうございます! 仮想ネットワーク ゲートウェイを使用して VNet 間接続を構成しました。
