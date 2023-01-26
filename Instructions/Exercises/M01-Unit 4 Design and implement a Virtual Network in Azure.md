---
Exercise:
  title: M01 - ユニット 4 Azure で仮想ネットワークを設計および実装する
  module: Module 01 - Introduction to Azure Virtual Networks
---
# <a name="m01-unit-4-design-and-implement-a-virtual-network-in-azure"></a>M01-ユニット 4 Azure で仮想ネットワークを設計および実装する


## <a name="exercise-scenario"></a>演習のシナリオ 

現在、あなたは Azure portal で仮想ネットワークをデプロイする準備ができています。

ここで考える架空の組織 Contoso Ltd は、インフラストラクチャとアプリケーションを Azure に移行している途中です。 ネットワーク エンジニアとしての役割を持つあなたは、それらの仮想ネットワーク内のリソースをサポートするため、3 つの仮想ネットワークとサブネットを計画して実装する必要があります。

#### <a name="estimated-time-20-minutes"></a>推定時間:20 分

**CoreServicesVnet** 仮想ネットワークは、**米国東部**リージョンにデプロイされています。 この仮想ネットワークには、最も多くのリソースを配置します。 VPN 接続を介してオンプレミス ネットワークに接続されます。 このネットワークには、ビジネスの運営にとって重要な Web サービス、データベース、その他のシステムを配置します。 ドメイン コントローラーや DNS などの共有サービスもここに配置されます。 大きく成長することが見込まれるため、この仮想ネットワークには大きなアドレス空間が必要です。

**ManufacturingVnet** 仮想ネットワークは、組織の製造施設がある場所に近い**西ヨーロッパ** リージョンにデプロイされています。 この仮想ネットワークには、製造施設の運用に使用するシステムが含まれます。 組織では、システムでの温度などのデータの取得元として、多数の内部接続デバイスを予想しており、拡張できる IP アドレス空間が必要になります。

**ResearchVnet** 仮想ネットワークは、組織の研究開発チームがある場所に近い**東南アジア**リージョンにデプロイされています。 研究開発チームは、この仮想ネットワークを使用します。 チームのリソースのセットは小規模で安定しており、拡大することはないと予想されます。 チームには、作業のための少数の仮想マシン用に少数の IP アドレスが必要です。

![Contoso のネットワーク レイアウト。 オンプレミスの 10.10.0.0/16 ResearchVNet 東南アジア 10.40.40.0/24 CoreServicesVNet 米国東部 10.20.0.0/16 ManufacturingVNet 西ヨーロッパ 10.30.0.0/16
](../media/design-implement-vnet-peering.png)


これから次のようなリソースを作成します。
 

| **Virtual Network** | **[リージョン]**   | **仮想ネットワークのアドレス空間** | **サブネット**                | **サブネット**    |
| ------------------- | ------------ | --------------------------------- | ------------------------- | ------------- |
| CoreServicesVnet    | 米国東部      | 10.20.0.0/16                      |                           |               |
|                     |              |                                   | GatewaySubnet             | 10.20.0.0/27  |
|                     |              |                                   | SharedServicesSubnet      | 10.20.10.0/24 |
|                     |              |                                   | DatabaseSubnet            | 10.20.20.0/24 |
|                     |              |                                   | PublicWebServiceSubnet    | 10.20.30.0/24 |
| ManufacturingVnet   | 西ヨーロッパ  | 10.30.0.0/16                      |                           |               |
|                     |              |                                   | ManufacturingSystemSubnet | 10.30.10.0/24 |
|                     |              |                                   | SensorSubnet1             | 10.30.20.0/24 |
|                     |              |                                   | SensorSubnet2             | 10.30.21.0/24 |
|                     |              |                                   | SensorSubnet3             | 10.30.22.0/24 |
| ResearchVnet        |東南アジア| 10.40.0.0/16                      |                           |               |
|                     |              |                                   | ResearchSystemSubnet      | 10.40.0.0/24  |


これらの仮想ネットワークとサブネットは、既存のリソースに対応しながら、予想される成長にも対応できるような構造になっています。 これらの仮想ネットワークとサブネットを作成して、ネットワーク インフラストラクチャの基盤を構築してみましょう。

この演習では、以下のことを行います。

+ タスク 1: Contoso のリソース グループを作成する
+ タスク 2: CoreServicesVnet の仮想ネットワークとサブネットを作成する
+ タスク 3: ManufacturingVnet の仮想ネットワークとサブネットを作成する
+ タスク 4: ResearchVnet の仮想ネットワークとサブネットを作成する
+ タスク 5: VNet とサブネットの作成を確認する

## <a name="task-1-create-the-contoso-resource-group"></a>タスク 1: Contoso のリソース グループを作成する

1. [Azure ポータル](https://portal.azure.com/)に移動します。

2. ホーム ページの **[Azure サービス]** で、**[リソース グループ]** を選択します。  

3. [リソース グループ] で、**[+ 作成]** を選択します。

4. 次の表の情報を使用して、リソース グループを作成します。

| **Tab**         | **オプション**                                 | **Value**            |
| --------------- | ------------------------------------------ | -------------------- |
| 基本          | Resource group                             | ContosoResourceGroup |
|                 | リージョン                                     | (米国) 米国東部         |
| タグ            | 変更は必要ありません                        |                      |
| [確認および作成] | 設定を確認し、**[作成]** を選択します |                      |


5. [リソース グループ] で、一覧に **ContosoResourceGroup** が表示されていることを確認します。

 

## <a name="task-2-create-the-coreservicesvnet-virtual-network-and-subnets"></a>タスク 2: CoreServicesVnet の仮想ネットワークとサブネットを作成する

1. Azure portal のホーム ページで、グローバル検索バーに移動し、「**仮想ネットワーク**」を検索して、[サービス] の下の仮想ネットワークを選択します。  ![仮想ネットワークに対する Azure portal のホーム ページのグローバル検索バーの結果。](../media/global-search-bar.PNG)
2. 仮想ネットワーク ページで、**[作成]** を選択します。  ![仮想ネットワークの作成ウィザード。](../media/create-virtual-network.png)
3. 次の表の情報を使用して、CoreServicesVnet 仮想ネットワークを作成します。  
   ‎既定の IP アドレス空間を削除または上書きします![Azure 仮想ネットワークのデプロイのための IP アドレスの構成 ](../media/default-vnet-ip-address-range-annotated.png)

 

| **Tab**      | **オプション**         | **Value**            |
| ------------ | ------------------ | -------------------- |
| 基本       | リソース グループ     | ContosoResourceGroup |
|              | Name               | CoreServicesVnet     |
|              | リージョン             | (米国) 米国東部         |
| IP アドレス | IPv4 アドレス空間 | 10.20.0.0/16         |

 4. 次の表の情報を使用して、CoreServicesVnet のサブネットを作成します。

 5. 各サブネットの作成を始めるには、**[+ サブネットの追加]** を選択します。 各サブネットの作成を終えるには、**[追加]** を選択します。

| **サブネット**             | **オプション**           | **Value**              |
| ---------------------- | -------------------- | ---------------------- |
| GatewaySubnet          | サブネット名          | GatewaySubnet          |
|                        | サブネットのアドレス範囲 | 10.20.0.0/27           |
| SharedServicesSubnet   | サブネット名          | SharedServicesSubnet   |
|                        | サブネットのアドレス範囲 | 10.20.10.0/24          |
| DatabaseSubnet         | サブネット名          | DatabaseSubnet         |
|                        | サブネットのアドレス範囲 | 10.20.20.0/24          |
| PublicWebServiceSubnet | サブネット名          | PublicWebServiceSubnet |
|                        | サブネットのアドレス範囲 | 10.20.30.0/24          |

 6. CoreServicesVnet とその関連サブネットの作成を終えるには、**[確認および作成]** を選択します。

 7. 構成が検証に合格したことを確認して、**[作成]** を選択します。
 
 8. 次の表に基づいて、VNet ごとにステップ 1 から 8 を繰り返します  

## <a name="task-3-create-the-manufacturingvnet-virtual-network-and-subnets"></a>タスク 3: ManufacturingVnet の仮想ネットワークとサブネットを作成する


| **Tab**      | **オプション**         | **Value**             |
| ------------ | ------------------ | --------------------- |
| 基本       | リソース グループ     | ContosoResourceGroup  |
|              | Name               | ManufacturingVnet     |
|              | リージョン             | (ヨーロッパ) 西ヨーロッパ  |
| IP アドレス | IPv4 アドレス空間 | 10.30.0.0/16          |



| **サブネット**                | **オプション**           | **Value**                 |
| ------------------------- | -------------------- | ------------------------- |
| ManufacturingSystemSubnet | サブネット名          | ManufacturingSystemSubnet |
|                           | サブネットのアドレス範囲 | 10.30.10.0/24             |
| SensorSubnet1             | サブネット名          | SensorSubnet1             |
|                           | サブネットのアドレス範囲 | 10.30.20.0/24             |
| SensorSubnet2             | サブネット名          | SensorSubnet2             |
|                           | サブネットのアドレス範囲 | 10.30.21.0/24             |
| SensorSubnet3             | サブネット名          | SensorSubnet3             |
|                           | サブネットのアドレス範囲 | 10.30.22.0/24             |
 

## <a name="task-4-create-the-researchvnet-virtual-network-and-subnets"></a>タスク 4: ResearchVnet の仮想ネットワークとサブネットを作成する


| **Tab**      | **オプション**         | **Value**            |
| ------------ | ------------------ | -------------------- |
| 基本       | リソース グループ     | ContosoResourceGroup |
|              | Name               | ResearchVnet         |
|              | リージョン             | 東南アジア       |
| IP アドレス | IPv4 アドレス空間 | 10.40.0.0/16         |

| **サブネット**           | **オプション**           | **Value**            |
| -------------------- | -------------------- | -------------------- |
| ResearchSystemSubnet | サブネット名          | ResearchSystemSubnet |
|                      | サブネットのアドレス範囲 | 10.40.0.0/24         |
 

## <a name="task-5-verify-the-creation-of-vnets-and-subnets"></a>タスク 5: VNet とサブネットの作成を確認する

1. Azure portal のホーム ページで、**[すべてのリソース]** を選択します。

2. CoreServicesVnet、ManufacturingVnet、ResearchVnet が一覧に表示されていることを確認します。

3. 使用するリージョンごとに、Azure によって Network Watcher が作成されることに注意してください。

4. **CoreServicesVnet** を選択します。 

5. CoreServicesVnet の **[設定]** で、**[サブネット]** を選択します。

6. CoreServicesVnet の [サブネット] で、作成したサブネットが一覧にあり、IP アドレス範囲が正しいことを確認します。

   ![CoreServicesVnet のサブネットの一覧。](../media/verify-subnets-annotated.png)

7. VNet ごとに手順 4 〜 6 を繰り返します。

お疲れさまでした。 リソース グループ、3 つの VNet、関連するサブネットが正常に作成されました。 
