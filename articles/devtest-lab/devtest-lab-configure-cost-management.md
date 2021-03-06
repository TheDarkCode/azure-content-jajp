<properties
	pageTitle="Azure DevTest Labs で、ラボの月間推定コスト傾向を表示する | Microsoft Azure"
	description="Azure DevTest Labs の月間推定コスト傾向グラフについて説明します。"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="tarcher"/>

# Azure DevTest Labs で、ラボの月間推定コスト傾向を表示する

DevTest ラボのコスト管理機能は、ラボのコストを追跡するのに役立ちます。この記事では、**月間推定コスト傾向**グラフを使用して、現在のカレンダー月の現時点までの推定コストおよび現在のカレンダー月の月末の予測コストを表示する方法を示します。この記事では、Azure Portal で月間推定コスト傾向グラフを表示する方法について説明します。

## 月間推定コスト傾向グラフを表示する

月間推定コスト傾向グラフを表示するには、次の手順に従います:

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。

1. **[その他のサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。

1. ラボの一覧で目的のラボを選択します。

1. ラボのブレードで、**[コスト設定]** を選択します。

1. ラボの **[コスト設定]** ブレードで、**[ラボのコスト傾向]** を選択します。

1. 次のスクリーン ショットはコスト グラフの例を示しています。

    ![コスト グラフ](./media/devtest-lab-configure-cost-management/graph.png)

**[推定コスト]** 値は現在のカレンダー月のこれまでの推定コストです。**[予測コスト]** は現在のカレンダー月全体の推定コストで、過去 5 日間のラボのコストを使用して計算されます。
 
コストの金額の端数は切り上げられます。次に例を示します。

- 5\.01 は切り上げて 6 になります。
- 5\.50 は切り上げて 6 になります。
- 5\.99 は切り上げて 6 になります。

グラフの上に示されているとおり、グラフに表示されるコストは、[従量課金](https://azure.microsoft.com/offers/ms-azr-0003p/)プラン料金を使用した*推定*コストです。また、コスト計算には次のものは含まれて*いません*。

- CSP サブスクリプションと Dreamspark サブスクリプションは現在サポートされません。Azure DevTest Labs は、[Azure Billing API](../billing-usage-rate-card-overview.md) を使用してラボ コストを計算しますが、CSP サブスクリプションも Dreamspark サブスクリプションもサポートしていないためです。
- プラン料金。現時点では、Microsoft または Microsoft パートナーとネゴシエートしたプラン料金 (サブスクリプションの下に表示される) は使用できません。従量課金料金を使用します。
- 税金
- 割引
- 請求通貨。現時点では、ラボのコストは USD 通貨でのみ表示されます。

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## 関連するブログ記事

- [Two more things to keep your cost on track in DevTest Labs (DevTest Labs でコストを正常に保つためのさらに 2 つのこと)](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
- [Why Cost Thresholds? (なぜコストのしきい値か?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## 次のステップ

次に試す操作をいくつか以下に示します。

- [ラボ ポリシーを定義する](./devtest-lab-set-lab-policy.md) - ラボとその VM の使用方法を制御するために使用されるさまざまなポリシーを設定する方法を確認します。
- [カスタム イメージを作成する](./devtest-lab-create-template.md) - VM を作成する場合は、ベースとしてカスタム イメージまたは Marketplace イメージを指定します。この記事では、VHD ファイルからカスタム イメージを作成する方法について説明します。
- [Marketplace イメージを構成する](./devtest-lab-configure-marketplace-images.md) - DevTest Labs では、Azure Marketplace イメージを基にした VM を作成することができます。この記事では、ラボで VM を作成する際に使用できるようにする Azure Marketplace イメージ (該当するものがある場合) を指定する方法について説明します。
- [ラボで VM を作成する](./devtest-lab-add-vm-with-artifacts.md) - 基本イメージ (カスタムまたは Marketplace) から VM を作成する方法と、VM でアーティファクトを操作する方法について説明します。

<!---HONumber=AcomDC_0907_2016-->