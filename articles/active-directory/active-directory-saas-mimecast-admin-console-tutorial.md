<properties 
    pageTitle="チュートリアル: Azure Active Directory と Mimecast Admin Console の統合 | Microsoft Azure" 
    description="Azure Active Directory で Mimecast Admin Console を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="07/08/2016" 
    ms.author="jeedes" />

#チュートリアル: Azure Active Directory と Mimecast Admin Console の統合
  
このチュートリアルでは、Azure と Mimecast Admin Console の統合について説明します。このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Mimecast Admin Console でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Mimecast Admin Console に割り当てた Azure AD ユーザーは、Mimecast Admin Console 企業サイト (サービス プロバイダーが開始したサインオン) で、または「[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)」に従って、アプリケーションにシングル サインオンできるようになります。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Mimecast Admin Console のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "シナリオ")
##Mimecast Admin Console のアプリケーション統合の有効化
  
このセクションでは、Mimecast Admin Console のアプリケーション統合を有効にする方法を説明します。

###Mimecast Admin Console のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure クラシック ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "アプリケーション")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "アプリケーションの追加")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "ギャラリーからのアプリケーションの追加")

6.  **[検索ボックス]** に、**[Mimecast Admin Console]** と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "アプリケーション ギャラリー")

7.  結果ウィンドウで **[Mimecast Admin Console]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Mimecast Admin Console](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast Admin Console")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Mimecast Admin Console に対する認証を行うことができるようにする方法を説明します。この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。この手順に慣れていない場合は、「[How to convert a binary certificate into a text file (バイナリ証明書をテキスト ファイルに変換する方法)](http://youtu.be/PlgrzUZ-Y1o)」をご覧ください。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure クラシック ポータルの **[Mimecast Admin Console]** アプリケーション統合ページで **[シングル サインオンの構成]** をクリックし、**[シングル サインオンの構成]** ダイアログを開きます。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Configure Single Sign-On")

2.  **[ユーザーの Mimecast Admin Console へのアクセスを設定してください]** ページで、**[Microsoft Azure AD のシングル サインオン]** を選択し、**[次へ]** をクリックします。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Configure Single Sign-On")

3.  **[アプリ URL の構成]** ページで、**[Mimecast Admin Console サインオン URL]** テキストボックスに、ユーザーが Mimecast Admin Console アプリケーションへのサインオンする際に使用する URL (例: “https://webmail-uk.mimecast.com” や “https://webmail-us.mimecast.com”) を入力して、**[次へ]** をクリックします。

    >[AZURE.NOTE] サインオン URL は、リージョン固有のものになります。

    ![Configure App URL](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "アプリケーション URL の構成")

4.  **[Mimecast Admin Console でのシングル サインオンの構成]** ページで、**[証明書のダウンロード]** をクリックして証明書をダウンロードし、証明書ファイルをコンピューターのローカルに保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Mimecast Admin Console の企業サイトに管理者としてログインします。

6.  **[サービス] > [ アプリケーション]** に移動します。

    ![サービス](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "サービス")

7.  **[認証プロファイル]** をクリックします。

    ![認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "認証プロファイル")

8.  **[新しい認証プロファイル]** をクリックします。

    ![新しい認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "新しい認証プロファイル")

9.  [**認証プロバイダー**] セクションで、次の手順に従います。

    ![認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "認証プロファイル")

    1.  **[説明]** テキストボックスに、構成の名前を入力します。
    2.  **[Mimecast Admin Console に SAML 認証を適用]** を選択します。
    3.  **[プロバイダー]** で **[Azure Active Directory]** を選択します。
    4.  Azure クラシック ポータルの **[Mimecast Admin Console でのシングル サインオンの構成]** ダイアログ ページで **[発行者 URL]** の値をコピーし、それを **[発行者 URL]** テキストボックスに貼り付けます。
    5.  Azure クラシック ポータルの **[Mimecast Admin Console でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログイン URL]** の値をコピーし、それを **[ログイン URL]** テキストボックスに貼り付けます。
    6.  Azure クラシック ポータルの **[Mimecast Admin Console でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログイン URL]** の値をコピーし、それを **[ログアウト URL]** テキストボックスに貼り付けます。

        >[AZURE.NOTE]Mimecast Admin Console では、[ログイン URL] と [ログアウト URL] の値は同じです。

    7.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。

        >[AZURE.TIP]詳細については、「[How to convert a binary certificate into a text file (バイナリ証明書をテキスト ファイルに変換する方法)](http://youtu.be/PlgrzUZ-Y1o)」をご覧ください。

    8.  base-64 でエンコードされた証明書をメモ帳で開き、最初の行 (“*--*“) と最後の行 (“*--*“) を削除し、残りの内容をクリップボードにコピーして、**[ID プロバイダー証明書 (メタデータ)]** テキストボックスに貼り付けます。
    9.  **[シングル サインオンの許可]** を選択します。
    10. **[保存]** をクリックします。

10. Azure クラシック ポータルで、[シングル サインオンの構成の確認] を選択し、**[完了]** をクリックして **[シングル サインオンの構成]** ダイアログを閉じます。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Mimecast Admin Console にログインできるようにするには、そのユーザーを Mimecast Admin Console にプロビジョニングする必要があります。Mimecast Admin Console の場合、プロビジョニングは手動で行います。
  
ユーザーを作成する前に、ドメインを登録する必要があります。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  管理者として **[Mimecast Admin Console]** にサインオンします。

2.  **[ディレクトリ] > [内部]** に移動します。

    ![ディレクトリ](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "ディレクトリ")

3.  **[新規ドメインの登録]** をクリックします。

    ![新規ドメインの登録](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "新規ドメインの登録")

4.  新しいドメインを作成したら、**[新規アドレス]** をクリックします。

    ![新規アドレス](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "新規アドレス")

5.  新しいアドレスのダイアログで、次の手順に従います。

    ![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Save")

    1.  該当するテキストボックスに、プロビジョニングする有効な AAD アカウントの **[メール アドレス]**、**[グローバル名]**、**[パスワード]**、**[パスワードの確認]** 属性を入力します。
    2.  **[保存]** をクリックします。

>[AZURE.NOTE]Mimecast Admin Console から提供されている他の IdeaScale ユーザー アカウント作成ツールや API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Mimecast Admin Console に割り当てるには、次の手順に従います。

1.  Azure クラシック ポータルで、テスト アカウントを作成します。

2.  **Mimecast Admin Console** アプリケーション統合ページで、**[ユーザーの割り当て]** をクリックします。

    ![ユーザーの割り当て](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "ユーザーの割り当て")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![Yes](./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。アクセス パネルの詳細については、「[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)」をご覧ください。

<!---HONumber=AcomDC_0713_2016-->