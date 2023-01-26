---
Exercise:
  title: M06 - ユニット 4 Azure portal を使用して仮想ネットワーク上に DDoS Protection を構成する
  module: Module 06 - Design and implement network security
---

# <a name="m06-unit-4-configure-ddos-protection-on-a-virtual-network-using-the-azure-portal"></a>M06-ユニット 4 Azure portal を使用して仮想ネットワーク上に DDoS Protection を構成する

Contoso のネットワーク セキュリティ チームを担当しているあなたは、仮想ネットワークで模擬 DDoS 攻撃を実行しようとしています。 次の手順では、仮想ネットワークを作成し、DDoS Protection を構成し、テレメトリとメトリックを使用して観察および監視できる攻撃を作成する手順について説明します。

この演習では、以下のことを行います。

+ タスク 1: リソース グループを作成する
+ タスク 2: DDoS Protection プランを作成する
+ タスク 3: 新しい仮想ネットワークで DDoS Protection を有効にする
+ タスク 4: DDoS テレメトリを構成する
+ タスク 5: DDoS 診断ログを構成する
+ タスク 6: DDoS アラートを構成する
+ タスク 7: DDoS サービス要求を送信して DDoS 攻撃を実行する
+ タスク 8: リソースをクリーンアップする



#### <a name="estimated-time-40-minutes"></a>予想所要時間: 40 分

## <a name="task-1-create-a-resource-group"></a>タスク 1: リソース グループを作成する

1. Azure アカウントにログインします。

2. Azure portal のホーム ページで、**[リソース グループ]** を選択します。

3. **［作成］** を選択します 

4. **[基本]** タブの **[リソース グループ]** に「**MyResourceGroup**」と入力します。

5. **[リージョン]** で [米国東部] を選択します。

6. **[Review + create](レビュー + 作成)** を選択します。

7. **［作成］** を選択します



## <a name="task-2-create-a-ddos-protection-plan"></a>タスク 2: DDoS Protection プランを作成する

1. Azure portal のホームページで、検索ボックスに「**DDoS**」と入力し、表示された **[DDoS 保護プラン]** を選びます。

2. **[+ 作成]** を選択します。

3. **[基本]** タブの **[リソース グループ]** の一覧で、先ほど作成したリソース グループを選択します。

4. **[インスタンス名]** ボックスに「**MyDdoSProtectionPlan**」と入力し、 **[確認および作成]** を選びます。

5. **［作成］** を選択します

 

## <a name="task-3-enable-ddos-protection-on-a-new-virtual-network"></a>タスク 3: 新しい仮想ネットワークで DDoS Protection を有効にする

ここでは、既存の仮想ネットワークではなく新しい仮想ネットワークで DDoS を有効にします。そのため、最初に新しい仮想ネットワークを作成してから、先ほど作成したプランを使用して DDoS Protection を有効にする必要があります。

1. Azure portal のホーム ページで **[リソースの作成]** を選択し、検索ボックスに「**仮想ネットワーク**」と入力して、表示されたら **[仮想ネットワーク]** を選択します。

2. **[仮想ネットワーク]** ページで、 **[作成]** を選択します。

3. **[基本]** タブで、前に作成したリソース グループを選択します。

4. **[名前]** ボックスに「**MyVirtualNetwork**」と入力し、**[セキュリティ]** タブをクリックします。 

5. **[セキュリティ]** タブで、**[DDoS Protection Standard]** の横にある **[有効]** を選択します。

6. **[DDoS 保護計画]** ドロップダウン リストで、**MyDdosProtectionPlan** を選択します。

   ![[仮想ネットワークの作成] - [セキュリティ] タブ](../media/create-virtual-network-security-for-ddos-protection.png)

7. **[Review + create](レビュー + 作成)** をクリックします。

8. **Create** をクリックしてください。

 

## <a name="task-4-configure-ddos-telemetry"></a>タスク 4: DDoS テレメトリを構成する

パブリック IP アドレスを作成し、次のステップでテレメトリを設定します。

1. Azure portal のホーム ページで **[リソースの作成]** を選択し、検索ボックスに「**パブリック IP**」と入力して、**[パブリック IP アドレス]** が表示されたらクリックします。

2. **[パブリック IP アドレス]** ページで、**[作成]** をクリックします。

3. **[パブリック IP アドレスの作成]** ページの **[SKU]** で、**[Basic]** を選択します。

4. **[名前]** ボックスに「**MyPublicIPAddress**」と入力します。

5. **[IP アドレスの割り当て]** で、**[静的]** を選択します。

6. **[DNS 名ラベル]** に「**mypublicdnsxx**」と入力します (xx は、これを一意にするためのユーザーのイニシャルです)。

7. 一覧から使用するリソース グループを選択します。

   ![パブリック IP アドレスの作成](../media/create-public-ip-address-for-ddos-telemetry.png)

8. **[作成]** をクリックします。

9. Azure のホーム ページで、**[すべてのリソース]** をクリックします。

10. リソースの一覧で、**MyDdosProtectionPlan** をクリックします。

11. **[監視]** で **[メトリック]** を選びます。

12. **[スコープ]** ボックスを選択し、**MyPublicIPAddress** の横にあるチェック ボックスをオンにします。

    ![DDoS テレメトリのメトリック スコープを作成します](../media/create-metrics-scope-for-ddos-telemetry.png)

13. **[適用]** をクリックします。

14. **[メトリック]** ボックスで、**[Inbound packets dropped DDoS](DDoS で破棄されたインバウンド パケット数)** を選択します。

15. **[集計]** ボックスで、**[最大]** を選択します。

    ![DDoS テレメトリ用に作成されたメトリック](../media/metrics-created-for-ddos-telemetry.png)

 

## <a name="task-5-configure-ddos-diagnostic-logs"></a>タスク 5: DDoS 診断ログを構成する

1. Azure のホーム ページで、**[すべてのリソース]** をクリックします。

2. リソースの一覧で **MyPublicIPAddress** をクリックします。

3. **[監視]** で **[診断設定]** を選択します。

4. **[診断設定の追加]** をクリックします。 

5. **[診断設定]** ページの **[診断設定の名前]** ボックスに、「**MyDiagnosticSetting**」と入力します。 

6. **[カテゴリの詳細]** で、3 つの**ログ**すべてのチェック ボックスと **[AllMetrics]** チェック ボックスをオンにします。

7. **[宛先の詳細]** で、**[Send to Log Analytics workspace](Log Analytics ワークスペースに送信する)** チェック ボックスをオンにします。 ここでは、既存の Log Analytics ワークスペースを選択することもできますが、診断ログの宛先をまだ設定していないので、設定を入力しても、この演習の次のステップでそれらを破棄します。

   ![DDoS の新しい診断設定を構成します](../media/configure-ddos-diagnostic-settings-new.png)

8. 通常は、ここで **[保存]** をクリックして診断設定を保存します。 設定の構成をまだ完了できないため、このオプションは淡色表示になっています。

9. **[破棄]** をクリックし、**[はい]** をクリックします。

 

## <a name="task-6-configure-ddos-alerts"></a>タスク 6: DDoS アラートを構成する

このステップでは、仮想マシンを作成し、パブリック IP アドレスをそれに割り当ててから、DDoS アラートを構成します。

### <a name="create-the-vm"></a>VM の作成

1. Azure portal のホーム ページで **[リソースの作成]** を選択し、検索ボックスに「**仮想マシン**」と入力して、表示されたら **[仮想マシン]** をクリックします。

2. **[仮想マシン]** ページで、**[作成]** をクリックします。

3. **[基本]** タブで、次の表の情報を使用して新しい VM を作成します。

   | **設定**           | **Value**                                                    |
   | --------------------- | ------------------------------------------------------------ |
   | サブスクリプション          | サブスクリプションを選択します。                                     |
   | Resource group        | **MyResourceGroup**                                          |
   | 仮想マシン名  | **MyVirtualMachine**                                         |
   | リージョン                | 自分のリージョン                                                  |
   | 可用性のオプション  | **インフラストラクチャ冗長は必要ありません**                   |
   | イメージ                 | **Ubuntu Server 18.04 LTS - Gen 1** (必要な場合は、[VM の世代の構成] リンクを選びます) |                     
   | サイズ                  | **[See all sizes](すべてのサイズを表示)** を選択し、一覧で **[B1ls]** を選択して、 **[選択]** を選択します (**Standard_B1ls - 1 vcpu, 0.5 GiB memory**) |
   | 認証の種類   | **SSH 公開キー**                                           |
   | ユーザー名              | **azureuser**                                                |
   | SSH 公開キーのソース | **新しいキーの組の生成**                                    |
   | キーの組名         | **myvirtualmachine-ssh-key**                                 |


4. **[Review + create](レビュー + 作成)** をクリックします。

5. **Create** をクリックしてください。

6. **[新しいキーの組を生成]** ダイアログ ボックスで、**[Download private key and create resource](秘密キーをダウンロードしてリソースを作成する)** をクリックします。

7. 秘密キーを保存します。

8. デプロイが完了したら、 **[リソースに移動]** をクリックします。

### <a name="assign-the-public-ip-address"></a>パブリック IP アドレスを割り当てる

1. 新しい仮想マシンの **[概要]** ページで、**[設定]** の下にある **[ネットワーク]** をクリックします。

2. **[ネットワーク インターフェイス]** の横にある **[myvirtualmachine-nic]** をクリックします。 NIC の名前は異なる場合があります。

3. **[設定]** で **[IP 構成]** をクリックします。

4. **ipconfig1** を選択します。

5. **[パブリック IP アドレス]** の一覧で、**MyPublicIPAddress** を選択します。

6. **[保存]** をクリックします。

   ![DDoS VM のパブリック IP アドレスを変更します](../media/change-public-ip-config-for-ddos-vm-new.png)

### <a name="configure-ddos-alerts"></a>DDoS アラートを構成する

1. Azure のホーム ページで、**[すべてのリソース]** をクリックします。

2. リソースの一覧で、**MyDdosProtectionPlan** をクリックします。

3. **[監視]** で **[アラート]** を選択します。

4. **[新しいアラート ルール]** をクリックします。

5. **[アラート ルールの作成]** ページの **[スコープ]** で、**[リソースの編集]** をクリックします。

6. **[リソースの選択]** ペインの **[リソースの種類でフィルター]** ボックスで、一覧を下にスクロールし、**[パブリック IP アドレス]** を選択します。

   ![新しいアラート ルールのスコープをパブリック IP アドレスに変更します](../media/new-alert-rule-change-scope-to-public-ip-address-1.png)

7. **[リソース]** の一覧で **MyPublicIPAddress** を選択し、**[完了]** をクリックします。

8. **[アラート ルールの作成]** ページの **[条件]** で、**[条件の追加]** をクリックします。

9. **[DDoS 攻撃中かどうか]** を選択します。

   ![アラート ルールに条件を追加します - シグナルを選択します](../media/add-condition-to-alert-rule-1.png)

10. **[演算子]** ボックスで、**[以上]** を選択します。

11. **[しきい値]** に「**1**」と入力します (攻撃を受けている場合を意味します)。

12. **[Done]** をクリックします。

    ![アラート ルールに条件を追加します - シグナルのロジックを構成します](../media/add-condition-to-alert-rule-2.png)

13. **[アラート ルールの作成]** ページに戻り、 **[アラート ルールの詳細]** セクションの **[アラート ルール名]** に「**MyDdosAlert**」と入力します。

    ![新しいアラート ルールの作成の終了点](../media/new-alert-rule-end.png)

14. **[アラート ルールの作成]** をクリックします。

 

## <a name="task-7-submit-a-ddos-service-request-to-run-a-ddos-attack"></a>タスク 7: DDoS サービス要求を送信して DDoS 攻撃を実行する

1. [BreakingPoint Cloud](https://breakingpoint.cloud/) でアカウントを作成します

2. 下のスクリーンショットの設定に従って DDoS テストを設定しますが (試用アカウントで 100k pps テスト サイズを選ぶことが必要な場合があります)、 **[ターゲット IP アドレス]** ボックスでは自分の **MyPublicIPAddress** リソースの IP アドレスを指定します (例: **51.140.137.219**) ![DDOSAttack](https://user-images.githubusercontent.com/46939028/138599420-58bef33a-2597-4fa2-919f-bf1614037bc3.JPG)

   ![DDoS テストの設定](../media/ddos-test-setup.png)

3. Azure portal のホーム ページで、**[すべてのリソース]** をクリックします。

4. リソースの一覧で **MyPublicIPAddress** リソースをクリックし、**[監視]** で **[メトリック]** をクリックします。 

5. **[メトリック]** ボックスで、一覧から **[Under DDoS attack or not](DDoS 攻撃の有無)** を選択します。

6. これで、発生している DDoS 攻撃を見ることができるようになります。 結果が表示されるまでに、10 分ほどかかる場合があることに注意してください。

   ![DDoS 攻撃を受けているリソースを示すメトリック](../media/metrics-showing-resource-under-attack.png)

 
## <a name="task-8-clean-up-resources"></a>タスク 8: リソースをクリーンアップする

>**注**:新規に作成し、使用しなくなったすべての Azure リソースを削除することを忘れないでください。 使用していないリソースを削除することで、予期しない料金が発生しなくなります。

1. Azure portal で、**[Cloud Shell]** ペイン内に **PowerShell** セッションを開きます。

1. 次のコマンドを実行して、このモジュールのラボ全体を通して作成したすべてのリソース グループを削除します。

   ```powershell
   Remove-AzResourceGroup -Name 'MyResourceGroup' -Force -AsJob
   ```

    >**注**:このコマンドは非同期で実行されるため (-AsJob パラメーターによって決定されます)、同じ PowerShell セッション内で直後に別の PowerShell コマンドを実行できますが、リソース グループが実際に削除されるまでに数分かかります。
