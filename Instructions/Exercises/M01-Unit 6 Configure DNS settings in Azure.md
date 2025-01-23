---
Exercise:
  title: M01 - ユニット 6 Azure で DNS 設定を構成する
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 - ユニット 6 Azure で DNS 設定を構成する

## 演習のシナリオ

このユニットでは、Contoso Ltd の DNS 名前解決を構成します。contoso.com という名前のプライベート DNS ゾーンを作成し、登録と解決のために VNet をリンクした後、2 つの仮想マシンを作成して構成をテストします。

![DNS アーキテクチャの図。](../media/6-exercise-configure-domain-name-servers-configuration-azure.png)

この演習では、次のことを行います。

+ タスク 1: プライベート DNS ゾーンを作成する
+ タスク 2: 自動登録のためにサブネットをリンクする
+ タスク 3: 構成をテストするための仮想マシンを作成する
+ タスク 4: DNS ゾーンにレコードが存在することを確認する

   >**メモ:** このラボをご自分のペースでクリックして進めることができる、 **[ラボの対話型シミュレーション](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20DNS%20settings%20in%20Azure)** が用意されています。 対話型シミュレーションとホストされたラボの間に若干の違いがある場合がありますが、示されている主要な概念とアイデアは同じです。

### 推定時間: 25 分

## タスク 1: プライベート DNS ゾーンを作成する

1. [Azure Portal](https://portal.azure.com/) に移動します。

1. Azure ホーム ページの検索バーに「dns」と入力し、 **[プライベート DNS ゾーン]** を選択します。  
   ![DNS が検索されている Azure portal ホーム ページ](../media/create-private-dns-zone.png)

1. [プライベート DNS ゾーン] で **[+ 作成]** を選択します。

1. 次の表の情報を使用して、プライベート DNS ゾーンを作成します。

    | **Tab**         | **オプション**                             | **Value**            |
    | --------------- | -------------------------------------- | -------------------- |
    | 基本          | Resource group                         | ContosoResourceGroup |
    |                 | Name                                   | Contoso.com          |
    | タグ            | 変更は必要ありません                    |                      |
    | [確認および作成] | 設定を確認し、[作成] を選択します |                      |

1. デプロイが完了するまで待ち、**[リソースに移動]** を選択します。

1. ゾーンが作成されたことを確認します。

## タスク 2: 自動登録のためにサブネットをリンクする

1. Contoso.com の **[DNS 管理]** で、**[仮想ネットワーク リンク]** を選択します。

1. [Contoso.com \| 仮想ネットワーク リンク] で、**[+ 追加]** を選択します。

    ![[+ 追加] が強調されている [contoso.com \| 仮想リンク]。](../media/add-network-link-dns.png)

1. 次の表の情報を使用して、仮想ネットワーク リンクを追加します。

    | **オプション**                          | **Value**                               |
    | ----------------------------------- | --------------------------------------- |
    | リンク名                           | CoreServicesVnetLink                    |
    | サブスクリプション                        | 変更は必要ありません                     |
    | Virtual Network                     | CoreServicesVnet (ContosoResourceGroup) |
    | 自動登録を有効にする            | オン                                |
    | 設定を確認し、[OK] を選択します。 |                                         |

1. **[最新の情報に更新]** を選択します。

1. CoreServicesVnetLink が作成され、自動登録が有効になっていることを確認します。

1. 次の表の情報を使用して、ManufacturingVnet にステップ 2 から 5 を繰り返します。

    | **オプション**                          | **Value**                                |
    | ----------------------------------- | ---------------------------------------- |
    | リンク名                           | ManufacturingVnetLink                    |
    | サブスクリプション                        | 変更は必要ありません                      |
    | Virtual Network                     | ManufacturingVnet (ContosoResourceGroup) |
    | 自動登録を有効にする            | オン                                 |
    | 設定を確認し、[OK] を選択します。 |                                          |

1. **[最新の情報に更新]** を選択します。

1. ManufacturingVnetLink が作成され、自動登録が有効になっていることを確認します。

1. 次の表の情報を使用して、ResearchVnet にステップ 2 から 5 を繰り返します。

    | **オプション**                          | **Value**                           |
    | ----------------------------------- | ----------------------------------- |
    | リンク名                           | ResearchVnetLink                    |
    | サブスクリプション                        | 変更は必要ありません                 |
    | Virtual Network                     | ResearchVnet (ContosoResourceGroup) |
    | 自動登録を有効にする            | オン                            |
    | 設定を確認し、[OK] を選択します。 |                                     |

1. **[最新の情報に更新]** を選択します。

1. ResearchVnetLink が作成され、自動登録が有効になっていることを確認します。

## タスク 3: 構成をテストするための仮想マシンを作成する

このセクションでは、2 つのテスト VM を作成して、プライベート DNS ゾーンの構成をテストします。

1. Azure portal で、右上の [Cloud Shell] アイコンを選択します。 必要に応じて、シェルを構成します。  
    + **[PowerShell]** を選択します。
    + **[ストレージ アカウントは必要ありません]** と **[サブスクリプション]** を選択してから、**[適用]** を選択します。
    + ターミナルが作成され、プロンプトが表示されるまで待ちます。 

1. Cloud Shell 画面のツール バーで、**[ファイルの管理]** アイコンを選択し、ドロップダウン メニューで **[アップロード]** を選択して、**azuredeploy.json** ファイルと **azuredeploy.parameters.json** ファイルを、ソース フォルダー **F:\Allfiles\Exercises\M01** から Cloud Shell のホーム ディレクトリに 1 つずつアップロードします。

1. 次の ARM テンプレートをデプロイして、この演習に必要な VM を作成します。

   >**注**: 管理者パスワードを入力するように求められます。

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

### RDP を使用してテスト VM に接続する

1. Azure portal のホーム ページで、「**仮想マシン**」を選択します。

1. **TestVM1** を選択します。

1. TestVM1 で、 **[接続] &gt; [RDP]** を選択し、RDP ファイルをダウンロードします。

    ![[接続] と [RDP] が強調されている TestVM1。](../media/connect-to-am.png)

1. RDP ファイルをデスクトップに保存します。

1. **TestVM2** で同じ手順に従います

1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使用して、TestVM1 に接続します。

1. RDP ファイル、およびデプロイ時に指定したユーザー名 **TestUser** とパスワードを使用して、TestVM2 に接続します。

1. 両方の VM で、**[デバイスのプライバシー設定の選択]** の **[同意する]** を選択します。

1. 両方の VM 上で、確認を求められたら **[ネットワーク]** で **[はい]** を選びます。

1. TestVM1 でコマンド プロンプトを開き、コマンド `ipconfig /all` を入力します。

1. IP アドレスが、DNS ゾーンで記録したものと同じであることを確認します。

1. コマンド「ping TestVM2.contoso.com」を入力します。

1. FQDN がプライベート DNS ゾーンでメモした IP アドレスに解決されていることを確認します。 VM で Windows ファイアウォールが有効になっているため、ping 自体はタイムアウトします。

1. または、nslookup TestVM2.contoso.com コマンドを入力して、VM2 の名前解決の成功レコードを受信したことを確認します

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
