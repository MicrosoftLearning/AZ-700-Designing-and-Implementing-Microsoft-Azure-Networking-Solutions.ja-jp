---
Exercise:
  title: M02 - ユニット 3 仮想ネットワーク ゲートウェイを作成および構成する
  module: Module 02 - Design and implement hybrid networking
---


# <a name="m02-unit-3-create-and-configure-a-virtual-network-gateway"></a>M02-ユニット 3 仮想ネットワーク ゲートウェイを作成および構成する

この演習では、Contoso Core Services VNet と Manufacturing VNet を接続する仮想ネットワーク ゲートウェイを構成します。 

この演習では、以下のことを行います。

+ タスク 1: CoreServicesVnet と ManufacturingVnet を作成する
+ タスク 2:CoreServicesVM を作成する
+ タスク 3:ManufacturingVM を作成する
+ タスク 4: RDP を使用してテスト VM に接続する
+ タスク 5: VM 間の接続をテストする
+ タスク 6: CoreServicesVnet ゲートウェイを作成する
+ タスク 7: ManufacturingVnet ゲートウェイを作成する
+ タスク 8: CoreServicesVnet から ManufacturingVnet 
+ タスク 9: ManufacturingVnet を CoreServicesVnet に接続する
+ タスク 10: 接続が接続されていることを確認する 
+ タスク 11: VM 間の接続をテストする

#### <a name="estimated-time-70-minutes-including-45-minutes-deployment-waiting-time"></a>推定時間: 70 分 (最大 45 分のデプロイ待機時間を含む)

## <a name="task-1-create-coreservicesvnet-and-manufacturingvnet"></a>タスク 1: CoreServicesVnet と ManufacturingVnet を作成する

1. Azure portal で、**[Cloud Shell]** ウィンドウ内に **PowerShell** セッションを開きます。

2. [Cloud Shell] ペインのツール バーで、[ファイルのアップロード/ダウンロード] アイコンをクリックし、ドロップダウン メニューで [アップロード] をクリックして、**azuredeploy.json** および **azuredeploy.parameters.json** の各ファイルをソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします

3. 次の ARM テンプレートをデプロイして、この演習に必要な仮想ネットワークとサブネットを作成します。

   ```powershell
   $RGName = "ContosoResourceGroup"
   #create resource group if it doesnt exist
   New-AzResourceGroup -Name $RGName -Location "eastus"
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```

## <a name="task-2-create-coreservicesvm"></a>タスク 2:CoreServicesVM を作成する

1. Azure portal で、**[Cloud Shell]** ペイン内に **PowerShell** セッションを開きます。

2. [Cloud Shell] ペインのツールバーで、[ファイルのアップロード/ダウンロード] アイコンをクリックし、ドロップダウン メニューで [アップロード] をクリックして、**CoreServicesVMazuredeploy.json** および **CoreServicesVMazuredeploy.parameters.json** の各ファイルをソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします。

3. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
   ```
  
4. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

5. 仮想マシンが作成されていることを確認してください。

## <a name="task-3-create-manufacturingvm"></a>タスク 3:ManufacturingVM を作成する

1. Azure portal で、**[Cloud Shell]** ペイン内に **PowerShell** セッションを開きます。

2. [Cloud Shell] ペインのツールバーで、[ファイルのアップロード/ダウンロード] アイコンをクリックし、ドロップダウン メニューで [アップロード] をクリックして、**ManufacturingVMazuredeploy.json** および **ManufacturingVMazuredeploy.parameters.json** の各ファイルをソース フォルダー **F:\Allfiles\Exercises\M02** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします。

3. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
4. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

5. 仮想マシンが作成されていることを確認してください。


## <a name="task-4-connect-to-the-test-vms-using-rdp"></a>タスク 4: RDP を使用してテスト VM に接続する

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。
2. **ManufacturingVM** を選択します。
3. **ManufacturingVM** で **[接続] &gt; [RDP]** の順に選択します。
4. **[ManufacturingVM | 接続]** で **[RDP ファイルのダウンロード]** を選択します。
5. RDP ファイルをデスクトップに保存します。
6. この RDP ファイル、**TestUser** のユーザー名、**TestPa$$w0rd!** のパスワードを使用し、ManufacturingTestVM に接続します。 接続後、RDP セッションを最小化します。
7. Azure portal のホーム ページで、「**仮想マシン**」を選択します。
8. **[CoreServicesVM]** を選択します。
9. **[CoreServicesVM]** で **[接続] &gt; [RDP]** の順に接続します。
10. **[CoreServicesVM | 接続]** で、 **[RDP ファイルのダウンロード]** を選択します。
11. RDP ファイルをデスクトップに保存します。
12. この RDP ファイル、**TestUser** のユーザー名、**TestPa$$w0rd!** のパスワードを使用し、CoreServicesTestVM に接続します。
13. 両方の VM で、**[デバイスのプライバシー設定の選択]** の **[同意する]** を選択します。
14. 両方の VM で、**[ネットワーク]** の **[はい]** を選択します。
15. CoreServicesTestVM で、PowerShell を開き、次のコマンドを実行します: ipconfig
16. IPv4 アドレスをメモします。 

 

## <a name="task-5-test-the-connection-between-the-vms"></a>タスク 5: VM 間の接続をテストする

1. **ManufacturingVM** で、PowerShell を開きます。

2. 次のコマンドを使用して、CoreServicesVnet に CoreServicesVM への接続がないことを確認します。 CoreServicesVM には必ず IPv4 アドレスを使用してください。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

3. テスト接続が失敗すると、次のような結果が表示されます。

   ![Test-NetConnection が失敗しました。](../media/test-netconnection-fail.png)

 

##  <a name="task-6-create-coreservicesvnet-gateway"></a>タスク 6: CoreServicesVnet ゲートウェイを作成する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。
   ![Azure Portal で仮想ネットワーク ゲートウェイを検索します。](../media/virtual-network-gateway-search.png)

2. [仮想ネットワーク ゲートウェイ] で、**[+ 作成]** を選択します。

3. 次の表の情報を使用して、仮想ネットワーク ゲートウェイを作成します。

   | **Tab**         | **セクション**       | **オプション**                                  | **Value**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本          | プロジェクトの詳細   | サブスクリプション                                | 変更は必要ありません          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | インスタンスの詳細  | Name                                        | CoreServicesVnetGateway      |
   |                 |                   | リージョン                                      | 米国東部                      |
   |                 |                   | ゲートウェイの種類                                | VPN                          |
   |                 |                   | VPN の種類                                    | ルート ベース                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation1                  |
   |                 |                   | 仮想ネットワーク                             | CoreServicesVnet             |
   |                 |                   | Subnet                                      | GatewaySubnet (10.20.0.0/27) |
   |                 |                   | パブリック IP アドレスの種類                      | 基本                        |
   |                 | パブリック IP アドレス | パブリック IP アドレス                           | 新規作成                   |
   |                 |                   | パブリック IP アドレス名                      | CoreServicesVnetGateway-ip   |
   |                 |                   | アクティブ/アクティブ モードの有効化                   | 無効                     |
   |                 |                   | BGP の構成                               | 無効                     |
   | [確認および作成] |                   | 設定を確認し、 **[作成]** を選択します。 |                              |

   > [!NOTE] 
   >
   > 仮想ネットワーク ゲートウェイの作成には、最大で 45 分かかる場合があります。 

## <a name="task-7-create-manufacturingvnet-gateway"></a>タスク 7: ManufacturingVnet ゲートウェイを作成する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

2. [仮想ネットワーク ゲートウェイ] で、**[+ 作成]** を選択します。

3. 次の表の情報を使用して、仮想ネットワーク ゲートウェイを作成します。

   | **Tab**         | **セクション**       | **オプション**                                  | **Value**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | 基本          | プロジェクトの詳細   | サブスクリプション                                | 変更は必要ありません          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | インスタンスの詳細  | Name                                        | ManufacturingVnetGateway     |
   |                 |                   | リージョン                                      | 西ヨーロッパ                  |
   |                 |                   | ゲートウェイの種類                                | VPN                          |
   |                 |                   | VPN の種類                                    | ルート ベース                  |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation1                  |
   |                 |                   | 仮想ネットワーク                             | ManufacturingVnet            |
   |                 |                   | Subnet                                      | GatewaySubnet (10.30.0.0/27) |
   |                 |                   | パブリック IP アドレスの種類                      | 基本                        |
   |                 | パブリック IP アドレス | パブリック IP アドレス                           | 新規作成                   |
   |                 |                   | パブリック IP アドレス名                      | ManufacturingVnetGateway-ip  |
   |                 |                   | アクティブ/アクティブ モードの有効化                   | 無効                     |
   |                 |                   | BGP の構成                               | 無効                     |
   | [確認および作成] |                   | 設定を確認し、 **[作成]** を選択します。 |                              |
   
   > [!NOTE]
   >
   > 仮想ネットワーク ゲートウェイの作成には、最大で 45 分かかる場合があります。 

 

## <a name="task-8-connect-coreservicesvnet-to-manufacturingvnet"></a>タスク 8: CoreServicesVnet から ManufacturingVnet に接続する 

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

2. [仮想ネットワーク ゲートウェイ] で、**[CoreServicesVnetGateway]** を選択します。

3. CoreServicesGateway で、**[接続]** を選択し、**[+ 追加]** を選択します。

   > [!NOTE]
   >
   >  仮想ネットワーク ゲートウェイが完全にデプロイされるまで、この構成を完了することはできません。

4. 次の表の情報を使用して、接続を作成します。

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

5. 接続を作成するには、 **[OK]** を選択します。
   

## <a name="task-9-connect-manufacturingvnet-to-coreservicesvnet"></a>タスク 9: ManufacturingVnet を CoreServicesVnet に接続する

1. **[リソース、サービス、ドキュメントの検索 (G+/)]** で、「**仮想ネットワーク ゲートウェイ**」 と入力し、結果から **[仮想ネットワーク ゲートウェイ]** を選択します。

2. [仮想ネットワーク ゲートウェイ] で、**[ManufacturingVnetGateway]** を選択します。

3. CoreServicesGateway で、**[接続]** を選択し、**[+ 追加]** を選択します。

4. 次の表の情報を使用して、接続を作成します。

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

5. 接続を作成するには、 **[OK]** を選択します。

## <a name="task-10-verify-that-the-connections-connect"></a>タスク 10: 接続が接続されていることを確認する 

1. **[リソース、サービス、ドキュメント (G+/) の検索]** で、**[接続]** を入力し、結果から **[接続]** を選択します。

2. 両方の接続の状態が **[接続済み]** になるまで待ちます。 場合によっては、スクリーンを更新する必要があります。 

   ![VPN Gateway の接続が正常に作成されました。](../media/connections-status-connected.png)

 

## <a name="task-11-test-the-connection-between-the-vms"></a>タスク 11: VM 間の接続をテストする

1. **ManufacturingVM** で、PowerShell を開きます。

2. 次のコマンドを使用して、CoreServicesVnet に CoreServicesVM への接続が確立されたことを確認します。 CoreServicesVM には必ず IPv4 アドレスを使用してください。

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

3. テスト接続が成功すると、次のような結果が表示されます。

   ![Test-NetConnection が成功しました。](../media/test-connection-succeeded.png)

4. リモート デスクトップ接続のウィンドウを閉じます。

おめでとうございます! 仮想ネットワーク ゲートウェイを使用して VNet 間接続を構成しました。
