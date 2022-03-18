---
Exercise:
  title: M01-ユニット 6 Azure で DNS 設定を構成する
  module: Module - Introduction to Azure Virtual Networks
ms.openlocfilehash: e9def7c4f9c73455fa951706fe250f55f5f79152
ms.sourcegitcommit: 7f291542d9277fb2ea77561bd0b097ffffbfb815
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/22/2022
ms.locfileid: "139132634"
---
# <a name="m01---unit-6-configure-dns-settings-in-azure"></a>M01 - ユニット 6 Azure で DNS 設定を構成する

## <a name="exercise-scenario"></a>演習のシナリオ 
このユニットでは、Contoso Ltd の DNS 名前解決を構成します。contoso.com という名前のプライベート DNS ゾーンを作成し、登録と解決のために VNet をリンクした後、2 つの仮想マシンを作成して構成をテストします。

この演習では、以下のことを行います。

+ タスク 1: プライベート DNS ゾーンを作成する
+ タスク 2: 自動登録のためにサブネットをリンクする
+ タスク 3: 構成をテストするための仮想マシンを作成する
+ タスク 4: DNS ゾーンにレコードが存在することを確認する


## <a name="task-1-create-a-private-dns-zone"></a>タスク 1: プライベート DNS ゾーンを作成する

1. [Azure Portal](https://portal.azure.com/) に移動します。

2. Azure ホーム ページの検索バーに「dns」と入力し、 **[プライベート DNS ゾーン]** を選択します。  
   ‎![DNS 検索を使用した Azure portal ホーム ページ。](../media/create-private-dns-zone.png)

3. [プライベート DNS ゾーン] で **[+ 作成]** を選択します。

4. 次の表の情報を使用して、プライベート DNS ゾーンを作成します。

| **Tab**         | **オプション**                             | **Value**            |
| --------------- | -------------------------------------- | -------------------- |
| 基本          | Resource group                         | ContosoResourceGroup |
|                 | Name                                   | Contoso.com          |
| タグ            | 変更は必要ありません                    |                      |
| [確認および作成] | 設定を確認し、[作成] を選択します |                      |


5. デプロイが完了するまで待ち、**[リソースに移動]** を選択します。

6. ゾーンが作成されたことを確認します。

## <a name="task-2-link-subnet-for-auto-registration"></a>タスク 2: 自動登録のためにサブネットをリンクする

1. contoso.com の **[設定]** で、**[仮想ネットワーク リンク]** を選択します。

2. [contoso.com |仮想ネットワーク リンク] で、**[+ 追加]** を選択します。

![[+ 追加] が強調されている [contoso.com |仮想ネットワーク リンク]。](../media/add-network-link-dns.png)

3. 次の表の情報を使用して、仮想ネットワーク リンクを追加します。

| **オプション**                          | **Value**                               |
| ----------------------------------- | --------------------------------------- |
| リンク名                           | CoreServicesVnetLink                    |
| サブスクリプション                        | 変更は必要ありません                     |
| Virtual Network                     | CoreServicesVnet (ContosoResourceGroup) |
| 自動登録を有効にする            | オン                                |
| 設定を確認し、[OK] を選択します。 |                                         |


4. **[最新の情報に更新]** を選択します。

5. CoreServicesVnetLink が作成され、自動登録が有効になっていることを確認します。

6. 次の表の情報を使用して、ManufacturingVnet にステップ 2 から 5 を繰り返します。 

| **オプション**                          | **Value**                                |
| ----------------------------------- | ---------------------------------------- |
| リンク名                           | ManufacturingVnetLink                    |
| サブスクリプション                        | 変更は必要ありません                      |
| Virtual Network                     | ManufacturingVnet (ContosoResourceGroup) |
| 自動登録を有効にする            | オン                                 |
| 設定を確認し、[OK] を選択します。 |                                          |


7. **[最新の情報に更新]** を選択します。

8. ManufacturingVnetLink が作成され、自動登録が有効になっていることを確認します。

9. 次の表の情報を使用して、ResearchVnet にステップ 2 から 5 を繰り返します。 

| **オプション**                          | **Value**                           |
| ----------------------------------- | ----------------------------------- |
| リンク名                           | ResearchVnetLink                    |
| サブスクリプション                        | 変更は必要ありません                 |
| Virtual Network                     | ResearchVnet (ContosoResourceGroup) |
| 自動登録を有効にする            | オン                            |
| 設定を確認し、[OK] を選択します。 |                                     |


10. **[最新の情報に更新]** を選択します。

11. ResearchVnetLink が作成され、自動登録が有効になっていることを確認します。

 

##  <a name="task-3-create-virtual-machines-to-test-the-configuration"></a>タスク 3: 構成をテストするための仮想マシンを作成する

このセクションでは、2 つのテスト VM を作成して、プライベート DNS ゾーンの構成をテストします。

1. Azure portal で、**[Cloud Shell]** ペイン内に **PowerShell** セッションを開きます。

2. [Cloud Shell] ウィンドウのツール バーで、[ファイルのアップロード/ダウンロード] アイコンを選択し、ドロップダウン メニューで [アップロード] を選択して、**azuredeploy.json** および **azuredeploy.parameters.json** の各ファイルをソース フォルダー **F:\Allfiles\Exercises\M01** から Cloud Shell のホーム ディレクトリにアップロードします。

3. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```
  
4. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

5. 両方の仮想マシンが作成されていることを確認します。

 

## <a name="task-4-verify-records-are-present-in-the-dns-zone"></a>タスク 4: DNS ゾーンにレコードが存在することを確認する

1. Azure portal のホーム ページで、「**プライベート DNS ゾーン**」を選択します。

2. [プライベート DNS ゾーン] で、**contoso.com** を選択します。

3. 次に示すように、両方の VM でホスト (A) レコードが一覧に表示されることを確認します。

![自動登録されたホスト A レコードが示されている contoso.com の DNS ゾーン。](../media/contoso_com-dns-zone.png)

 

4. VM の名前と IP アドレスを記録しておきます。

 

### <a name="connect-to-the-test-vms-using-rdp"></a>RDP を使用してテスト VM に接続する

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

2. **TestVM1** を選択します。

3. TestVM1 で、**[接続] &gt; [RDP]** を選択します。

![[接続] と [RDP] が強調されている TestVM1。](../media/connect-to-am.png)

4. [TestVM1 | 接続] で、**[RDP ファイルのダウンロード]** を選択します。

5. RDP ファイルをデスクトップに保存します。

6. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

7. **TestVM2** を選択します。

8. TestVM2 で、**[接続] &gt; [RDP]** を選択します。

9. [TestVM2 | 接続] で、**[RDP ファイルのダウンロード]** を選択します。

10. RDP ファイルをデスクトップに保存します。

11. この RDP ファイルと、ユーザー名 **TestUser**、パスワード **TestPa$$w0rd!** を使って TestVM1 に接続します。

12. この RDP ファイルと、ユーザー名 **TestUser**、パスワード **TestPa$$w0rd!** を使って TestVM2 に接続します。

13. 両方の VM で、**[デバイスのプライバシー設定の選択]** の **[同意する]** を選択します。

14. 両方の VM 上で、確認を求められたら **[ネットワーク]** で **[はい]** を選びます。

15. TestVM1 でコマンド プロンプトを開き、コマンド「ipconfig /all」を入力します。

16. IP アドレスが、DNS ゾーンで記録したものと同じであることを確認します。

17. コマンド「ping TestVM2.contoso.com」を入力します。

18. FQDN がプライベート DNS ゾーンでメモした IP アドレスに解決されていることを確認します。 VM で Windows ファイアウォールが有効になっているため、ping 自体はタイムアウトします。

19. または、nslookup TestVM2.contoso.com コマンドを入力して、VM2 の名前解決の成功レコードを受信したことを確認します
 

お疲れさまでした。 プライベート DNS ゾーンを作成し、名前解決と自動登録リンクを追加し、構成での名前解決をテストしました。 
