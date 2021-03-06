<properties
	pageTitle="Sails.js Web アプリを Azure App Service にデプロイする"
	description="Node.js アプリケーションを Azure App Service にデプロイする方法を学習します。このチュートリアルは、Sails.js Web アプリをデプロイする方法を説明します。"
	services="app-service\web"
	documentationCenter="nodejs"
	authors="cephalin"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="nodejs"
	ms.topic="article"
	ms.date="07/19/2016"
	ms.author="cephalin"/>

# Sails.js Web アプリを Azure App Service にデプロイする

このチュートリアルでは、Sails.js アプリを Azure App Service にデプロイする方法について説明します。その過程で、App Service で実行する Node.js アプリを構成する方法の一般的な知識を得ることができます。

## 前提条件

- [Node.JS](https://nodejs.org/)。
- [Sails.js](http://sailsjs.org/get-started)。
- Sails.js の実用的な知識。このチュートリアルは、一般的な Sail.js の実行に関する問題解決を支援するものではありません。
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)。
- Microsoft Azure アカウント。アカウントを持っていない場合は、[無料試用版にサインアップする](/pricing/free-trial/?WT.mc_id=A261C142F)か、または [Visual Studio サブスクライバー特典を有効にする](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)ことができます。

>[AZURE.NOTE] Azure App Service が動作するところを見てから Azure アカウントにサインアップするには、[App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)をご覧ください。有効期間が短いスターター アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードや契約は必要ありません。

## 手順 1: 開発環境内で Sails.js アプリを作成する

最初に、次の手順に従って既定の Sails.js アプリをすばやく作成します。

1. 任意のコマンド ライン ターミナルと `CD` を作業ディレクトリに開きます。

2. 新しい Sails.js アプリを作成して実行します。

        sails new <appname>
        cd <appname>
        sails lift

    http://localhost:1377 で既定のホーム ページに移動できることを確認します。

## 手順 2: Azure で App Service アプリのリソースを作成する

次に App Service アプリのリソースを作成します。そのリソースに Sails.js アプリを後でデプロイします。

1. 同じターミナルで Azure にそのようにログインします。

        azure login

    画面の指示に従い、Azure サブスクリプションのある Microsoft アカウントを使用してブラウザーでログイン操作を進めます。

2. まだ Sails.js プロジェクトのルート ディレクトリにいることを確認します。次のコマンドを使用し、一意のアプリ名で Azure に App Service アプリ リソースを作成します。Web アプリの URL は http://&lt;appname>.azurewebsites.net になります。

        azure site create --git <appname>

    プロンプトに従って、デプロイ先の Azure リージョンを選択します。Azure サブスクリプションの Git/FTP デプロイの資格情報を設定したことがない場合は、資格情報の作成も求められます。

    App Service アプリのリソースの作成後:

    - Sails.js アプリは Git 初期化されます。
    - Git 初期化されたローカル リポジトリは Git リモートとして ("azure" という適切な名前が付けられた) 新しい App Service アプリに接続されます。
    - iisnode.yml ファイルがルート ディレクトリに作成されます。このファイルを使用して [iisnode](https://github.com/tjanczuk/iisnode) を構成できます。iisnode は Node.js アプリを実行するために App Service で使用されます。

## 手順 3: Sails.js アプリを構成およびデプロイする

 App Service での Sails.js アプリの使用は、次の 3 つの主要な手順で構成されます。

 - App Service で実行するようにアプリを構成する
 - アプリを App Service にデプロイする
 - デプロイに関するトラブルシューティングのために、stderr と stdout のログを読み取る

次の手順に従います。

1. ルート ディレクトリに新しい iisnode.yml ファイルを開いて、次の 2 行を追加します。

        loggingEnabled: true
        logDirectory: iisnode

    これで iisnode のログが有効になりました。このしくみの詳細については、「[iisnode から stdout ログと stderr ログを取得する](app-service-web-nodejs-get-started.md#iisnodelog)」を参照してください。

2. config/env/production.js を開いて、運用環境を構成して、`port` と `hookTimeout` を設定します。

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    これらの構成設定のドキュメントは [Sails.js ドキュメント](http://sailsjs.org/documentation/reference/configuration/sails-config)にあります。

    次に、[Grunt](https://www.npmjs.com/package/grunt) に Azure のネットワーク ドライブとの互換性があることを確認する必要があります。バージョン 1.0.0 未満の Grunt は、使用されている [glob](https://www.npmjs.com/package/glob) パッケージが古く (5.0.14 未満)、ネットワーク ドライブをサポートしていません。

3. package.json を開いて、`grunt` のバージョンを `1.0.0` に変更し、すべての `grunt-*` パッケージを削除します。変更後の `dependencies` プロパティは次のようになります。

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

6. 変更内容を保存して、アプリがまだローカルで実行されるかどうかを確認するために変更をテストします。そのためには、`node_modules` フォルダーを削除して次のコマンドを実行します。

        npm install
        sails lift

4. ここで git を使用して、アプリを Azure にデプロイします。

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. 最後に、ブラウザーでライブ Azure アプリを起動します。

        azure site browse

    同じ Sails.js ホーム ページが表示されます。
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## デプロイのトラブルシューティング

App Service で何らかの理由により Sails.js アプリケーションのエラーが発生した場合は、そのトラブルシューティングを行うのに役立つ stderr ログを探します。詳細については、「[Get stdout and stderr logs from iisnode (iisnode から stdout ログと stderr ログを取得する)](app-service-web-nodejs-sails.md#iisnodelog)」をご覧ください。Sails.js アプリケーションが正常に開始した場合は、見慣れたメッセージが stdout ログに表示されます。

                .-..-.

    Sails              <|    .-..-.
    v0.12.3             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\a76e8111-663e-449d-956e-5c5deff2d304
    To shut down Sails, press <CTRL> + C at any time.

stdout ログの粒度は [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) ファイルで制御することができます。

## Azure のデータベースへの接続

データベース Azure に接続するには、Azure SQL Database、MySQL、MongoDB、Azure (Redis) のキャッシュなど、Azure で任意のデータベースを作成して、対応する[データストア アダプター](https://github.com/balderdashy/sails#compatibility)を使用してそのデータベースに接続します。このセクションの手順は、Azure SQL Database に接続する方法を説明しています。

1. [こちら](../sql-database/sql-database-get-started.md)のチュートリアルに従って、新しい SQL Server に空の Azure SQL Database を作成します。既定のファイアウォール設定では、Azure サービス (たとえば App Service) に接続できます。

2. コマンド ライン ターミナルから、SQL Server のアダプターをインストールします。

        npm install sails-sqlserver --save

3. config/connections.js を開いて次の接続オブジェクトをリストに追加します。

        sqlserver: {
            adapter: 'sails-sqlserver',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.sqlserver, 
            database: process.env.dbname,
            options: {
                encrypt: true   // use this for Azure databases
            }
        },

4. 環境変数 (`process.env.*`) ごとに、App Service で設定する必要があります。それには、ターミナルから次のコマンドを実行します。

        azure site appsetting add dbuser="<database server administrator>"
        azure site appsetting add dbpassword="<database server password>"
        azure site appsetting add sqlserver="<database server name>.database.windows.net"
        azure site appsetting add dbname="<database name>"
        
    必要な設定を Azure のアプリケーション設定に置くことで、ソース管理 (Git) の対象から機密データを除外することができます。次に、同じ接続情報を使用するように開発環境を構成します。

4. config/local.js を開き、次の接続オブジェクトを追加します。

        connections: {
            sqlserver: {
                user: "<database server administrator>",
                password: "<database server password>",
                host: "<database server name>.database.windows.net", 
                database: "<database name>",
            },
        },
    
    ローカル環境の config/connections.js ファイルに含まれている設定は、この構成によって上書きされます。このファイルは、プロジェクトの既定の .gitignore によって除外され、Git には保管されません。これで、Azure Web アプリとローカル開発環境のどちらからでも、Azure SQL Database に接続できるようになりました。

4. config/env/production.js を開いて運用環境を構成し、次の `models` オブジェクトを追加します。

        models: {
            connection: 'sqlserver',
            migrate: 'safe'
        },

4. config/env/development.js を開いて開発環境を構成し、次の `models` オブジェクトを追加します。

        models: {
            connection: 'sqlserver',
            migrate: 'alter'
        },

    `migrate: 'alter'` を指定すると、データベース移行機能を使って簡単に、Azure SQL Database のデータベース テーブルを作成したり更新したりすることができます。ただし、Azure (運用) 環境で使用されるのは `migrate: 'safe'` です。Sails.js では、運用環境で `migrate: 'alter'` を使用することが認められていません ([Sails.js のドキュメント](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)を参照)。

4. ターミナルから、普段と同じように Sails.js の [blueprint API](http://sailsjs.org/documentation/concepts/blueprints) を[生成](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate)し、`sails lift` を実行して、データベースを作成すると共に Sails.js データベースを移行します。次に例を示します。

         sails generate api mywidget
         sails lift

    このコマンドによって生成される `mywidget` モデルは空ですが、このモデルを使用することで、データベース接続の存在を証明できます。`sails lift` を実行すると、アプリで使用されているモデルに足りないテーブルが作成されます。

6. 今作成した blueprint API にブラウザーでアクセスします。次に例を示します。

        http://localhost:1337/mywidget/create
    
    この API からは、作成されたエントリがブラウザー ウィンドウに返されます。つまり、これをもって、データベースが正常に作成されたと考えることができます。

        {"id":1,"createdAt":"2016-03-28T23:08:01.000Z","updatedAt":"2016-03-28T23:08:01.000Z"}

5. 変更内容を Azure にプッシュし、アプリに移動して、アプリがまだ動作しているかどうかを確認します。

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Azure Web アプリの blueprint API にアクセスします。次に例を示します。

        http://<appname>.azurewebsites.net/mywidget/create

    API から別の新しいエントリが返されれば、Azure Web アプリが Azure SQL Database と対話しているということです。

## その他のリソース

- [Get started with Node.js web apps in Azure App Service (Azure App Service で Node.js Web アプリの使用を開始する)](app-service-web-nodejs-get-started.md)
- [Azure アプリケーションでの Node.js モジュールの使用](../nodejs-use-node-modules-azure-apps.md)

<!---HONumber=AcomDC_0720_2016-->