<properties
   pageTitle="Azure Resource Manager と Azure ポータルを使用してサイト間 VPN 接続を持つ仮想ネットワークを作成する | Microsoft Azure"
   description="Resource Manager デプロイメント モデルを使用して VNet を作成し、S2S VPN ゲートウェイ接続を使用してローカルのオンプレミス ネットワークにこれを接続する方法について説明します。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# Azure Portal を使用してサイト間接続を持つ VNet を作成する

> [AZURE.SELECTOR]
- [Azure ポータル](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Azure クラシック ポータル](vpn-gateway-site-to-site-create.md)
- [PowerShell - Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


この記事では、**Azure Resource Manager デプロイメント モデル**と Azure Portal を使用して、仮想ネットワークと、オンプレミス ネットワークに対するサイト間 VPN 接続を作成する手順について説明します。サイト間接続は、クロスプレミスおよびハイブリッド構成に使用できます。

![ダイアグラム](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)



### サイト間接続のデプロイ モデルとデプロイ ツール

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

VNet どうしは接続しても、オンプレミスへの接続は作成しない場合は、[VNet 間の接続の構成](vpn-gateway-vnet-vnet-rm-ps.md)に関するページを参照してください。

## 開始する前に

構成を開始する前に、以下が揃っていることを確認します。

- 互換性のある VPN デバイスおよびデバイスを構成できる人員。「[VPN デバイスについて](vpn-gateway-about-vpn-devices.md)」を参照してください。VPN デバイスの構成に詳しくない場合や、オンプレミス ネットワーク構成の IP アドレス範囲を把握していない場合は、詳細な情報を把握している担当者と協力して作業を行ってください。

- VPN デバイスの外部接続用パブリック IP アドレス。この IP アドレスを NAT の内側に割り当てることはできません。
	
- Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、[無料試用版](http://azure.microsoft.com/pricing/free-trial/)にサインアップしてください。

### <a name="values"></a>この演習のサンプル構成値


以下の手順を練習として使用する場合は、次のサンプル構成値を使用してください。

- VNet の名前: TestVNet1
- アドレス空間: 10.11.0.0/16 および 10.12.0.0/16
- サブネット:
	- FrontEnd: 10.11.0.0/24
	- BackEnd: 10.12.0.0/24
	- GatewaySubnet: 10.12.255.0/27
- リソース グループ: TestRG1
- 場所: 米国東部
- DNS サーバー: 8.8.8.8
- ゲートウェイ名: VNet1GW
- パブリック IP: VNet1GWIP
- VPN の種類: ルート ベース
- 接続の種類: サイト間 (IPsec)
- ゲートウェイの種類: VPN
- ローカル ネットワーク ゲートウェイ名: Site2
- 接続名: VNet1toSite2



## 1\.仮想ネットワークの作成 

既に VNet がある場合は、設定が VPN ゲートウェイの設計に適合していることを確認します。特に、他のネットワークと重複している可能性のあるサブネットに注意してください。サブネットの重複があると、接続が適切に動作しません。VNet が正しい設定で構成されていたら、「[DNS サーバーの指定](#dns)」セクションの手順に進んでください。

### 仮想ネットワークを作成するには

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## 手順 2.アドレス空間とサブネットの追加

VNet が作成されたら、アドレス空間とサブネットをさらに追加できます。

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="dns"></a>3.DNS サーバーの指定

練習としてこの構成を作成する場合は、DNS サーバーの指定時に、上記の[値](#values)を参照してください。

### DNS サーバーを指定するには

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## 4\.ゲートウェイ サブネットの作成

仮想ネットワークをゲートウェイに接続する前に、まず、接続先の仮想ネットワークのゲートウェイ サブネットを作成する必要があります。作成するゲートウェイ サブネットには、*GatewaySubnet* という名前を付ける必要があります。そうしないと、正しく動作しません。

一部の構成のゲートウェイ サブネット プレフィックスには、プールに必要な IP アドレスの数に合わせて /28 以上のサブネットが必要です。つまり、ゲートウェイ サブネット プレフィックスは、/28、/27、/26 などである必要があります。今後構成が追加される可能性に対応するために、ここでは大き目のサブネットを作成できます。

練習としてこの構成を作成する場合は、ゲートウェイ サブネットの作成時に、上記の[値](#values)を参照してください。

### ゲートウェイ サブネットを作成するには

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## 5\.仮想ネットワーク ゲートウェイの作成

練習としてこの構成を作成する場合は、ゲートウェイの作成時に、上記の[値](#values)を参照してください。

### 仮想ネットワーク ゲートウェイを作成するには

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## 6\.ローカル ネットワーク ゲートウェイの作成

*ローカル ネットワーク ゲートウェイ*は、オンプレミスの場所を指します。Azure から参照できるように、ローカル ネットワーク ゲートウェイに名前を付けます。

練習としてこの構成を作成する場合は、ローカル サイトの追加時に、上記の[値](#values)を参照してください。

### ローカル ネットワーク ゲートウェイを作成するには

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## 7\.VPN デバイスの構成

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## 8\.サイト間 VPN 接続の作成

仮想ネットワーク ゲートウェイと VPN デバイスの間にサイト間 VPN 接続を作成します。サンプルの値は必ず実際の値に変更してください。共有キーは、VPN デバイスの構成に使用したものと同じ値にする必要があります。

このセクションを開始する前に、仮想ネットワーク ゲートウェイとローカル ネットワーク ゲートウェイの作成が完了していることを確認してください。練習としてこの構成を作成する場合は、接続の作成時に、上記の[値](#values)を参照してください。

### VPN 接続を作成するには


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## 9\.VPN 接続の確認

VPN 接続の確認はポータルで行えるほか、PowerShell を使って確認することもできます。

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## 次のステップ

- 接続が完成したら、仮想ネットワークに仮想マシンを追加することができます。詳細については、仮想マシンの[ラーニング パス](https://azure.microsoft.com/documentation/learning-paths/virtual-machines)を参照してください。

- BGP の詳細については、[BGP の概要](vpn-gateway-bgp-overview.md)に関する記事と [BGP の構成方法](vpn-gateway-bgp-resource-manager-ps.md)に関する記事を参照してください。

<!---HONumber=AcomDC_0831_2016-->