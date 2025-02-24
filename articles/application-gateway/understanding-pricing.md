---
title: 価格について - Azure Application Gateway
description: この記事では、v1 から v2 の両方の SKU の Application Gateway と Web アプリケーション ファイアウォールの課金プロセスについて説明します
services: application-gateway
author: azhar2005
ms.service: application-gateway
ms.topic: conceptual
ms.custom: references_regions
ms.date: 09/01/2020
ms.author: azhussai
ms.openlocfilehash: 8c6afc21ce2dd4ba08a29d2a1c19e680b838c9ee
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2021
ms.locfileid: "121737238"
---
# <a name="understanding-pricing-for-azure-application-gateway-and-web-application-firewall"></a>Azure Application Gateway と Web アプリケーション ファイアウォールの価格について

>[!NOTE]
>この記事に記載した価格は例であり、説明のためにのみ示されています。 ご自身のリージョンに応じた価格の情報については、[価格のページ](https://azure.microsoft.com/pricing/details/application-gateway/)をご覧ください。

Azure Application Gateway はレイヤー 7 の負荷分散ソリューションであり、Azure でのスケーラブルで可用性の高い、セキュリティで保護された Web アプリケーションのデリバリーを実現します。

Application Gateway に関連する事前コストや解約手数料は発生しません。
事前にプロビジョニングし、利用したリソースに対して、実際の時間単位の消費量に基づいて課金されるだけです。 Application Gateway に関連するコストは、固定コストと変動コストという 2 つのコンポーネントに分類されます。 コンポーネントごとの実際のコストは、利用されている SKU によって異なります。

この記事では、各 SKU に関連するコストについて説明します。このドキュメントを利用して、Azure Application Gateway に関連するコストの計画と管理を行うことをお勧めします。

## <a name="v1-skus"></a>V1 の SKU

Standard Application Gateway と WAF V1 SKU は、以下を組み合わせたものとして課金されます。

* [固定コスト]

    特定の種類の Application Gateway と WAF がプロビジョニングされ、要求を処理するために実行されている時間に基づくコスト。
    固定コスト コンポーネントは、次の要因を考慮しています。
    * サービス レベル - Standard Application Gateway または WAF
    * サイズ - Small、Medium、Large
    * インスタンス数 - デプロイされるインスタンスの数

    N 個のインスタンスがある場合、関連するコストは、特定のサービス レベル (Standard と WAF) とサイズ (Small、Medium、Large) を組み合わせた 1 つのインスタンスのコストに、N を掛けたものになります。

* 変動コスト

    Application Gateway と WAF による処理済みデータの量に基づくコスト。 Application Gateway によって処理された要求と応答の両方のバイト数が課金対象と見なされます。

合計コスト = 固定コスト + 変動コスト

### <a name="standard-application-gateway"></a>Standard Application Gateway

固定コストと変動コストは Application Gateway の種類に応じて計算されます。
次の表は、米国東部の価格のスナップショットに基づく価格例を示したもので、説明のみを目的としています。

#### <a name="fixed-cost-east-us-region-pricing"></a>固定コスト (米国東部リージョンの価格)

|              Application Gateway の種類             |  コスト ($/時間)  |
| ------------------------------------------------- | ---------------|
|                     小                         |    $0.025      |
|                     Medium                        |    $0.07       |
|                     大                         |    $0.32       |

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

#### <a name="variable-cost-east-us-region-pricing"></a>変動コスト (米国東部リージョンの価格)

|              処理済みのデータ             |  Small ($/GB)  |  Medium ($/GB) |  Large ($/GB) |
| --------------------------------------- | -------------- | -------------- | ------------- |
| 最初の 10 TB/月                       |     $0.008     |      Free      |     Free      |
| 次の 30 TB (10–40 TB)/月             |     $0.008     |     $0.007     |     Free      |
| 40 TB 超/月                        |     $0.008     |     $0.007     |     $0.0035   |

リージョンに応じた詳しい価格の情報については、[価格のページ](https://azure.microsoft.com/pricing/details/application-gateway/)を参照してください。

### <a name="waf-v1"></a>WAF V1

固定コストと変動コストは、プロビジョニングされた Application Gateway の種類に従って計算されます。

次の表は、米国東部の価格のスナップショットに基づく価格例を示したもので、説明のみを目的としています。

#### <a name="fixed-cost-east-us-region-pricing"></a>固定コスト (米国東部リージョンの価格)

|              Application Gateway の種類             |  コスト ($/時間)  |
| ------------------------------------------------- | ---------------|
|                     小                         |       NA       |
|                     Medium                        |     $0.126     |
|                     大                         |     $0.448     |

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

#### <a name="variable-cost-east-us-region-pricing"></a>変動コスト (米国東部リージョンの価格)

|              処理済みのデータ             |  Small ($/GB)  |  Medium ($/GB) |  Large ($/GB) |
| --------------------------------------- | -------------- | -------------- | ------------- |
| 最初の 10 TB/月                       |     $0.008     |      Free      |     Free      |
| 次の 30 TB (10–40 TB)/月             |     $0.008     |     $0.007     |     Free      |
| 40 TB 超/月                        |     $0.008     |     $0.007     |     $0.0035   |

リージョンに応じた詳しい価格の情報については、[価格のページ](https://azure.microsoft.com/pricing/details/application-gateway/)を参照してください。

> [!NOTE]
> 送信データ転送 - Application Gateway から Azure データセンター外に送られるデータは、標準の[データ転送料金](https://azure.microsoft.com/pricing/details/bandwidth/)で課金されます。

### <a name="example-1-a--standard-application-gateway-with-1-instance-count"></a>例 1 (a) – インスタンス数 1 の Standard Application Gateway

インスタンス数が 1 で、1 か月に 500 GB を処理する Medium タイプの Standard Application Gateway をプロビジョニングしたとします。 前述の価格を使用した Application Gateway のコストは、次のように計算されます。

固定価格 = $0.07 * 730 (時間) = $51.1 月額料金の見積もりは、1 か月の使用時間を 730 時間として算出しています。

変動コスト = 無料 (Medium レベルでは 1 か月に処理される最初の 10 TB にはコストはかかりません) 合計コスト = $51.1 + 0 = $51.1 

> [!NOTE]
> 高可用性のシナリオをサポートするには、V1 SKU に対して少なくとも 2 つのインスタンスをセットアップする必要があります。 「[Application Gateway の SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/)」を参照してください。

### <a name="example-1-b--standard-application-gateway-with--1-instance-count"></a>例 1 (b) – インスタンス数が 1 より多い Standard Application Gateway

たとえば、インスタンス数が 5 で、1 か月に 500 GB を処理するMedium タイプの Standard Application Gateway をプロビジョニングしたとします。 前述の価格を使用した Application Gateway のコストは、次のように計算されます。

固定価格 = 5 (インスタンス数) * $0.07 * 730 (時間) = $255.5 月額料金の見積もりは、1 か月の使用時間を 730 時間として算出しています。

変動コスト = 無料 (Medium レベルでは 1 か月に処理される最初の 10 TB にはコストはかかりません) 合計コスト = $255.5 + 0 = $255.5 

### <a name="example-2--waf-application-gateway"></a>例 2 – WAF Application Gateway

月の前半の 15 日間を対象に、Small タイプの Standard Application Gateway と、Large タイプの WAF Application Gateway をプロビジョニングしたとします。 Small Application Gateway はアクティブになっている期間に 15 TB を処理し、Large WAF Application Gateway はアクティブになっている期間に 100 TB を処理します。 前述の価格を使用した Application Gateway のコストは、次のように計算されます。 

###### <a name="small-instance-standard-application-gateway"></a>Small インスタンスの Standard Application Gateway

24 時間 * 15 日 = 360 時間

固定価格 = $0.025 * 360 (時間) = $9

変動コスト = 10 * 1000 * $0.008/GB + 5 * 1000 * $0.008/GB = $120

合計コスト = $9 + $120 = $129

###### <a name="large-instance-waf-application-gateway"></a>Large インスタンスの WAF Application Gateway
24 時間 * 15 日 = 360 時間

固定価格 = $0.448 * 360 (時間) = $161.28

変動コスト = 60 * 1000 * $0.0035/GB = $210 (Large レベルでは 1 か月あたり最初の 40 TB にはコストはかかりません)

合計コスト = $161.28 + $210 = $371.28

## <a name="v2-skus"></a>V2 の SKU  

Application Gateway V2 と WAF V2 の SKU では、自動スケーリングがサポートされ、高可用性が既定で保証されます。

### <a name="key-terms"></a>主な用語

##### <a name="capacity-unit"></a>容量ユニット 
容量ユニットは、複数のパラメーターにわたる Application Gateway の容量使用率のメジャーです。

1 容量ユニットは、次のパラメーターで構成されます。
* 2500 個の永続的な接続
* 2.22 Mbps のスループット
* 1 コンピューティング ユニット

これらのパラメーターのいずれか 1 つを超過した場合、他の 2 つのパラメーターがこの 1 つの容量ユニットの制限を超えていなくても、別に n 個の容量ユニットが必要です。
上記の 3 つのうち最も使用率の高いパラメーターが、容量ユニットの計算のために内部的に使用され、されに対して課金が行われます。

##### <a name="compute-unit"></a>コンピューティング ユニット
コンピューティング ユニットは、使用されたコンピューティングの容量のメジャーです。 コンピューティング ユニット消費量に影響する要因は、1 秒あたりの TLS 接続数、URL 書き換え計算、WAF ルールの処理です。 コンピューティング ユニットが処理できる要求の数は、TLS 証明書のキーのサイズ、キー交換アルゴリズム、ヘッダーの書き換え、WAF での着信要求のサイズなど、さまざまな条件に依存します。

コンピューティング ユニットのガイダンス:
* Standard_v2 - 各コンピューティング ユニットは、RSA 2048 ビット キーの TLS 証明書で 1 秒あたり約 50 の接続に対応します。

* WAF_v2 - 各コンピューティング ユニットは、70% が 2 KB 未満の GET/POST 要求、残りがそれ以上の要求の混合で、1 秒あたり約 10 件の同時要求をサポートできます。 WAF のパフォーマンスは、現在、応答のサイズによる影響を受けません。

##### <a name="instance-count"></a>Instance Count 
Application Gateway V2 SKU のリソースの事前プロビジョニングは、インスタンス数に基づいて定義されます。 各インスタンスでは、処理能力の観点から最低 10 容量ユニットが保証されます。 同じインスタンスが、容量ユニットのパラメーターに応じて、トラフィック パターンごとに 10 を超える容量ユニットをサポートする可能性があります。

自動スケーリング (最小または最大) のために手動で定義したスケールと制限は、インスタンス数に基づいて設定されます。 インスタンス数に対して手動で設定したスケールと自動スケーリング構成の最小インスタンス数によって、インスタンスあたり 10 容量ユニットが予約されます。 これらの予約 CU は、実際のリソース消費量に関係なく、Application Gateway がアクティブである限り、課金されます。 実際の消費量がインスタンスあたり 10 容量ユニットのしきい値を超えた場合、追加の容量ユニットは変動コンポーネントで課金されます。

V2 SKU は消費量に基づいて課金され、次の 2 つの部分で構成されます。

* 固定コスト

    Application Gateway V2 と WAF V2 がプロビジョニングされた、要求を処理するために利用可能になっている時間に基づくコスト。 これにより、自動スケーリングの一部として最小インスタンス数に 0 を指定することによって予約されているインスタンスが 0 の場合でも、高可用性が保証されます。 
    
    固定コストには、Application Gateway に接続されているパブリック IP に関連するコストも含まれます。

    任意の時点で実行されているインスタンスの数は、V2 SKU の固定コストに関する要因とは見なされません。 同じ Azure リージョン内で実行されているインスタンスの数に関係なく、Standard_V2 (または WAF_V2) を実行するための 1 時間あたりの固定コストは同じになります。

* 容量ユニットのコスト 

    受信要求を処理するために予約されているか、必要に応じて追加で利用される容量ユニットの数に基づくコスト。  消費量ベースのコストは、時間単位で計算されます。

合計コスト = 固定コスト + 容量ユニットのコスト

> [!NOTE]
> 1 時間未満の時間は 1 時間として課金されます。

次の表は、米国東部の価格のスナップショットに基づく価格例を示したもので、説明のみを目的としています。

#### <a name="fixed-costs-east-us-region-pricing"></a>固定コスト (米国東部リージョンの価格)

|              V2 SKU             |  コスト ($/時間)  |
| ------------------------------- | ---------------|
|            Standard_V2          |     $0.246     |
|              WAF_V2             |     $0.443     |

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

#### <a name="variable-costs-east-us-region-pricing"></a>変動コスト (米国東部リージョンの価格)

|        容量ユニット         |  Standard_V2 ($/時間)  |  WAF_V2 ($/時間) |
| ---------------------------- | -------------------- | -------------- |
|             1 CU             |        $0.008        |     $0.0144    |

リージョンに応じた詳しい価格の情報については、[価格のページ](https://azure.microsoft.com/pricing/details/application-gateway/)を参照してください。

> [!NOTE]
> 送信データ転送 - Application Gateway から Azure データセンター外に送られるデータは、標準の[データ転送料金](https://azure.microsoft.com/pricing/details/bandwidth/)で課金されます。

### <a name="example-1-a--manual-scaling"></a>例 1 (a) – 手動スケーリング 
手動スケーリングを 1 か月全体で 8 インスタンスに設定した Standard_V2 Application Gateway をプロビジョニングしたとします。 この期間中、平均 88.8 Mbps のデータ転送を受信します。

前述の価格を使用した Application Gateway のコストは、次のように計算されます。

1 CU で 2.22 Mbps のスループットを処理できます。

88.8 Mbps を処理するために必要な CU = 88.8 / 2.22 = 40 CU 

事前プロビジョニング済み CU = 8 (インスタンス数) * 10 = 80 

80 (予約容量) > 40 (必要な容量) であるため、追加の CU は必要ありません。 

固定価格 = $0.246 * 730 (時間) = $179.58

変動コスト = $0.008 * 8 (インスタンス ユニット) * 10 (容量ユニット) * 730 (時間) = $467.2

合計コスト = $179.58 + $467.2 = $646.78

![手動スケーリング 1 の図。](./media/pricing/manual-scale-1.png)

### <a name="example-1-b--manual-scaling-with-traffic-going-beyond-provisioned-capacity"></a>例 1 (b) – トラフィックがプロビジョニングされた容量を超える手動スケーリング

手動スケーリングを 1 か月全体で 3 インスタンスに設定した Standard_V2 Application Gateway をプロビジョニングしたとします。 この期間中、平均 88.8 Mbps のデータ転送を受信します。

前述の価格を使用した Application Gateway のコストは、次のように計算されます。

1 CU で 2.22 Mbps のスループットを処理できます。

88.8 Mbps を処理するために必要な CU = 88.8 / 2.22 = 40 

事前プロビジョニング済み CU = 3 (インスタンス数) * 10 = 30 

40 (必要な容量) > 30 (予約容量) であるため、追加の CU が必要です。
利用する追加の CU の数は、各インスタンスで使用可能な空き容量によって異なります。

追加の 10 CU に相当する処理容量が 3 つの予約インスタンスで利用可能な場合。

固定価格 = $0.246 * 730 (時間) = $179.58

変動コスト = $0.008 * (3 (インスタンス ユニット) * 10 (容量ユニット) + 10 (追加の容量ユニット)) * 730 (時間) = $233.6

合計コスト = $179.58 + $233.6 = $413.18

ただし、たとえば追加のわずか 7 CU に相当する処理容量が 3 つの予約インスタンスで利用可能な場合。
このシナリオでは、Application Gateway リソースのスケーリングが不十分であり、待機時間の増加や要求のドロップにつながる可能性があります。

固定価格 = $0.246 * 730 (時間) = $179.58

変動コスト = $0.008 * (3 (インスタンス ユニット) * 10 (容量ユニット) + 7 (追加の容量ユニット)) * 730 (時間) = $216.08

合計コスト = $179.58 + $216.08 = $395.66


![手動スケーリング 2 の図。](./media/pricing/manual-scale-2.png)

> [!NOTE]
> 手動スケーリングの場合、予約インスタンスの最大処理容量を超える追加の要求によって、アプリケーションの可用性に影響が出る可能性があります。 負荷が高い状況においては、受信要求の構成と種類に応じて、予約インスタンスで 10 個を超える処理容量ユニットを提供できる場合があります。 ただし、トラフィック要件に従ってインスタンスの数をプロビジョニングすることをお勧めします。

### <a name="example-2--waf_v2-instance-with-autoscaling"></a>例 2 – 自動スケーリングを使用する WAF_V2 インスタンス

自動スケーリングを有効にし、1 か月全体の最小インスタンス数を 6 に設定した WAF_V2 をプロビジョニングしたとします。 要求負荷によって WAF インスタンスのスケールアウトが発生し、65 容量ユニット (スケールアウト分が 5 容量ユニット、60 ユニットは予約済み) を 1 か月にわたって利用ました。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

固定価格 = $0.443 * 730 (時間) = $323.39

変動コスト = $0.0144 * 65 (容量ユニット) * 730 (時間) = $683.28

合計コスト = $323.39 + $683.28 = $1006.67

![自動スケーリング 2 の図。](./media/pricing/auto-scale-1.png)

> [!NOTE]
> Application Gateway で観測される実際のトラフィックは、このような一定のトラフィック パターンではない可能性があります。Application Gateway で観測される負荷は、実際の使用状況に応じて変動します。

### <a name="example-3-a--waf_v2-instance-with-autoscaling-and-0-min-scale-config"></a>例 3 (a) – 自動スケーリングで最小スケール構成 0 の WAF_V2 インスタンス

自動スケーリングを有効にし、1 か月全体の最小インスタンス数を 0 に設定した WAF_V2 をプロビジョニングしたとします。 WAF に対する要求の負荷は最小限ですが、1 か月全体にわたり毎時間一貫して存在します。 負荷は 1 容量ユニットの容量を下回っています。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

固定価格 = $0.443 * 730 (時間) = $323.39

変動コスト = $0.0144 * 1 (容量ユニット) * 730 (時間) = $10.512

合計コスト = $323.39 + $10.512 = $333.902

### <a name="example-3-b--waf_v2-instance-with-autoscaling-with-0-min-instance-count"></a>例 3 (b) – 自動スケーリングの最小インスタンス数 0 の WAF_V2 インスタンス

自動スケーリングを有効にし、1 か月全体の最小インスタンス数を 0 に設定した WAF_V2 をプロビジョニングしたとします。 ただし、1 か月全体にわたって WAF インスタンスに送信されるトラフィックは 0 です。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

固定価格 = $0.443 * 730 (時間) = $323.39

変動コスト = $0.0144 * 0 (容量ユニット) * 730 (時間) = $0

合計コスト = $323.39 + $0 = $323.39

### <a name="example-3-c--waf_v2-instance-with-manual-scaling-set-to-1-instance"></a>例 3 (C) – 手動スケーリングを 1 インスタンスに設定した WAF_V2 インスタンス

WAF_V2 をプロビジョニングし、1 か月全体の最小許容値を 1 インスタンスとして手動スケーリングに設定したとします。 ただし、1 か月全体にわたって WAF に送信されるトラフィックは 0 です。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

固定価格 = $0.443 * 730 (時間) = $323.39

変動コスト = $0.0144  * 1 (インスタンス数 count) * 10 (容量ユニット) * 730 (時間) = $105.12

合計コスト = $323.39 + $105.12 = $428.51

### <a name="example-4--waf_v2-with-autoscaling-capacity-unit-calculations"></a>例 4 – 自動スケーリングを使用した WAF_V2 での容量ユニットの計算

自動スケーリングを有効にし、1 か月全体の最小インスタンス数を 0 に設定した WAF_V2 をプロビジョニングしたとします。 この間に、毎秒 25 個の新規 TLS 接続と平均 8.88 Mbps のデータ転送を受信します。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

月額料金の見積もりは 1 か月の使用時間を 730 時間として算出しています。

固定価格 = $0.443 * 730 (時間) = $323.39

変動コスト = $0.0144 * 730 (時間) * {Max (25/50, 8.88/2.22)} = $23.36 (8.88 Mbps を処理するのに必要な 4 容量ユニット)

合計コスト = $323.39 + $23.36 = $346.75

### <a name="example-5-a--standard_v2-with-autoscaling-time-based-calculations"></a>例 5 (a) – 自動スケーリングを使用した Standard_V2 での時間ベースの計算

自動スケーリングを有効し、最小インスタンス数を 0 に設定した standard_V2 をプロビジョニングし、このアプリケーション ゲートウェイを 2 時間アクティブにしたとします。
最初の 1 時間に、10 容量ユニットで処理できるトラフィックを受信し、2 時間目に、負荷を処理するために 20 容量ユニットが必要なトラフィックを受信します。
前述の価格を使用した Application Gateway のコストは、次のように計算されます。

固定価格 = $0.246 * 2 (時間) = $0.492

変動コスト = $0.008 * 10 (容量ユニット) * 1 (時間) + $0.008 * 20 (容量ユニット) * 1 (時間) = $0.24

合計コスト = $0.492 + $0.24 = $0.732


## <a name="monitoring-billed-usage"></a>課金対象の使用量の監視

**[監視]** セクションに、さまざまなパラメーター (コンピューティング ユニット、スループット、永続的な接続) の消費量と、Application Gateway メトリックの一部として利用されている容量ユニットを表示できます。

![メトリック セクションの図。](./media/pricing/metrics-1.png)

### <a name="useful-metrics-for-cost-estimation"></a>コスト見積もりに役立つメトリック

* 現在の容量ユニット数

    3 つのパラメーター (現在の接続、スループット、およびコンピューティング ユニット) 全体でトラフィックの負荷を分散するために消費された容量ユニットの数

* 固定請求可能容量ユニット

    Application Gateway 構成の最小インスタンス数設定 (1 つのインスタンスが 10 個の容量ユニットに変換される) に従ってプロビジョニングが維持される容量ユニットの最小数。

* 推定課金対象容量ユニット数

    推定課金対象容量ユニット数は、課金が推定される使用中の容量ユニットの数を示します。 これは現在の容量ユニット (トラフィックの負荷を分散するために必要な容量ユニット) と固定請求可能容量ユニット (プロビジョニングが維持される最小容量ユニット) のうち大きい方の値として計算されます。

スループット、現在の接続数、コンピューティング ユニットなど、さらに多くのメトリックを使用して、ボトルネックを把握し、必要な容量ユニットの数を見積もることもできます。 詳細については、「[Application Gateway メトリック](application-gateway-metrics.md)」を参照してください。

#### <a name="example---estimating-capacity-units-being-utilized"></a>例 - 利用されている容量ユニットの推定

**観測されたメトリック:**

* コンピューティング ユニット = 17.38
* スループット = 1.37M バイト/秒 - 10.96 Mbps
* 現在の接続数 = 123.08k
* 計算された容量ユニット = max(17.38, 10.96/2.22, 123.08k/2500) = 49.232

メトリックで観測された容量ユニット = 49.23

## <a name="next-steps"></a>次のステップ

Azure Application Gateway での課金のしくみの詳細については、以下の記事を参照してください。

* [Application Gateway の価格のページ](https://azure.microsoft.com/pricing/details/application-gateway/)
* [Azure Application Gateway 料金計算ツール](https://azure.microsoft.com/pricing/calculator/?service=application-gateway)
