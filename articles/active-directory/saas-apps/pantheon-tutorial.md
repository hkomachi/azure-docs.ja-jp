---
title: 'チュートリアル: Azure Active Directory シングル サインオン (SSO) と Pantheon の統合 | Microsoft Docs'
description: Azure Active Directory と Pantheon の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/09/2021
ms.author: jeedes
ms.openlocfilehash: e85a65a619f858a91b5b375bbb60c47adbb3af86
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2021
ms.locfileid: "132300168"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-pantheon"></a>チュートリアル: Azure Active Directory シングル サインオン (SSO) と Pantheon の統合

このチュートリアルでは、Pantheon と Azure Active Directory (Azure AD) を統合する方法について説明します。 Azure AD と Pantheon を統合すると、次のことができます。

* Pantheon にアクセスできるユーザーを Azure AD で制御できます。
* ユーザーが自分の Azure AD アカウントを使用して Pantheon に自動的にサインインできるように設定できます。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を取得できます。
* Pantheon でのシングル サインオン (SSO) が有効なサブスクリプション。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD の SSO を構成してテストします。

* Pantheon では、**IDP** によって開始される SSO がサポートされます。

## <a name="add-pantheon-from-the-gallery"></a>ギャラリーからの Pantheon の追加

Azure AD への Pantheon の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Pantheon を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、Azure portal にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。
1. **[ギャラリーから追加する]** セクションで、検索ボックスに、「**Pantheon**」と入力します。
1. 結果のパネルから **[Pantheon]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。

## <a name="configure-and-test-azure-ad-sso-for-pantheon"></a>Pantheon のための Azure AD SSO の構成とテスト

**B.Simon** というテスト ユーザーを使用して、Pantheon に対する Azure AD SSO を構成してテストします。 SSO が機能するためには、Azure AD ユーザーと Pantheon の関連ユーザーとの間にリンク関係を確立する必要があります。

Pantheon 用に Azure AD SSO を構成してテストするには、次の手順を行います。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[Pantheon SSO の構成](#configure-pantheon-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    1. **[Pantheon のテスト ユーザーの作成](#create-pantheon-test-user)** - Pantheon で B.Simon に対応するユーザーを作成し、Azure AD の B. Simon にリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

## <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. Azure portal の **Pantheon** アプリケーション統合ページで、 **[管理]** セクションを見つけて、 **[シングル サインオン]** を選択します。
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。
1. **[SAML によるシングル サインオンのセットアップ]** ページで、 **[基本的な SAML 構成]** の鉛筆アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

1. **[SAML でシングル サインオンをセットアップします]** ページで、次の手順を実行します。

    a. **[識別子]** ボックスに、`urn:auth0:pantheon:<orgname>-SSO` の形式で値を入力します。

    b. **[応答 URL]** ボックスに、`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO` のパターンを使用して URL を入力します

    > [!NOTE]
    > これらは実際の値ではありません。 実際の識別子と応答 URL でこれらの値を更新します。 これらの値を取得するには、[Pantheon クライアント サポート チーム](https://pantheon.io/docs/getting-support/)に連絡してください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

1. Pantheon アプリケーションは、特定の形式の SAML アサーションを使用するため、カスタム属性のマッピングを SAML トークンの属性の構成に追加する必要があります。 次のスクリーンショットは、既定の属性の一覧を示しています。ここで、**nameidentifier** は **user.userprincipalname** にマップされています。 Pantheon アプリケーションでは、**nameidentifier** が **user.mail** にマップされると想定されているため、**[編集]** アイコンをクリックして属性マッピングを編集し、属性マッピングを変更する必要があります。

    ![image](common/edit-attribute.png)

1. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションで、 **[証明書 (Base64)]** を見つけて、 **[ダウンロード]** を選択し、証明書をダウンロードして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/certificatebase64.png)

1. **[Pantheon のセットアップ]** セクションで、要件に基づいて適切な URL をコピーします。

    ![構成 URL のコピー](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションでは、Azure portal 内で B.Simon というテスト ユーザーを作成します。

1. Azure portal の左側のウィンドウから、 **[Azure Active Directory]** 、 **[ユーザー]** 、 **[すべてのユーザー]** の順に選択します。
1. 画面の上部にある **[新しいユーザー]** を選択します。
1. **[ユーザー]** プロパティで、以下の手順を実行します。
   1. **[名前]** フィールドに「`B.Simon`」と入力します。  
   1. **[ユーザー名]** フィールドに「username@companydomain.extension」と入力します。 たとえば、「 `B.Simon@contoso.com` 」のように入力します。
   1. **[パスワードを表示]** チェック ボックスをオンにし、 **[パスワード]** ボックスに表示された値を書き留めます。
   1. **Create** をクリックしてください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、B.Simon に Pantheon へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で **[Pantheon]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。
1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。
1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. SAML アサーション内に任意のロール値が必要な場合、 **[ロールの選択]** ダイアログでユーザーに適したロールを一覧から選択し、画面の下部にある **[選択]** をクリックします。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

## <a name="configure-pantheon-sso"></a>Pantheon の SSO の構成

**Pantheon** 側にシングルサインオンを構成するには、ダウンロードされた **証明書 (Base64)** およびコピーされた適切な URL を [Pantheon サポート チーム](https://pantheon.io/docs/getting-support/)に送信する必要があります。

> [!Note]
> この接続を有効にするには、電子メール ドメイン情報と日時も提供する必要があります。 詳細については、[こちら](https://pantheon.io/docs/sso-organizations/)を参照してください。

### <a name="create-pantheon-test-user"></a>Pantheon のテスト ユーザーの作成

このセクションでは、Pantheon で B.Simon というユーザーを作成します。 Pantheon でユーザーを追加するには、次の手順に従ってください。 

>[!NOTE] 
>SSO が機能するには、まず Pantheon でユーザーを作成する必要があります。

1. 管理者の資格情報で Pantheon にサインインします。

2. **[組織]** ダッシュボード ページに移動します。
 
3. **[People]\(ユーザー\)** をクリックします。

4. **[ユーザーの追加]** をクリックします。

5. ユーザーの電子メール アドレスを入力します。

6. ユーザーの役割を選択します。

7. **[ユーザーの追加]** をクリックします。

## <a name="test-sso"></a>SSO のテスト 

このセクションでは、次のオプションを使用して Azure AD のシングル サインオン構成をテストします。

* Azure portal で [このアプリケーションをテストします] をクリックすると、SSO を設定した Pantheon に自動的にサインインされます。

* Microsoft マイ アプリを使用することができます。 マイ アプリで [Pantheon] タイルをクリックすると、SSO を設定した Pantheon に自動的にサインインされます。 マイ アプリの詳細については、[マイ アプリの概要](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510)に関するページを参照してください。

## <a name="next-steps"></a>次のステップ

Pantheon を構成したら、組織の機密データを流出と侵入からリアルタイムで保護するセッション制御を適用できます。 セッション制御は、条件付きアクセスを拡張したものです。 [Microsoft Defender for Cloud Apps でセッション制御を強制する方法](/cloud-app-security/proxy-deployment-aad)をご覧ください。
