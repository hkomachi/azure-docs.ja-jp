---
title: 'Partition and Sample (パーティションとサンプル): コンポーネント リファレンス'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 内の Partition and Sample (パーティションとサンプル) コンポーネントを使用して、データセットでサンプリングを実行したり、データセットからパーティションを作成したりする方法を学習します。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: fcab83e37d28383bc4faf64a231b9e9254e67790
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2021
ms.locfileid: "131565750"
---
# <a name="partition-and-sample-component"></a>Partition and Sample (パーティションとサンプル) コンポーネント

この記事では Azure Machine Learning デザイナーのコンポーネントについて説明します。

パーティションとサンプル コンポーネントを使用して、データセットでサンプリングを実行するか、またはデータセットからパーティションを作成します。

サンプリングは、値の同じ比率を維持しながら、データセットのサイズを小さくできるため、機械学習で重要なツールです。 このコンポーネントでは、機械学習で重要な以下のいくつかの関連タスクがサポートされます。 

- 同じサイズの複数のサブセクションにデータを分割する。 

  クロス検証のために、またはランダム グループにケースを割り当てるために、パーティションを使用する可能性があります。

- グループにデータを分離してから、特定のグループからのデータを操作する。 

  さまざまなグループにランダムにケースを割り当てた後、1 つのグループのみに関連付けられているフィーチャーを変更する必要がある場合があります。

- サンプリングを行う。 

  データの割合を抽出し、ランダム サンプリングを適用するか、データセットの負荷分散で使用する列を選択し、その値に対して階層サンプリングを行うことができます。

- テスト用のより小さなデータセットを作成する。 

  大量のデータがある場合は、パイプラインの設定中は最初の *n* 行のみを使用し、モデルを構築するときに完全なデータセットを使用するように切り替えることができます。 サンプリングを使って、開発で使用するためのより小さなデータセットを作成することもできます。

## <a name="configure-the-component"></a>コンポーネントを構成する

このコンポーネントでは、データをパーティションに分割するために、またはサンプリングのために、次の方法がサポートされます。 最初に方法を選択し、次にその方法で必要な追加のオプションを設定します。

- Head
- サンプリング
- [Assign to folds]\(フォールドに割り当てる\)
- [Pick fold]\(フォールドを選択する\)

### <a name="get-top-n-rows-from-a-dataset"></a>[Get TOP N rows from a dataset]\(データセットから上位 N 行を取得する\)

このモードを使用して最初の *n* 行のみを取得します。 このオプションは、少ない数の行でパイプラインをテストするときに、いかなる方法でもデータの負荷を分散したりサンプリングしたりする必要がない場合に便利です。

1. **Partition and Sample (パーティションとサンプル)** コンポーネントをインターフェイスでパイプラインに追加して、データセットを接続します。  

1. **[Partition or sample mode]\(パーティションまたはサンプル モード\)** : このオプションを **[ヘッド]** に設定します。

1. **[Number of rows to select]\(選択する行の数\)** : 返される行の数を入力します。

   行の数は、負ではない整数にする必要があります。 選択した行の数が、データセット内の行の数よりも大きい場合は、データセット全体が返されます。

1. パイプラインを送信します。

コンポーネントにより、指定された数の行のみを含む単一データセットが出力されます。 行は常に、データセットの一番上から読み取られます。

### <a name="create-a-sample-of-data"></a>[Create a sample of data]\(データのサンプルを作成する\)

このオプションでは、簡単なランダム サンプリングまたは階層ランダム サンプリングがサポートされます。 これは、テスト用のより小さい代表的なサンプル データセットを作成する場合に便利です。

1. **Partition and Sample (パーティションとサンプル)** コンポーネントをパイプラインに追加して、データセットを接続します。

1. **[Partition or sample mode]\(パーティションまたはサンプル モード\)** : このオプションを **[サンプル]** に設定します。

1. **[Rate of sampling]\(サンプリング率\)** : 0 から 1 の値を入力します。 この値では、出力データセットに含める必要があるソース データセットからの行の割合を指定します。

   たとえば、元のデータセットの半分のみが必要な場合は、「`0.5`」と入力して、サンプリング率が 50% である必要があることを示します。

   入力データセットの行はシャッフルされ、指定された比率に従って、選択的に出力データセットに配置されます。

1. **[Random seed for sampling]\(サンプリングのためのランダム シード\)** : 必要に応じて、シード値として使用する整数を入力します。

   このオプションは、行を常に同じように分割する場合に重要となります。 既定値が **0** の場合、システム クロックに基づいて開始シードが生成されることを意味します。 この値は、パイプラインを実行するたびに、若干異なる結果となる可能性があります。

1. **[Stratified split for sampling]\(サンプリングのための階層分割\)** : サンプリングする前にいくつかのキー列でデータセット内の行を均等に分割することが重要である場合は、このオプションを選択します。

   **[Stratification key column for sampling]\(サンプリングのための階層キー列\)** では、データセットを分割するときに使用する単一の *階層列* を選択します。 その後、データセット内の行は次のように分割されます。

   1. すべての入力行が、指定された階層列の値でグループ化 (階層化) されます。

   1. 各グループ内で行がシャッフルされます。

   1. 各グループは、指定された比率を満たすために、出力データセットに選択的に追加されます。


1. パイプラインを送信します。

   このオプションでは、データの代表的なサンプリングを含む単一データセットがコンポーネントによって出力されます。 データセットの残りの、サンプリングされていない部分は出力されません。 

## <a name="split-data-into-partitions"></a>[Split data into partitions]\(データをパーティションに分割する\)

データのサブセットにデータセットを分割する場合は、このオプションを使用します。 このオプションは、クロス検証のためにユーザーが設定した数のフォールドを作成する場合や、行をいくつかのグループに分割する場合にも役立ちます。

1. **Partition and Sample (パーティションとサンプル)** コンポーネントをパイプラインに追加して、データセットを接続します。

1. **[Partition or sample mode]\(パーティションまたはサンプル モード\)** では、**[Assign to Folds]\(フォールドに割り当てる\)** を選択します。

1. **[Use replacement in the partitioning]\(パーティション分割で置換を使用する\)** : 再利用される可能性がある行のプールにサンプリングされた行を戻す場合は、このオプションを選択します。 その結果、同じ行がいくつかのフォールドに割り当てられる可能性があります。

   置換を使用しない場合 (既定のオプション)、再利用される可能性がある行のプールにサンプリングされた行は戻されません。 その結果、各行を 1 つのフォールドのみに割り当てることができます。

1. **[Randomized split]\(ランダム分割\)** : 行をフォールドにランダムに割り当てる場合は、このオプションを選択します。

   このオプションを選択しない場合、ラウンドロビン方式を使用して、フォールドに行が割り当てられます。

1. **[Random seed]\(ランダム シード\)** : 必要に応じて、シード値として使用する整数を入力します。 このオプションは、行を常に同じように分割する場合に重要となります。 それ以外の場合、既定値の **0** が設定され、ランダム開始シードが使用されることになります。

1. **[Specify the partitioner method]\(パーティショナー方法を指定する\)** : これらのオプションを使用して、データを各パーティションにどのように割り当てるかを示します。

   - **[Partition evenly]\(均等にパーティション分割する\)** : 各パーティションに同数の行を配置するには、このオプションを使用します。 出力パーティションの数を指定するには、 **[均等に分割するフォールド数を指定する]** ボックスに整数を入力します。

   - **[Partition with customized proportions]\(カスタマイズされた比率でパーティション分割する\)** : 各パーティションのサイズをコンマ区切りのリストとして指定するには、このオプションを使用します。

     たとえば、3 つのパーティションを作成するとします。 最初のパーティションには、データの 50% が含まれます。 残りの 2 つのパーティションには、それぞれデータの 25% が含まれます。 **[コンマで区切られた割合の一覧]** ボックスに、数値「 **.5, .25, .25**」を入力します。

     すべてのパーティション サイズの合計が、ちょうど 1 になるようにしてください。

     合計が *1 より小さく* なる数値を入力すると、残りの行を保持するために余分なパーティションが作成されます。 たとえば、値として **.2** と **.3** を入力した場合、すべての行の残りの 50% を保持する 3 つ目のパーティションが作成されます。
     
     合計が *1 より大きく* なる数値を入力すると、パイプラインを実行したときにエラーが発生します。

1. **[Stratified split]\(階層分割|)** : 行を分割時に階層化する場合は、このオプションを選択してから、_階層列_ を選びます。

1. パイプラインを送信します。

   このオプションを使用すると、コンポーネントは複数のデータセットを出力します。 データセットは、指定したルールに従ってパーティション分割されます。

### <a name="use-data-from-a-predefined-partition"></a>[Use data from a predefined partition]\(定義済みのパーティションからデータを使用する\)  

このオプションは、データセットを複数のパーティションに分割しており、各パーティションをさらに分析するか、処理するために読み込む場合に使用します。

1. **Partition and Sample (パーティションとサンプル)** コンポーネントをパイプラインに追加します。

1. コンポーネントを、**Partition and Sample (パーティションとサンプル)** の前のインスタンスの出力に接続します。 そのインスタンスで、いくつかのパーティションを生成するために **[Assign to Folds]\(フォールドに割り当てる\)** オプションが使用されている必要があります。

1. **[Partition or sample mode]\(パーティションまたはサンプル モード\)** : **[Pick Fold]\(フォールドを選択する\)** を選択します。

1. **[Specify which fold to be sampled from]\(サンプリング元のフォールドを指定する\)** : インデックスを入力して、使用するパーティションを選択します。 パーティション インデックスは 1 から始まります。 たとえば、データセットを 3 つの部分に分割した場合、パーティションのインデックスは 1、2、3 となります。

   無効なインデックス値を入力すると、"Error 0018: Dataset contains invalid data.\(エラー 0018: データセットに無効なデータが含まれています。\)" というデザイン時エラーが発生します。

   フォールドでデータセットをグループ化するだけでなく、ターゲット フォールドとその他すべてという 2 つのグループにデータセットを分離することができます。 これを行うには、単一フォールドのインデックスを入力してから、 **[選択したフォールドの補数を選択する]** というオプションを選択し、指定されたフォールドのデータ以外のすべてを取得します。

1. 複数のパーティションを操作する場合は、**Partition and Sample (パーティションとサンプル)** コンポーネントのインスタンスをさらに追加して、各パーティションを処理する必要があります。

   たとえば、2 番目の行の **Partition and Sample (パーティションとサンプル)** コンポーネントは、 **[フォールドへの割り当て]** に設定され、3 番目の行のコンポーネントは **[Pick Fold]\(フォールドを選択する\)** に設定されます。   

   ![パーティションとサンプル](./media/module/partition-and-sample.png)

1. パイプラインを送信します。

   このオプションでは、そのフォールドに割り当てられた行のみを含む、単一データセットがコンポーネントによって出力されます。

> [!NOTE]
>  フォールドの指定を直接表示することはできません。 これらはメタデータ内にのみ存在します。

## <a name="next-steps"></a>次のステップ

Azure Machine Learning で[使用できる一連のコンポーネント](component-reference.md)をご覧ください。 