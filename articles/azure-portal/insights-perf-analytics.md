<properties
	pageTitle="Azure Web アプリのパフォーマンスの監視 | Microsoft Azure"
	description="チャートの読み込みおよび応答時間、依存関係の情報やパフォーマンス警告を設定します。"
	services="azure-portal"
    documentationCenter="na"
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/01/2016"
	ms.author="awills"/>

# Azure Web アプリのパフォーマンスの監視

[Azure Portal](https://portal.azure.com) では、[Azure Web アプリ](../app-service-web/app-service-web-overview.md)または[仮想マシン](../virtual-machines/virtual-machines-linux-about.md)のアプリケーション パフォーマンス監視を設定することができます。パフォーマンス監視ソリューションは、アプリをインストルメント化して、アクティビティに関するテレメトリを送信します。得られたメトリックとテレメトリは、問題の診断、パフォーマンスの改善、使用状況の評価などに使用することができます。

## 実行時またはビルド時

監視は、アプリを次の 2 つの方法のどちらかでインストルメント化することによって構成できます。

* **実行時** - Web アプリが既に実行されているときにパフォーマンス監視拡張機能を選択できます。アプリを再構築または再インストールする必要はありません。応答時間、成功率、例外、依存関係などを監視するパッケージの標準セットを利用できます。

    **Application Insights** と **New Relic** は、利用できる実行時パフォーマンス監視拡張機能のうちの 2 つです。
 
* **ビルド時** - 開発時にアプリにパッケージをインストールすることができます。これは、汎用性が高い方法です。同じ標準パッケージに加えて、コードを記述してテレメトリをカスタマイズしたり、独自のテレメトリを送信したりすることができます。アプリのドメインのセマンティクスに従って、特定のアクティビティをログに記録したり、イベントを記録したりすることができます。

    **Application Insights** は、ビルド時パッケージを提供します。


## Application Insights パッケージを使用したアプリケーションのビルド

Application Insights では、アプリへの SDK のインストールによって、より詳細なテレメトリを提供できます。

Visual Studio (2013 Update 2 以降) で、Application Insights SDK をプロジェクトに追加します。

![Web プロジェクトを右クリックし、[Application Insights の追加] を選択する](./media/insights-perf-analytics/03-add.png)

サインインが要求されたら、Azure アカウントの資格情報を使用します。

この操作には、次の 2 つの効果があります。

1. Azure に Application Insights リソースが作成されます。このリソースでテレメトリが格納、分析、表示されます。
2. Application Insights NuGet パッケージがコードに追加され、テレメトリを Azure リソースに送信するように構成されます。

テレメトリをテストするには、開発用コンピューターでアプリを実行する (F5 キーを押す) か、単純にアプリを再発行します。

SDK には[カスタム テレメトリを記述して](../application-insights/app-insights-api-custom-events-metrics.md)利用状況を追跡する API があります。

### リソースを手動でセットアップする

Visual Studio で SDK を追加しなかった場合は、テレメトリが格納、分析、表示される Application Insights リソースを Azure でセットアップする必要があります。

![Click Add, Developer Services, Application Insights.Choose ASP.NET app type.](./media/insights-perf-analytics/01-new.png)


## 拡張機能を有効にする

1. インストルメント化する Web アプリまたは仮想マシンの制御ブレードを参照します。

2. Application Insights または New Relic の拡張機能を追加します。

    Web アプリをインストルメント化する場合:

![設定、拡張機能、追加、Application Insights](./media/insights-perf-analytics/05-extend.png)

仮想マシンを使用している場合は、次のように操作します。

![分析タイルをクリックする](./media/insights-perf-analytics/10-vm1.png)



## データを検索する

1. Application Insights リソースを開きます ([参照] から直接開くか、Web アプリのパフォーマンス管理ツールから開きます)。

2. 任意のグラフをクリックすると、より詳細な情報が表示されます。

    ![On the Application Insights overview blade, click a chart](./media/insights-perf-analytics/07-dependency.png)

    [メトリック ブレードをカスタマイズする](../application-insights/app-insights-metrics-explorer.md)ことができます。

3. 個々のイベントとそのプロパティを表示するには、さらにクリックします。

    ![Click an event type to open a search filtered on that type](./media/insights-perf-analytics/08-requests.png)

    "..." というリンクを使用すると、すべてのプロパティが開かれます。

    [検索をカスタマイズ](../application-insights/app-insights-diagnostic-search.md)できます。

テレメトリに対するより強力な検索については、[Analytics クエリ言語](../application-insights/app-insights-analytics-tour.md)を使用してください。


## Q & A

別の Application Insights リソースにデータを送信するように変更するには、どうすればよいですか。

* "*Visual Studio で Application Insights をコードに追加した場合:*" プロジェクトを右クリックし、**[Application Insights]、[構成]** の順に選択して、目的のリソースを選択します。新しいリソースを作成するオプションが表示されます。リビルドして再デプロイします。
* "*それ以外の場合:*" Azure で Web アプリの制御ブレードを開き、**[ツール]、[拡張機能]** の順に開きます。Application Insights 拡張機能を削除します。次に、**[ツール]、[パフォーマンス]** の順に開き、[ここをクリック] をクリックして、[Application Insights] を選択し、目的のリソースを選択します (新しい Application Insights リソースを作成する場合は、先に作成します)。


## 次のステップ

* [サービスの正常性のメトリックスを監視](insights-how-to-customize-monitoring.md)して、サービスの可用性と応答性を確認します。
* [監視と診断を有効](insights-how-to-use-diagnostics.md)にしてサービスに関する詳細な頻度の高いメトリックを収集します。
* 操作イベントが発生したり、メトリックがしきい値を超えたりするたびに、[アラート通知を受け取り](insights-receive-alert-notifications.md)ます。
* [JavaScript のアプリや Web ページに Application Insights](../application-insights/app-insights-web-track-usage.md) を使用して、Web ページを参照しているブラウザーからクライアント テレメトリを取得します。
* [可用性 Web テストを設定](../application-insights/app-insights-monitor-web-app-availability.md)して、サイトがダウンした場合にアラートを送信するようにします。

<!---HONumber=AcomDC_0907_2016-->