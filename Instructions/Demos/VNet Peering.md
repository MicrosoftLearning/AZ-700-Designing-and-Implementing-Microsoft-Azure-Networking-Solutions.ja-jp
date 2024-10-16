---
demo:
  title: VNET ピアリング (モジュール 01)
  module: Module 01 - Introduction to Azure Virtual Networks
---
## VNet ピアリングを構成する

**注:**  このデモには、2 つの仮想ネットワークが必要です。

**シミュレーション:** [Azure ピアリングを使用して 2 つの 仮想ネットワークを接続する](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering)

**参照**: [VNet ピアリングを使用して仮想ネットワークを接続する - チュートリアル](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**1 番目の仮想ネットワークの VNet ピアリングを構成する**

1.  **Azure portal** で、1 番目の仮想ネットワークを選択します。 ピアリングの値を確認します。 

1.  **[設定]** で、 **[ピアリング]** 、 **[+ 追加]** の順に選択し、新しいピアリングを追加します。

1. 2 番目の仮想ネットワークのピアリングを構成します。 情報アイコンを使用して、さまざまな設定を確認します。 

1. ピアリングが完了したら、 **[ピアリング状態]** を確認します。 

**2 番目の仮想ネットワークの VNET ピアリングを確認する**

1.  **Azure portal** で、2 番目の仮想ネットワークを選択します

1.  **[設定]** で、 **[ピアリング]** を選択します。

1. ピアリングが自動的に作成されていることがわかります。  **[ピアリング状態]**  が **[接続済み]** であることに注目してください。

1. ラボでは、受講者がピアリングを作成し、仮想マシン間の接続をテストします。 