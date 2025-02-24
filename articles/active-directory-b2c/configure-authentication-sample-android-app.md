---
title: Azure Active Directory B2C を使用してサンプル Android アプリケーションで認証を構成する
description: この記事では、Azure Active Directory B2C を使用して Android アプリケーションでユーザーをサインインおよびサインアップさせる方法について説明します。
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/05/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 699e548809b123589466062f1e9600edee9e7b78
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2021
ms.locfileid: "131007937"
---
# <a name="configure-authentication-in-a-sample-android-app-by-using-azure-ad-b2c"></a>Azure AD B2C を使用して サンプル Android アプリで認証オプションを構成する

この記事では、サンプルの Android アプリケーション (Kotlin および Java) を使用して、モバイル アプリに Azure Active Directory B2C (Azure AD B2C) 認証を追加する方法を説明します。

## <a name="overview"></a>概要

OpenID Connect (OIDC) は、OAuth 2.0 を基盤にした認証プロトコルです。 OIDC を使用して、ユーザーを安全にアプリケーションにサインインさせることができます。 このモバイル アプリのサンプルでは、OIDC 認可コード PKCE フローで [Microsoft Authentication Library (MSAL)](../active-directory/develop/msal-overview.md) を使用します。 MSAL は、モバイル アプリへの認証と認可サポートの追加を簡略化する、Microsoft 提供のライブラリです。 

サインイン フローでは、次の手順が実行されます。

1. ユーザーがアプリを開き、**[サインイン]** を選択します。
1. アプリによってモバイル デバイスのシステム ブラウザーが開き、Azure AD B2C に対する認証要求が開始されます。
1. ユーザーが[サインアップまたはサインイン](add-sign-up-and-sign-in-policy.md)、[パスワードのリセット](add-password-reset-policy.md)、または[ソーシャル アカウント](add-identity-provider.md)を使用したサインインを行います。
1. ユーザーがサインインに成功すると、Azure AD B2C からアプリに認可コードが返されます。
1. アプリによって次のアクションが実行されます。
    1. 認可コードが、ID トークン、アクセス トークン、更新トークンと交換されます。
    1. ID トークン要求が読み取られます。
    1. 後で使用できるように、トークンがメモリ内キャッシュに格納されます。

### <a name="app-registration-overview"></a>アプリ登録の概要

アプリで Azure AD B2C を使用してサインインし、Web API を呼び出せるようにするには、Azure AD B2C ディレクトリに 2 つのアプリケーションを登録します。  

- **モバイル アプリケーション** の登録により、アプリでは Azure AD B2C を使用してサインインできるようになります。 アプリの登録時に、″リダイレクト URI″ を指定します。 リダイレクト URI は、ユーザーが Azure AD B2C で認証を行った後、Azure AD B2C によってリダイレクトされるエンドポイントです。 アプリの登録プロセスによって、モバイル アプリを一意に識別する "アプリケーション ID″ ("クライアント ID″ とも呼ばれる) が生成されます (たとえば "アプリ ID: 1″)。

- **Web API** を登録すると、保護された Web API をアプリで呼び出すことができます。 この登録により、Web API のアクセス許可 (スコープ) が公開されます。 アプリの登録プロセスによって、Web API を一意に識別する "*アプリケーション ID*" が生成されます (たとえば "*アプリ ID: 2*")。 モバイル アプリ (App ID: 1) のアクセス許可を Web API スコープ (App ID: 2) に付与します。 


アプリの登録とアプリケーションのアーキテクチャについて、次の図に示します。

![Web API 呼び出しの登録とトークンが使用されているモバイル アプリの図。](./media/configure-authentication-sample-android-app/mobile-app-with-api-architecture.png) 

### <a name="call-to-a-web-api"></a>Web API の呼び出し

[!INCLUDE [active-directory-b2c-app-integration-call-api](../../includes/active-directory-b2c-app-integration-call-api.md)]

### <a name="the-sign-out-flow"></a>サインアウト フロー

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)]

## <a name="prerequisites"></a>前提条件

次が実行されているコンピューター: 


- [Java Development Kit (JDK) 8 以降](https://openjdk.java.net/)
- [Apache Maven](https://maven.apache.org/)
- [Android API レベル 16 以降](https://developer.android.com/studio/releases/platforms)
- [Android Studio](https://developer.android.com/studio)、または別のコード エディター


## <a name="step-1-configure-your-user-flow"></a>手順 1: ユーザー フローを構成する

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](../../includes/active-directory-b2c-app-integration-add-user-flow.md)]

## <a name="step-2-register-mobile-applications"></a>手順 2: モバイル アプリケーションを登録する

モバイル アプリと Web API アプリケーションの登録を作成し、Web API のスコープを指定します。

### <a name="step-21-register-the-web-api-app"></a>手順 2.1: Web API アプリを登録する

[!INCLUDE [active-directory-b2c-app-integration-register-api](../../includes/active-directory-b2c-app-integration-register-api.md)]

### <a name="step-22-configure-web-api-app-scopes"></a>手順 2.2: Web API アプリのスコープを構成する

[!INCLUDE [active-directory-b2c-app-integration-api-scopes](../../includes/active-directory-b2c-app-integration-api-scopes.md)]


### <a name="step-23-register-the-mobile-app"></a>手順 2.3: モバイル アプリを登録する

モバイル アプリの登録を作成するには、次の手順を実行します。

1. [Azure portal](https://portal.azure.com) にサインインします。
1. **[アプリの登録]** を選択し、 **[新規登録]** を選択します。
1. **[名前]** には、アプリケーションの名前を入力します (たとえば *android-app1*)。
1. **[サポートされているアカウントの種類]** で、 **[Accounts in any identity provider or organizational directory (for authenticating users with user flows)]\((ユーザー フローを使用してユーザーを認証するための) 任意の ID プロバイダーまたは組織のディレクトリのアカウント\)** を選択します。 
1. **[リダイレクト URI]** で、**[パブリック クライアント/ネイティブ (モバイルとデスクトップ)]** を選択してから、URL ボックスに次のいずれかの URI を入力します。
    - Kotlin サンプルの場合: `msauth://com.azuresamples.msalandroidkotlinapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D`
    - Java サンプルの場合: `msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D`
1. **[登録]** を選択します。
1. アプリ登録が完了したら、 **[概要]** を選択します。
1. **アプリケーション (クライアント) ID** を記録しておきます。これは、後でモバイル アプリケーションを構成するときに使用します。

    ![Android アプリケーション ID が強調表示されたスクリーンショット](./media/configure-authentication-sample-android-app/get-azure-ad-b2c-app-id.png)  


### <a name="step-24-grant-the-mobile-app-permissions-for-the-web-api"></a>手順 2.4: Web API に対するアクセス許可をモバイル アプリに付与する

[!INCLUDE [active-directory-b2c-app-integration-grant-permissions](../../includes/active-directory-b2c-app-integration-grant-permissions.md)]

## <a name="step-3-get-the-android-mobile-app-sample"></a>手順 3: Android モバイル アプリのサンプルを取得する

以下のいずれかを実行します。

- 次のいずれかのサンプルをダウンロードします。 
   - [Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin/archive/refs/heads/master.zip)
   - [Java](https://github.com/Azure-Samples/ms-identity-android-java/archive/refs/heads/master.zip) 

   サンプルの .zip ファイルを作業フォルダーに抽出します。

- GitHub からサンプル Android モバイル アプリケーションをクローンします。 

    #### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


    ```bash
    git clone https://github.com/Azure-Samples/ms-identity-android-kotlin
    ```

    #### <a name="java"></a>[Java](#tab/java)

    ```bash
    git clone https://github.com/Azure-Samples/ms-identity-android-java
    ```

    --- 


## <a name="step-4-configure-the-sample-web-api"></a>手順 4: サンプル Web API を構成する

このサンプルでは、モバイル アプリが Web API に対して使用できる、関連するスコープを持つアクセス トークンを取得します。 コードから Web API を呼び出すには、次の手順を実行します。

1. 既存の Web API を使用するか、新しいものを作成します。 詳細については、「[Azure AD B2C を使用して独自の Web API で認証を有効にする](enable-authentication-web-api.md)」を参照してください。
1. [Web API を呼び出す](enable-authentication-android-app.md#step-6-call-a-web-api)ようにサンプル コードを変更します。

## <a name="step-5-configure-the-sample-mobile-app"></a>手順 5: サンプル モバイル アプリを構成する

Android Studio またはその他のコード エディターでサンプル プロジェクトを開き、*/app/src/main/res/raw/auth_config_b2c.json* ファイルを開きます。 

*auth_config_b2c.json* 構成ファイルには、Azure AD B2C ID プロバイダーに関する情報が含まれています。 モバイル アプリでは、この情報を使用して Azure AD B2C との信頼関係を確立し、ユーザーのサインインとサインアウトを行い、トークンを取得して検証します。 

次のアプリ設定のプロパティを更新します。

|キー  |値  |
|---------|---------|
| [client_id](../active-directory/develop/msal-client-application-configuration.md#client-id) | [手順 2.3](#step-23-register-the-mobile-app) のモバイル アプリケーション ID。 | 
| [redirect_uri](../active-directory/develop/msal-client-application-configuration.md#redirect-uri) | [手順 2.3](#step-23-register-the-mobile-app) のモバイル アプリケーション リダイレクト URI。 | 
| [authorities](../active-directory/develop/msal-client-application-configuration.md#authority)| 機関は、MSAL がトークンを要求できるディレクトリを示す URL です。 次の形式を使用します: `https://<your-tenant-name>.b2clogin.com/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>`。 `<your-tenant-name>` を Azure AD B2C の[テナント名](tenant-management.md#get-your-tenant-name)に置き換えます。 次に、`<your-sign-in-sign-up-policy>` を、[手順 1](#step-1-configure-your-user-flow) で作成したユーザー フローまたはカスタム ポリシーに置き換えます。 |
| | | 


`B2CConfiguration` クラスを開き、次のクラス メンバーを更新します。

|キー  |値  |
|---------|---------|
| ポリシー| [手順 1](#step-1-configure-your-user-flow) で作成したユーザー フローまたはカスタム ポリシーの一覧。|
| azureAdB2CHostName| Azure AD B2C [テナント名](tenant-management.md#get-your-tenant-name)の最初の部分 (例: `https://contoso.b2clogin.com`)。|
| tenantName| Azure AD B2C テナントの完全な[テナント名](tenant-management.md#get-your-tenant-name) (例: `contoso.onmicrosoft.com`)。|
| スコープ| [手順 2.4](#step-24-grant-the-mobile-app-permissions-for-the-web-api) で作成した Web API スコープ。|
| | |


## <a name="step-6-run-and-test-the-mobile-app"></a>手順 6: モバイル アプリを実行してテストする

1. プロジェクトをビルドして実行します。
1. 以下のような、左上にあるハンバーガー アイコン (折りたたまれたメニュー) を選択します。
    
    ![ハンバーガー (または折りたたまれたメニュー) アイコンが強調表示されたスクリーンショット。](./media/configure-authentication-sample-android-app/select-hamburger-icon.png)

1. 左側のペインで、**[B2C モード]** を選択します。

    ![左側のペインで [B2C モード] コマンドが強調表示されたスクリーンショット。](./media/configure-authentication-sample-android-app/select-azure-ad-b2c-mode.png)

1. **[ユーザー フローを実行します]** を選択します。

    ![[ユーザー フローを実行します] ボタンが強調表示されたスクリーンショット。](./media/configure-authentication-sample-android-app/select-policy-and-sign-in.png)

1. Azure AD B2C のローカルまたはソーシャル アカウントを使用してサインアップまたはサインインします。

1. 認証が成功すると、**[B2C モード]** ペインに表示名が表示されます。

    ![サインインしているユーザーとポリシーが表示された、成功した認証を示すスクリーンショット。](./media/configure-authentication-sample-android-app/access-token.png) 

## <a name="next-steps"></a>次のステップ

具体的には、次の方法を学習します。
* [独自の Android アプリで認証を有効にする](enable-authentication-android-app.md)
* [Android アプリで認証オプションを構成する](enable-authentication-android-app-options.md)
* [ユーザー自身の Web API で認証を有効にする](enable-authentication-web-api.md)
