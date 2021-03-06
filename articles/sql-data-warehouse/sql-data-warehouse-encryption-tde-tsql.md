<properties
   pageTitle="SQL Data Warehouse Transparent Data Encryption (TDE) TSQL の概要| Microsoft Azure"
   description="SQL Data Warehouse Transparent Data Encryption (TDE) TSQL の概要"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/29/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# Transparent Data Encryption (TDE) の概要


> [AZURE.SELECTOR]
- [セキュリティの概要](sql-data-warehouse-overview-manage-security.md)
- [脅威の検出](sql-data-warehouse-security-threat-detection.md)
- [暗号化 (ポータル)](sql-data-warehouse-encryption-tde.md)
- [暗号化 (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
- [監査の概要](sql-data-warehouse-auditing-overview.md)
- [ダウンレベル クライアントの監査](sql-data-warehouse-auditing-downlevel-clients.md)


Azure SQL Data Warehouse の Transparent Data Encryption (TDE) を使用すると、データベース、関連付けられているバックアップ、保管されているトランザクション ログ ファイルの暗号化と暗号化解除をリアルタイムで実行することにより、悪意のあるアクティビティの脅威からデータを保護できます。アプリケーションを変更する必要はありません。

TDE は、データベース暗号化キーと呼ばれる対称キーを使用してデータベース全体のストレージを暗号化します。SQL Database では、データベース暗号化キーは組み込まれているサーバー証明書によって保護されます。組み込みのサーバー証明書は、SQL Database サーバーごとに一意です。Microsoft は、少なくとも 90 日ごとにこれらの証明書を自動的にローテーションします。SQL Data Warehouse で使用される暗号化アルゴリズムは AES-256 です。TDE の一般的な説明については、「[透過的なデータ暗号化 (TDE)]」を参照してください。

##暗号化の有効化

SQL Data Warehouse の TDE を有効にするには、次の手順を実行します。

1. 管理者のログインまたは master データベースの **dbmanager** ロールのメンバーであるログインを使用して、データベースをホストしているサーバーの *master* データベースに接続します
2. データベースの暗号化するには、次のステートメントを実行します。

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

##暗号化の無効化

SQL Data Warehouse の TDE を無効にするには、次の手順を実行します。

1. 管理者のログインまたは master データベースの **dbmanager** ロールのメンバーであるログインを使用して、*master* データベースに接続します
2. データベースの暗号化するには、次のステートメントを実行します。

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

注: 一時停止した SQL Data Warehouse は、TDE 設定を変更する前に再開する必要があります。

##暗号化の検証

SQL Data Warehouse の暗号化状態を確認するには、次の手順を実行します。

1. 管理者のログインまたは master データベースの **dbmanager** ロールのメンバーであるログインを使用して、*master* データベースまたはインスタンス データベースに接続します
2. データベースの暗号化するには、次のステートメントを実行します。

```sql
SELECT
	[name],
	[is_encrypted]
FROM
	sys.databases;
```

結果が ```1``` の場合はデータベースが暗号化されていることを示し、```0``` の場合は暗号化されていないことを示します。

##暗号化の DMV  

- [sys.databases][]
- [sys.dm\_pdw\_nodes\_database\_encryption\_keys][]


<!--Anchors-->
[透過的なデータ暗号化 (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm\_pdw\_nodes\_database\_encryption\_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->

<!--Link references-->

<!---HONumber=AcomDC_0907_2016-->