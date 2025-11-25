---
Exercise:
  title: M01 - ユニット 6 Azure で DNS 設定を構成する
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 - ユニット 6 Azure で DNS 設定を構成する

## 演習のシナリオ

このユニットでは、Contoso Ltd の DNS 名前解決を構成します。contoso.com という名前のプライベート DNS ゾーンを作成し、登録と解決のために VNet をリンクした後、2 つの仮想マシンを作成して構成をテストします。

![DNS アーキテクチャの図。](../media/6-exercise-configure-domain-name-servers-configuration-azure.png)

### 職務スキル
この演習では、次のことを行います。

+ タスク 1: プライベート DNS ゾーンを作成する
+ タスク 2: 自動登録のためにサブネットをリンクする
+ タスク 3: 構成をテストするための仮想マシンを作成する
+ タスク 4: DNS ゾーンにレコードが存在することを確認する

### 推定時間: 25 分

**重要:** この演習には、前のラボの仮想ネットワークが必要です。 これは[テンプレート](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises/M01/template.json) ファイルを使用してインストールできます。 すべての仮想ネットワークがインストールされますが、必要なのは CoreServicesVNet のみです。 

## タスク 1: プライベート DNS ゾーンを作成する

1. [Azure Portal](https://portal.azure.com/) に移動します。

1. Azure ホーム ページの検索バーに「dns」と入力し、 **[プライベート DNS ゾーン]** を選択します。  
   ![DNS が検索されている Azure portal ホーム ページ](../media/create-private-dns-zone.png)

1. [プライベート DNS ゾーン] で **[+ 作成]** を選択します。

1. 次の表の情報を使用して、プライベート DNS ゾーンを作成します。

    | **Tab**         | **オプション**                             | **Value**            |
    | --------------- | -------------------------------------- | -------------------- |
    | 基本          | リソース グループ (必要に応じて新規に作成してください) | `ContosoResourceGroup` |
    |                 | Name                                   | `Contoso.com`          |
    | [確認および作成] | 設定を確認し、**[作成]** を選択します |                      |

1. デプロイが完了するまで待ち、**[リソースに移動]** を選択します。

1. ゾーンが作成されたことを確認します。

## タスク 2: 自動登録のためにサブネットをリンクする

1. Contoso.com の **[DNS 管理]** で、**[仮想ネットワーク リンク]** を選択します。

1. [contoso.com \| 仮想ネットワーク リンク] で、**[+ 追加]** を選択します。

    ![[+ 追加] が強調されている [contoso.com \| 仮想リンク]。](../media/add-network-link-dns.png)

1. 次の表の情報を使用して、仮想ネットワーク リンクを追加します。

    | **オプション**                          | **Value**                               |
    | ----------------------------------- | --------------------------------------- |
    | リンク名                           | `CoreServicesVnetLink`                   |
    | サブスクリプション                        | 変更は必要ありません                     |
    | Virtual Network                     | CoreServicesVnet (ContosoResourceGroup) |
    | 自動登録を有効にする            | 選択済み                                |
    | 設定を確認し、 **[作成]** を選択します。 |                                         |

1. **[最新の情報に更新]** を選択します。

1. CoreServicesVnetLink が作成され、自動登録が有効になっていることを確認します。

## タスク 3: 構成をテストするための仮想マシンを作成する

このセクションでは、2 つのテスト VM を作成して、プライベート DNS ゾーンの構成をテストします。

1. Azure portal で、右上の [Cloud Shell] アイコンを選択します。 必要に応じて、シェルを構成します。  
    + **[PowerShell]** を選択します。
    + **[ストレージ アカウントは必要ありません]** と **[サブスクリプション]** を選択してから、**[適用]** を選択します。
    + ターミナルが作成され、プロンプトが表示されるまで待ちます。 

1. Cloud Shell 画面のツール バーで、**[ファイルの管理]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**\\Allfiles\\Exercises\\M01\\** フォルダーの **azuredeploy.json** と **azuredeploy.parameters.json** というテンプレート ファイルをアップロードします。

   >**注:** 自分のサブスクリプションで作業している場合、[テンプレート ファイル](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises)は GitHub ラボ リポジトリで入手できます。
   
1. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   >**注**: 管理者パスワードを入力するように求められます。 このパスワードは後の手順で必要になります。 

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```
  
1. デプロイが完了したら、Azure portal のホーム ページに移動し、**[仮想マシン]** を選択します。

1. 両方の仮想マシンが作成されていることを確認します。

## タスク 4: DNS ゾーンにレコードが存在することを確認する

1. Azure portal のホーム ページで、「**プライベート DNS ゾーン**」を選択します。

1. [プライベート DNS ゾーン] で、**contoso.com** を選択します。

1. 次に示すように、両方の VM でホスト (A) レコードが一覧に表示されることを確認します。

    ![自動登録されたホスト A レコードが示されている contoso.com の DNS ゾーン。](../media/contoso_com-dns-zone.png)

1. VM の名前と IP アドレスを記録しておきます。

### VM に接続して名前解決をテストする

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

1. **TestVM1** を選択します。

1. TestVM1 で、**[接続]、[接続]** の順に選択し、RDP ファイルをダウンロードします。 ファイルが正常にダウンロードされたことを確認します。

1. RDP ファイルの場所に移動し、ダブルクリックしてファイルを実行します。

1. **[接続]** を選択し、テンプレートのデプロイ時に指定した **TestUser** のパスワードを指定します。

1. **[OK]** を選択して、警告ページで **[はい]** を選択します。

1. TestVM1 でコマンド プロンプトを開き、コマンド `ipconfig /all` を入力します。

1. IP アドレスが DNS ゾーンのものと同じであることを確認します。

1. `ping TestVM2.contoso.com` コマンドを入力します。

    >**注:**  このコマンドは、VM で有効になっている Windows ファイアウォールが原因でタイムアウトになります。

1. 代わりに、`nslookup TestVM2.contoso.com` コマンドを使用します。

    >**注:**  このコマンドは正常に実行され、プライベート ゾーンの名前解決を示します。 

## Copilot を使用して学習を拡張する

Copilot は、Azure スクリプト ツールの使用方法を学習するのに役立ちます。 Copilot は、ラボでは対象外の、またはさらに詳しい情報が必要な領域でも役立ちます。 Edge ブラウザーを開き、Copilot (右上) を選択するか、*copilot.microsoft.com* に移動します。 次のプロンプトを試すには数分かかります。
+ Azure DNS と Azure プライベート DNS の違いは何ですか? Azure プライベート DNS を使用する状況の例を示してください。
+ Azure DNS ゾーンを作成するときの自動登録の目的は何ですか?

## 自習トレーニングでさらに学習する

+ [Azure DNS の概要](https://learn.microsoft.com/training/modules/intro-to-azure-dns/)。 このモジュールでは、Azure DNS の機能、そのしくみ、および組織のニーズを満たすソリューションとして Azure DNS を使用することを選択すべきタイミングについて説明します。
+ [Azure DNS でドメインをホストする](https://learn.microsoft.com/training/modules/host-domain-azure-dns/)。 このモジュールでは、ドメインを IP アドレスにマップする DNS ゾーンと DNS レコードを作成します。 また、ドメイン名が Web サーバーに解決されることを確認するためにテストもします。

## 要点

以上でラボは完了です。 このラボの要点は次のとおりです。 

+ Azure DNS は、ドメイン ネーム システム (DNS) ドメイン (DNS ゾーンともいう) をホストおよび管理できるクラウド サービスです。 
+ Azure DNS パブリック ゾーンでは、インターネット上のホストによって解決されるようにするレコードのドメイン ネーム ゾーン データをホストします。
+ Azure プライベート DNS ゾーンを使用すると、プライベート Azure リソースのプライベート DNS ゾーン名前空間を構成できます。
+ DNS ゾーンは、DNS レコードのコレクションです。 DNS レコードは、ドメインに関する情報を提供します。
