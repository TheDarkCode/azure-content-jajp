<properties
 pageTitle="Azure の HPC Pack クラスターにジョブを送信する | Microsoft Azure"
 description="Azure の HPC Pack クラスターに HPC ジョブを送信するようにオンプレミス コンピューターを設定する方法について説明します。"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/15/2016"
 ms.author="danlep"/>

# オンプレミス コンピューターから Azure にデプロイされた HPC Pack クラスターに HPC ジョブを送信する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure の HPC Pack クラスターと HTTPS で通信する HPC Pack ジョブ送信ツールを実行するように、Windows を実行するオンプレミス クライアント コンピューターを構成します。さまざまなクラスター ユーザーが簡単かつ柔軟な方法でクラウドベースの HPC Pack クラスターにジョブを送信できます。ジョブ送信ツールを実行するために、ヘッド ノード VM に直接接続したり、Azure サブスクリプションにアクセスしたりする必要はありません。

![Azure のクラスターにジョブを送信する][jobsubmit]

## 前提条件

* **Azure VM にデプロイされた HPC Pack ヘッド ノード** - [Azure クイックスタート テンプレート](https://azure.microsoft.com/documentation/templates/)や [Azure PowerShell スクリプト](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)などの自動ツールを使用して、ヘッド ノードとクラスターをデプロイすることをお勧めします。この記事の手順を完了するには、ヘッド ノードの DNS 名とクラスター管理者の資格情報が必要です。

* **クライアント コンピューター** - HPC Pack クライアント ユーティリティを実行する Windows または Windows Server クライアント コンピューターが必要になります (「[システム要件](https://technet.microsoft.com/library/dn535781.aspx)」参照)。ジョブを送信する目的だけで HPC Pack Web ポータルまたは REST API を使用する場合、自分で選んだクライアント コンピューターを利用できます。

* **HPC Pack インストール メディア** - HPC Pack クライアント ユーティリティをインストールするには、HPC Pack (HPC Pack 2012 R2) の最新バージョンの無料インストール パッケージを [Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=328024)から入手できます。必ず、ヘッド ノード VM にインストールされている HPC Pack と同じバージョンをダウンロードしてください。

## 手順 1: ヘッド ノードに Web コンポーネントをインストールし、構成する

REST インターフェイスを有効にして HTTPS でクラスターにジョブを送信するには、HPC Pack ヘッド ノードに HPC Pack Web コンポーネントをインストールし、構成します (まだ構成されていない場合)。HpcWebComponents.msi インストール ファイルを実行し、最初に Web コンポーネントをインストールします。次に、HPC PowerShell スクリプト **Set-HPCWebComponents.ps1** を実行し、コンポーネントを構成します。

詳細な手順については、「[Microsoft HPC Pack Web コンポーネントのインストール](http://technet.microsoft.com/library/hh314627.aspx)」を参照してください。

>[AZURE.TIP] 一部の HPC Pack 用 Azure クイックスタート テンプレートでは、Web コンポーネントが自動的にインストールされ、構成されます。[HPC Pack IaaS デプロイメント スクリプト](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)を利用してクラスターを作成する場合、デプロイメントの一環として任意で Web コンポーネントをインストールし、構成します。

**Web コンポーネントをインストールするには**

1. クラスター管理者の資格情報を使用し、ヘッド ノード VM に接続します。

2. HPC Pack セットアップ フォルダーから、ヘッド ノードで HpcWebComponents.msi を実行します。

3. ウィザードの手順に従い、Web コンポーネントをインストールします

**Web コンポーネントを構成するには**

1. ヘッド ノードで、管理者として HPC PowerShell を起動します。

2. 構成スクリプトの場所にディレクトリを変更するには、次のコマンドを入力します。

    ```
    cd $env:CCP_HOME\bin
    ```
3. REST インターフェイスを構成し、HPC Web Service を起動し、次のコマンドを入力します。

    ```
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. 証明書の選択を求められたら、ヘッド ノードのパブリック DNS 名に対応する証明書を選択します。たとえば、クラシック デプロイメント モデルを使用してヘッド ノード VM をデプロイする場合、証明書名は CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net 形式になります。Resource Manager デプロイメント モデルを使用する場合、証明書名は、CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com 形式になります。

    >[AZURE.NOTE] オンプレミス コンピューターからヘッド ノードに後でジョブを送信するには、この証明書を選択する必要があります。Active Directory ドメインのヘッド ノードのコンピューター名に一致する証明書を選択したり、構成したりしないでください (CN=*MyHPCHeadNode.HpcAzure.local* など)。

5. Web ポータルのジョブ送信を構成するには、次のコマンドを入力します。

    ```
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. スクリプトが完了したら、次を入力し、HPC Job Scheduler Service を停止し、再起動します。

    ```
    net stop hpcscheduler
    net start hpcscheduler
    ```

## 手順 2: HPC Pack クライアント ユーティリティをオンプレミス コンピューターにインストールする

HPC Pack クライアント ユーティリティをインストールする場合は、[Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=328024)からクライアント コンピューターに HPC Pack セットアップ ファイル (完全インストール) をダウンロードします。インストールを開始するとき、HPC Pack クライアント ユーティリティのセットアップ オプションを選択します。

HPC Pack クライアント ツールを使用し、ジョブをヘッド ノード VM に送信するには、ヘッド ノードから証明書をエクスポートし、それをクライアント コンピューターにインストールする必要もあります。証明書は .CER 形式にする必要があります。

**ヘッド ノードから証明書をエクスポートするには**

1. ヘッド ノードで、ローカル コンピューター アカウントに対して Microsoft 管理コンソールに証明書スナップインを追加します。スナップインを追加する手順については、「[証明書スナップインを MMC に追加する](https://technet.microsoft.com/library/cc754431.aspx)」を参照してください。

2. コンソール ツリーで、**[証明書 - ローカル コンピューター]**、**[個人]** の順に展開し、**[証明書]** をクリックします。

3. 「[手順 1: ヘッド ノードに Web コンポーネントをインストールし、構成する](#step-1:-install-and-configure-the-web-components-on-the-head-node)」で HPC Pack Web コンポーネントに構成した証明書を見つけます (例: CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net)。

4. 証明書を右クリックし、**[すべてのタスク]** をクリックしてから **[エクスポート]** をクリックします。

5. 証明書のエクスポート ウィザードで、**[次へ]** をクリックしてから **[いいえ、秘密キーをエクスポートしません]** をクリックします。

6. ウィザードの残りの手順に従い、DER エンコード バイナリ X.509 (.CER) 形式で証明書をエクスポートします。


**クライアント コンピューターに証明書をインポートするには**


1. ヘッド ノードからクライアント コンピューターのフォルダーにエクスポートした証明書をコピーします。

2. クライアント コンピューターで、certmgr.msc を実行します。

3. 証明書マネージャーで、**[証明書 - 現在のユーザー]**、**[信頼されたルート証明機関]** の順に展開し、**[証明書]** を右クリックし、**[すべてのタスク]** をクリックし、**[インポート]** をクリックします。

4. 証明書インポート ウィザードで、**[次へ]** をクリックし、手順に従って、ヘッド ノードからエクスポートした証明書を信頼されたルート証明機関ストアにインポートします。



>[AZURE.TIP] クライアント コンピューターがヘッド ノードの証明機関を認識しないため、セキュリティ警告が表示されることがあります。テスト目的であるため、この警告を無視し、証明書インポートを完了します。

## 手順 3: クラスターでテスト ジョブを実行する

構成を検証する目的で、オンプレミス コンピューターから Azure のクラスターでジョブを実行してみます。たとえば、HPC Pack GUI ツールまたはコマンドライン コマンドを利用し、ジョブをクラスターに送信できます。Web ベース ポータルを利用してジョブを送信することもできます。


**クライアント コンピューターでジョブ送信コマンドを実行するには**


1. HPC Pack クライアント ユーティリティがインストールされているクライアント コンピューターで、コマンド プロンプトを起動します。

2. サンプル コマンドを入力します。たとえば、クラスターのすべてのジョブのリストを表示するには、ヘッド ノードの完全 DNS 名に応じて、次のいずれかと同様のコマンドを入力します。

    ```
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all

    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] スケジューラ URL には、IP アドレスではなく、ヘッド ノードの完全 DNS 名を使用します。IP アドレスを指定した場合、「The server certificate needs to either have a valid chain of trust or to be placed in the trusted root store (サーバー証明書は有効な信頼チェーンを持つか、信頼されたルート ストアに置かれている必要があります)」のようなエラーが表示されます。

3. 入力を求められたら、HPC クラスター管理者または構成済みの別のクラスター ユーザーのユーザー名 (&lt;DomainName&gt;\\&lt;UserName&gt; 形式) とパスワードを入力します。ジョブ操作が多ければ、資格情報をローカルに保存できます。

    ジョブの一覧が表示されます。


**クライアント コンピューターで HPC ジョブ マネージャーを使用するには**

1. 以前、ジョブの送信時、クライアント コンピューターでクラスター ユーザーのドメイン資格情報を保存しなかった場合、資格情報マネージャーで資格情報を追加できます。

    a.クライアント コンピューターのコントロール パネルで、資格情報マネージャーを起動します。

    b.**[Windows 資格情報]** をクリックし、**[汎用資格情報の追加]** をクリックします。

    c.インターネット アドレス(例: https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler または https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler) を指定し、HPC クラスター管理者または構成済みの別のクラスター ユーザーのユーザー名 (&lt;DomainName&gt;\\&lt;UserName&gt; 形式) とパスワードを入力します。

2. クライアント コンピューターで、HPC ジョブ マネージャーを起動します。

3. **[ヘッド ノードの選択]** ダイアログ ボックスに、Azure のヘッド ノードの URL (例: https://&lt;HeadNodeDnsName&gt;.cloudapp.net または https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com) を入力します。

    HPC ジョブ マネージャーが開き、ヘッド ノードでジョブの一覧が表示されます。

**ヘッド ノードで実行されている Web ポータルを使用するには**

1. クライアント コンピューターで Web ブラウザーを起動し、ヘッド ノードの完全 DNS 名に応じて、次のいずれかを入力します。

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal

    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. セキュリティ ダイアログ ボックスが表示されたら、HPC クラスター管理者のドメイン資格情報を入力します。(さまざまなロールで他のクラスター ユーザーを追加することもできます。「[クラスター ユーザーの管理](https://technet.microsoft.com/library/ff919335.aspx)」をご覧ください。)

    Web ポータルが開き、ジョブの一覧が表示されます。

3. クラスターから「Hello World」という文字列を返すサンプル ジョブを送信するには、左のナビゲーションで **[新しいジョブ]** をクリックします。

4. **[新しいジョブ]** ページの **[送信ページから]** で、**[HelloWorld]** をクリックします。ジョブの送信ページが表示されます。

5. **[Submit]** をクリックします。入力が求められたら、HPC クラスター管理者のドメインの資格情報を入力します。ジョブが送信され、ジョブ ID が **[自分のジョブ]** ページに表示されます。

6. 送信したジョブの結果を表示するには、ジョブ ID をクリックし、**[タスクの表示]** をクリックしてコマンドの出力を表示します (**[出力]** の下で)。

## 次のステップ

* [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx) を使用し、Azure クラスターにジョブを送信することもできます。

* Linux クライアントからクラスター ジョブを送信する場合は、[HPC Pack 2012 R2 SDK およびサンプル コード](https://www.microsoft.com/download/details.aspx?id=41633)の Python サンプルをご覧ください。


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png

<!---HONumber=AcomDC_0720_2016-->