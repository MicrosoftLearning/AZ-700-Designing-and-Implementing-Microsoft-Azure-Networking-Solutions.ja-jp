---
Exercise:
  title: M02 - ユニット 7 Azure portal を使用して仮想 WAN を作成する
  module: Module 02 - Design and implement hybrid networking
---

# <a name="m02-unit-7-create-a-virtual-wan-by-using-azure-portal"></a>M02-ユニット 7 Azure portal を使用して仮想 WAN を作成する


この演習では、Contoso 用の Virtual WAN を作成します。

この演習では、以下のことを行います。

+ タスク 1: Virtual WAN を作成する
+ タスク 2:Azure portal を使用してハブを作成する
+ タスク 3: 仮想ハブに VNet を接続する
+ タスク 4: リソースをクリーンアップする



#### <a name="estimated-time-65-minutes-including-45-minutes-deployment-waiting-time"></a>推定時間: 65 分 (最大 45 分のデプロイ待機時間を含む)

## <a name="task-1-create-a-virtual-wan"></a>タスク 1: Virtual WAN を作成する

1. ブラウザーから Azure portal に移動し、Azure アカウントでサインインします。

2. ポータルで、検索ボックスに「Virtual WAN」と入力し、結果リストから「**Virtual WAN**」を選択します。

   ![Azure portal で仮想ネットワークを検索します。](../media/search-for-virtual-wan.png)

 

3. [Virtual WAN] ページで **[+ 作成]** を選択します。 

4. [WAN の作成] ページの **[基本]** タブで、次のフィールドに入力します。

   - **サブスクリプション:** 既存のサブスクリプションを使用する

   - **リソース グループ:** ContosoResourceGroup

   - **リソース グループの場所:** ドロップダウンからリソースの場所を選択します。 WAN はグローバルなリソースであり、特定のリージョンに存在するものではありません。 ただし、作成した WAN リソースを管理および検索するために、リージョンを選択する必要があります。

   - **名前:** ContosoVirtualWAN

   - **種類:** Standard 

5. フィールドへの入力が終了したら、**[確認と作成]** を選択します。

6. 検証に合格したら、**[作成]** を選択して Virtual WAN を作成します。

## <a name="task-2-create-a-hub-by-using-azure-portal"></a>タスク 2:Azure portal を使用してハブを作成する

ハブには、サイト間、ExpressRoute、またはポイント対サイト機能のためのゲートウェイを含めることができます。 仮想ハブ内にサイト間 VPN ゲートウェイを作成するには 30 分かかります。 ハブを作成する前に、Virtual WAN を作成する必要があります。

1. 作成した仮想 WAN を探します。 
2. [Virtual WAN] ページの **[接続]** で、**[ハブ]** を選択します。
3. [ハブ] ページで、**[+ 新しいハブ]** を選択して [仮想ハブを作成する] ページを開きます。
   ![[仮想ハブの作成]、[基本] タブ。](../media/create-vwan-hub.png)
4. [仮想ハブを作成する] ページの **[基本]** タブで、次のフィールドを入力します。
   - **リージョン:** 米国西部
   - **名前:** ContosoVirtualWANHub-WestUS
   - **ハブ プライベート アドレス空間:** 10.60.0.0/24
   - **仮想ハブの容量:** 2 ルーティング インフラストラクチャ ユニット
5. **サイト間** を選択します。
6. **[サイト間]** タブで、次のフィールドを入力します。
   - **サイト間 (VPN Gateway) を作成しますか?:** はい
   - **[AS 番号]** フィールドは編集できません。
   - **ゲートウェイのスケール ユニット:** 1 スケール ユニット = 500 Mbps x 2
7. **[確認と作成]** を選択して検証します。
8. **[作成]** を選択してハブを作成します。 
9. 30 分後に **[更新]** を実行して、 [ハブ] ページでハブを確認します。 

## <a name="task-3-connect-a-vnet-to-the-virtual-hub"></a>タスク 3: 仮想ハブに VNet を接続する

1. 作成した仮想 WAN を探します。 

2. ContosoVirtualWAN の **[接続]** で、**[仮想ネットワーク接続]** を選択します。

   ![仮想ネットワーク接続が強調表示されている Virtual WAN 構成ページ。](../media/connect-vnet-to-virtual-hub.png)

3. [ContosoVirtualWAN | 仮想ネットワーク接続] で、**[+ 接続の追加]** を選択します。

4. [接続の追加] で、次の情報を使用して接続を作成します。

   - **接続名:** ContosoVirtualWAN-to-ResearchVNet

   - **ハブ:** ContosoVirtualWANHub-WestUS

   - **サブスクリプション:** 変更なし

   - **リソース グループ:** ContosoResourceGroup

   - **仮想ネットワーク:** ResearchVNet

   - **伝達しない:** はい

   - **ルート テーブルを関連付ける:** 既定値

5. **［作成］** を選択します

 

お疲れさまでした。 Virtual WAN と Virtual WAN ハブを作成し、ResearchVNet をハブに接続しました。

## <a name="task-4-clean-up-resources"></a>タスク 4: リソースをクリーンアップする

   >**注**:新規に作成し、使用しなくなったすべての Azure リソースを削除することを忘れないでください。 使用していないリソースを削除することで、予期しない料金が発生しなくなります。

1. Azure portal で、**[Cloud Shell]** ペイン内に **PowerShell** セッションを開きます。

1. 次のコマンドを実行して、このモジュールのラボ全体を通して作成したすべてのリソース グループを削除します。

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```

    >**注**:このコマンドは非同期で実行されるため (-AsJob パラメーターによって決定されます)、同じ PowerShell セッション内で直後に別の PowerShell コマンドを実行できますが、リソース グループが実際に削除されるまでに数分かかります。
