<properties
pageTitle="Trello | Microsoft Azure"
description="Azure App Service を使用してロジック アプリを作成します。Trello を使用すると、職場でも家庭でも、すべてのプロジェクトを把握できます。プロジェクトを管理したり何かを整理するための、簡単で、自由度が高く、柔軟性があり、視覚化された方法です。Trello に接続してボード、一覧、およびカードを管理する"
services="logic-apps"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# Trello コネクタの使用

Trello を使用すると、職場でも家庭でも、すべてのプロジェクトを把握できます。プロジェクトを管理したり何かを整理するための、簡単で、自由度が高く、柔軟性があり、視覚化された方法です。Trello に接続すると、ボード、一覧、およびカードを管理できます。

>[AZURE.NOTE] 本記事は、ロジック アプリの 2015-08-01-preview スキーマ バージョンを対象としています。

まず、ロジック アプリを作成します。[ロジック アプリの作成](../app-service-logic/app-service-logic-create-a-logic-app.md)に関する記事をご覧ください。

## トリガーとアクション

Trello コネクタは、アクションとして使用できます。Trello コネクタにはトリガーがあります。すべてのコネクタは、JSON および XML 形式のデータに対応します。

 Trello コネクタでは、次のアクションやトリガーを使用できます。

### Trello のアクション
実行できるアクションは以下のとおりです。

|アクション|Description|
|--- | ---|
|[ListCards](connectors-create-api-trello.md#listcards)|ボードのカードを一覧表示します|
|[GetCard](connectors-create-api-trello.md#getcard)|ID を使用してカードを取得します|
|[UpdateCard](connectors-create-api-trello.md#updatecard)|カードを更新します|
|[DeleteCard](connectors-create-api-trello.md#deletecard)|カードを削除します|
|[CreateCard](connectors-create-api-trello.md#createcard)|Trello アカウントに新しいカードを作成します|
|[ListBoards](connectors-create-api-trello.md#listboards)|ボードを一覧表示します|
|[GetBoard](connectors-create-api-trello.md#getboard)|ID を使用してボードを取得します|
|[ListLists](connectors-create-api-trello.md#listlists)|ボードのカードの一覧を一覧表示します|
|[GetList](connectors-create-api-trello.md#getlist)|ID を使用して一覧を取得します|
### Trello のトリガー
次のイベントをリッスンできます。

|トリガー | Description|
|--- | ---|
|ボードに新しいカードが追加されたタイミング|ボードに新しいカードが追加されたときに、フローをトリガーします|
|一覧に新しいカードが追加されたタイミング|一覧に新しいカードが追加されたときに、フローをトリガーします|


## Trello への接続を作成する
Trello を使用してロジック アプリを作成するには、まず**接続**を作成してから、次のプロパティの詳細を指定する必要があります。

|プロパティ| 必須|Description|
| ---|---|---|
|トークン|はい|Trello 資格情報を提供します|
接続を作成したら、その接続を使用してアクションを実行し、この記事で説明するトリガーをリッスンできます。

>[AZURE.INCLUDE [Trello への接続を作成する手順](../../includes/connectors-create-api-trello.md)]

>[AZURE.TIP] 他のロジック アプリでもこの接続を使用できます。

## Trello のリファレンス
適用されるバージョン: 1.0

## OnNewCardInBoard
ボードに新しいカードが追加されたタイミング: ボードに新しいカードが追加されたときに、フローをトリガーします。

```GET: /trigger/boards/{board_id}/cards```

| Name| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|path|なし|カードを取得するボードの一意の ID|

#### 応答

|Name|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## OnNewCardInList
一覧に新しいカードが追加されたタイミング: 一覧に新しいカードが追加されたときに、フローをトリガーします。

```GET: /trigger/lists/{list_id}/cards```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|カードを取得するボードの一意の ID|
|list\_id|string|○|path|なし|カードを取得する一覧の一意の ID|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## ListCards
ボードのカードの一覧表示: ボードのカードを一覧表示します。

```GET: /boards/{board_id}/cards```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|path|なし|すべてのカードを取得するボードの ID|
|actions|string|×|query|なし|返すアクションの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|attachments|boolean|×|query|なし|添付ファイルを表示するか|
|attachment\_fields|string|×|query|なし|返す添付ファイル フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|stickers|boolean|×|query|なし|ステッカーを表示するか|
|members|boolean|×|query|なし|メンバーを表示するか|
|memeber\_fields|string|×|query|なし|返すメンバー フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|CheckItemStates|boolean|×|query|なし|カードの状態を返すか|
|Checklists|string|×|query|なし|表示チェックリスト|
|limit|integer|×|query|なし|返す結果の最大数。1 ～ 1000|
|since|string|×|query|なし|この日付より後のすべてのカードを取得します|
|準備|string|×|query|なし|この日付より前のすべてのカードを取得します|
|filter|string|×|query|なし|応答をフィルター処理します|
|fields|string|×|query|なし|返すカード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|

#### 応答

|Name|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## GetCard
ID を使用するカードの取得: ID を使用してカードを取得します。

```GET: /cards/{card_id}```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|カードを取得するボードの ID|
|card\_id|string|○|path|なし|取得するカードの ID|
|actions|string|×|query|なし|返すアクションの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|actions\_entities|boolean|×|query|なし|アクション エンティティを返すか|
|actions\_display|boolean|×|query|なし|アクション表示を返すか|
|actions\_limit|integer|×|query|なし|返すアクションの最大数|
|action\_fields|string|×|query|なし|各アクションについて返すアクション フィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|action\_memberCreator\_fields|string|×|query|なし|返すアクション メンバー作成者のフィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|attachments|boolean|×|query|なし|添付ファイルを返すか|
|attachement\_fields|string|×|query|なし|各添付ファイルについて返す添付ファイルのフィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|members|boolean|×|query|なし|メンバーを返すか|
|member\_fields|string|×|query|なし|各メンバーについて対して返すメンバー フィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|membersVoted|boolean|×|query|なし|投票されたメンバーを返すか|
|memberVoted\_fields|string|×|query|なし|投票された各メンバーについて返す投票されたメンバー フィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|checkItemStates|boolean|×|query|なし|カードの状態を返すか|
|checkItemState\_fields|string|×|query|なし|各カード項目の状態について返す状態フィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|checklists|string|×|query|なし|返すチェックリスト|
|checklist\_fields|string|×|query|なし|各チェックリストについて返すチェックリスト フィールドの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|board|boolean|×|query|なし|カードが属するボードを返すか|
|board\_fields|string|×|query|なし|返すボード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|list|boolean|×|query|なし|カードが属する一覧を返すか|
|list\_fields|string|×|query|なし|返す一覧フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|stickers|boolean|×|query|なし|ステッカーを返すか|
|sticker\_fields|string|×|query|なし|各ステッカーについて返すステッカー フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|fields|string|×|query|なし|返すカード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## UpdateCard
カードの更新: カードを更新します。

```PUT: /cards/{card_id}```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|カードを取得するボードの ID|
|card\_id|string|○|path|なし|更新するカードの ID|
|updateCard| |○|body|なし|更新されたカード値|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## DeleteCard
カードの削除: カードを削除します。

```DELETE: /cards/{card_id}```

| Name| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|カードを取得するボードの ID|
|card\_id|string|○|path|なし|削除するカードの ID|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## CreateCard
カードの作成: Trello アカウントに新しいカードを作成します。

```POST: /cards```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|カードを作成するボードの一意の ID|
|newCard| |○|body|なし|Trello ボードに追加する新しいカード|

#### 応答

|Name|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## ListBoards
ボードの一覧表示: ボードを一覧表示します。

```GET: /member/me/boards```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|filter|string|×|query|なし|ボードの結果に適用するフィルターの一覧。'all' または有効な値のコンマ区切りの一覧を指定します|
|fields|string|×|query|なし|返すボード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|actions|string|×|query|なし|返すアクション フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|actions\_entities|boolean|×|query|なし|アクション エンティティを返すか|
|actions\_limit|integer|×|query|なし|返すアクションの最大数|
|actions\_format|string|×|query|なし|返すアクションの形式を指定します|
|actions\_since|string|×|query|なし|指定した日付より後のアクションを返します|
|action\_fields|string|×|query|なし|返すアクションのフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|memberships|string|×|query|なし|返すメンバーシップ情報を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|organization|boolean|×|query|なし|組織の情報を返すかどうかを指定します|
|organization\_fields|string|×|query|なし|返す組織フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|lists|string|×|query|なし|ボードに属している一覧を返すかどうかを指定します|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## GetBoard
ID を使用するボードの取得: ID を使用してボードを取得します。

```GET: /boards/{board_id}```

| Name| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|path|なし|取得するボードの一意の ID|
|actions|string|×|query|なし|返すアクションの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|action\_entities|boolean|×|query|なし|アクション エンティティを返すかどうかを指定します|
|actions\_display|boolean|×|query|なし|アクション表示を返すかどうかを指定します|
|actions\_format|string|×|query|なし|返すアクションの形式を指定します|
|actions\_since|string|×|query|なし|この日付より後のアクションのみを返します|
|actions\_limit|integer|×|query|なし|返すアクションの最大数|
|action\_fields|string|×|query|なし|各フィールドについて返すフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|action\_memeber|boolean|×|query|なし|アクション メンバーを返すかどうかを指定します|
|action\_member\_fields|string|×|query|なし|各アクション メンバーについて返すメンバー フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|action\_memberCreator|boolean|×|query|なし|アクション メンバー作成者を返すかどうかを指定します|
|action\_memberCreator\_fields|string|×|query|なし|返すアクション メンバー作成者フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|cards|string|×|query|なし|返すカードを指定します|
|card\_fields|string|×|query|なし|各カードについて返すフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|card\_attachments|boolean|○|query|なし|カードの添付ファイルを返すかどうかを指定します|
|card\_attachment\_fields|string|×|query|なし|各添付ファイルについて返す添付ファイル フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|card\_checklists|string|×|query|なし|各カードについて返すチェックリストを指定します|
|card\_stickers|boolean|×|query|なし|カード ステッカーを返すかどうかを指定します|
|boardStarts|string|×|query|なし|返すボードの星を指定します|
|ラベル|string|×|query|なし|返すラベルを指定します|
|label\_fields|string|×|query|なし|各カードについて返すラベル フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|labels\_limits|integer|×|query|なし|返すラベルの最大数|
|lists|string|×|query|なし|返す一覧を指定します|
|list\_fields|string|×|query|なし|各一覧について返す一覧フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|memberships|string|×|query|なし|返すメンバーシップの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|memberships\_member|boolean|×|query|なし|メンバーシップ メンバーを返すかどうかを指定します。|
|memberships\_member\_fields|string|×|query|なし|各メンバーシップ メンバーについて返すメンバー フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|members|string|×|query|なし|返すメンバーシップの一覧を指定します|
|member\_fields|string|×|query|なし|各メンバーについて返すメンバー フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|membersInvited|string|×|query|なし|返す招待されたメンバーを指定します|
|membersInvited\_fields|string|×|query|なし|それぞれについて返すフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|checklists|string|×|query|なし|返すチェックリストを指定します|
|checklist\_fields|string|×|query|なし|各チェックリストについて返すチェックリスト フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|organization|boolean|×|query|なし|組織の情報を返すかどうかを指定します|
|organization\_fields|string|×|query|なし|各組織について返す組織フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|organization\_memberships|string|×|query|なし|返す組織メンバーシップの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|myPerfs|boolean|×|query|なし|マイ パフォーマンスを返すかどうかを指定します|
|fields|string|×|query|なし|返すフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## ListLists
ボードのカード一覧の一覧表示: ボードのカード一覧を一覧表示します。

```GET: /boards/{board_id}/lists```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|path|なし|一覧を取得するボードの一意の ID|
|cards|string|×|query|なし|返すカードを指定します|
|card\_fields|string|×|query|なし|返すカード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|filter|string|×|query|なし|一覧のフィルター プロパティを指定します|
|fields|string|×|query|なし|返すフィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|

#### 応答

|名前|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## GetList
ID を使用する一覧の取得: ID を使用して一覧を取得します。

```GET: /lists/{list_id}```

| 名前| データ型|必須|場所|既定値|Description|
| ---|---|---|---|---|---|
|board\_id|string|○|query|なし|一覧を取得するボードの一意の ID|
|list\_id|string|○|path|なし|取得する一覧の一意の ID|
|cards|string|×|query|なし|返すカードを指定します|
|card\_fields|string|×|query|なし|各カードについて返すカード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|board|boolean|×|query|なし|ボード情報を返すかどうかを指定します|
|board\_fields|string|×|query|なし|返すボード フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|
|fields|string|×|query|なし|返す一覧フィールドの一覧を指定します。'all' または有効な値のコンマ区切りの一覧を指定します|

#### 応答

|Name|説明|
|---|---|
|200|OK|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました|


## オブジェクト定義 

### Card


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|id|string|なし |
|checkItemStates|array|なし |
|closed|boolean|なし |
|dateLastActivity|string|なし |
|desc|string|なし |
|idBoard|string|なし |
|idList|string|なし |
|idMembersVoted|array|なし |
|idShort|integer|なし |
|idAttachmentCover|string|なし |
|manualCoverAttachment|boolean|なし |
|idLabels|array|なし |
|name|string|なし |
|pos|integer|なし |
|shortLink|string|なし |
|badges|未定義|なし |
|due|string|なし |
|電子メール|string|なし |
|shortUrl|string|なし |
|subscribed|boolean|なし |
|url|string|なし |



### Badges


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|Votes|integer|なし |
|ViewingMemberVoted|boolean|なし |
|Subscribed|boolean|なし |
|Fogbugz|string|なし |
|CheckItems|integer|なし |
|CheckItemsChecked|integer|なし |
|説明|integer|なし |
|[添付ファイル]|integer|なし |
|Description|boolean|なし |
|Due|string|なし |



### オブジェクト


| プロパティ名 | データ型 | 必須 |
|---|---|---|



### CreateCard


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|idList|string|はい |
|name|string|はい |
|desc|string|なし |
|pos|string|なし |
|idMembers|string|なし |
|idLabels|string|なし |
|urlSource|string|なし |
|fileSource|string|なし |
|idCardSource|string|なし |
|keepFromSource|string|なし |



### UpdateCard


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|name|string|なし |
|desc|string|なし |
|closed|boolean|なし |
|idMembers|string|なし |
|idAttachmentCover|string|なし |
|idList|string|なし |
|idBoard|string|なし |
|pos|string|なし |
|due|string|なし |
|subscribed|boolean|なし |



### Board


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|id|string|なし |
|closed|boolean|なし |
|dateLastActivity|string|なし |
|dateLastView|string|なし |
|desc|string|なし |
|idOrganization|string|なし |
|invitations|array|なし |
|invited|boolean|なし |
|labelNames|未定義|なし |
|memberships|array|なし |
|name|string|なし |
|pinned|boolean|なし |
|powerUps|array|なし |
|perfs|未定義|なし |
|shortLink|string|なし |
|shortUrl|string|なし |
|starred|string|なし |
|subscribed|string|なし |
|url|string|なし |



### ラベル


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|green|string|なし |
|yellow|string|なし |
|orange|string|なし |
|red|string|なし |
|purple|string|なし |
|blue|string|なし |
|sky|string|なし |
|lime|string|なし |
|pink|string|なし |
|black|string|なし |



### Membership


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|id|string|なし |
|idMember|string|なし |
|memberType|string|なし |
|unconfirmed|boolean|なし |



### Perfs


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|permissionLevel|string|なし |
|voting|string|なし |
|コメント|string|なし |
|invitations|string|なし |
|selfJoin|boolean|なし |
|cardCovers|boolean|なし |
|calendarFeedEnabled|boolean|なし |
|background|string|なし |
|backgroundColor|string|なし |
|backgroundImage|string|なし |
|backgroundImageScaled|string|なし |
|backgroundTile|boolean|なし |
|backgroundBrightness|string|なし |
|canBePublic|boolean|なし |
|canBeOrg|boolean|なし |
|canBePrivate|boolean|なし |
|canInvite|boolean|なし |



### 一覧表示


| プロパティ名 | データ型 | 必須 |
|---|---|---|
|id|string|なし |
|name|string|なし |
|closed|boolean|なし |
|idBoard|string|なし |
|pos|number|なし |
|subscribed|boolean|なし |
|cards|array|なし |
|board|未定義|なし |


## 次のステップ
[ロジック アプリを作成します](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->