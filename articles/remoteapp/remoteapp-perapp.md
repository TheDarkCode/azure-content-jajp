<properties
   pageTitle="Azure RemoteApp コレクション (プレビュー) で個々のユーザーにアプリケーションを発行する | Microsoft Azure"
   description="Azure RemoteApp で、グループに応じてではなく、個々のユーザーにアプリケーションを発行する方法について説明します。"
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# Azure RemoteApp コレクション (プレビュー) で個々のユーザーにアプリケーションを発行する

> [AZURE.IMPORTANT]
Azure RemoteApp の提供は終了しました。詳細については、[お知らせ](https://go.microsoft.com/fwlink/?linkid=821148)をご覧ください。

この記事では、Azure RemoteApp コレクションで個々のユーザーにアプリケーションを発行する方法について説明します。これは Azure RemoteApp の新機能で、現時点では "プライベート プレビュー" 段階のため、選ばれた早期採用者のみが評価の目的で使用できます。

当初は、Azure RemoteApp により、アプリケーションを "発行する" 唯一の方法が実現しました。この方法では、管理者がイメージからアプリケーションを発行すると、そのアプリケーションがコレクション内のすべてのユーザーに表示されるようになりました。

一般的なシナリオでは、管理コストを削減するために、多くのアプリケーションを 1 つのイメージに含めて 1 つのコレクションをデプロイします。多くの場合、必ずしもすべてのアプリケーションがすべてのユーザーに関連するとは限りません。そのため、管理者は、個々のユーザーにアプリケーションを発行し、ユーザーのアプリケーション フィードに不要なアプリケーションが表示されないようにすることができます。

これは現在、Azure RemoteApp で限定プレビュー機能として使用できます。この新機能の簡単な概要を次に示します。

1. コレクションは、次の 2 つのモードのいずれかに設定できます。
 
  - 本来の "コレクション モード"。このモードでは、コレクション内のすべてのユーザーに、発行済みアプリケーションがすべて表示されます。これは既定のモードです。
  - 新しい "アプリケーション モード"。このモードでは、自身に明示的に割り当てられているアプリケーションのみが表示されます。

2. 現時点では、アプリケーション モードを有効にできるのは、Azure RemoteApp PowerShell コマンドレットを使用した場合のみです。

  - アプリケーション モードに設定すると、コレクション内のユーザー割り当てを Azure ポータルを使用して管理することができません。ユーザー割り当ては、PowerShell コマンドレットを使用して管理する必要があります。

3. ユーザーには、自身に直接発行されたアプリケーションのみが表示されます。ただし、ユーザーは引き続き、イメージ上の使用可能なその他のアプリケーションを、オペレーティング システム内で直接アクセスすることで起動できる場合があります。
  - この機能では、アプリケーションのセキュリティで保護されたロックダウンが提供されません。アプリケーション フィードでの表示が制限されるだけです。
  - アプリケーションからユーザーを分離する必要がある場合は、そのために個別のコレクションを使用する必要があります。

## Azure RemoteApp PowerShell コマンドレットを取得する方法

新しいプレビュー機能を試すには、Azure PowerShell コマンドレットを使用する必要があります。現在、Azure 管理ポータルを使用して新しいアプリケーション発行モードを有効にすることはできません。

最初に、[Azure PowerShell モジュール](../powershell-install-configure.md)がインストールされていることを確認します。

次に、PowerShell コンソールを管理者モードで起動し、次のコマンドレットを実行します。

		Add-AzureAccount

このコマンドレットでは、Azure のユーザー名とパスワードの入力が求められます。サインインすると、Azure サブスクリプションに対して Azure RemoteApp コマンドレットを実行できるようになります。

## コレクションのモードを確認する方法

次のコマンドレットを実行します。

		Get-AzureRemoteAppCollection <collectionName>

![Check the collection mode](./media/remoteapp-perapp/araacllelvel.png)

AclLevel プロパティには、次の値が表示されます。

- Collection: 本来の発行モード。すべてのユーザーに、すべての発行済みアプリケーションが表示されます。
- Application: 新しい発行モード。ユーザーに表示されるのは、自身に直接発行されたアプリケーションのみです。

## アプリケーション発行モードに切り替える方法

次のコマンドレットを実行します。

		Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

アプリケーションの発行状態が保持されます。最初は、すべてのユーザーに元の発行済みアプリケーションがすべて表示されます。

## 特定のアプリケーションを表示できるユーザーの一覧を表示する方法

次のコマンドレットを実行します。

		Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

これにより、アプリケーションを表示できるすべてのユーザーの一覧が表示されます。

注: Get-AzureRemoteAppProgram -CollectionName <collectionName> を実行すると、アプリケーションのエイリアス (上記の構文では "アプリのエイリアス") が表示されます。

## アプリケーションをユーザーに割り当てる方法

次のコマンドレットを実行します。

		Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

これで、ユーザーの Azure RemoteApp クライアントにアプリケーションが表示され、ユーザーがそのアプリケーションに接続できるようになります。

## ユーザーからアプリケーションを削除する方法

次のコマンドレットを実行します。

		Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## フィードバックの提供
このプレビュー機能に関連したご意見やご提案をお待ちしています。[アンケート](http://www.instant.ly/s/FDdrb)に答えて感想をお聞かせください。

## プレビュー機能を試す機会がなかった場合
プレビューにまだ参加していない方は、こちらの[アンケート](http://www.instant.ly/s/AY83p)を使って利用を申し込んでください。

<!---HONumber=AcomDC_0817_2016-->