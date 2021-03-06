---
title: パス式 (XQuery) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- path expressions [XQuery]
- expressions [XQuery], path
ms.assetid: b93fa36c-bf69-46b9-b137-f597d66fd0c0
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: fdc9279d821402e0011b10ef4c1843a721178a9f
ms.sourcegitcommit: 0f7cf9b7ab23df15624d27c129ab3a539e8b6457
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2018
ms.locfileid: "51291428"
---
# <a name="path-expressions-xquery"></a>パス式 (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery パス式は、ドキュメント内の要素、属性、テキストなど、ノードの位置を特定します。 パス式の結果は常に、結果シーケンス内でノードが重複することなく、ドキュメント順で生成されます。 パスの指定には、省略された構文と省略しない構文のいずれも使用することができます。 以下の説明では、主に省略しない構文を取り上げます。 省略された構文についてはこのトピックの最後で説明します。  
  
> [!NOTE]  
>  に対してこのトピックのサンプル クエリを指定するため、 **xml**種類の列、 **CatalogDescription**と**指示**の**ProductModel**テーブルが理解しておく内容とこれらの列に格納された XML ドキュメントの構造を使用します。  
  
 パス式には、相対パスまたは絶対パスのいずれを使用することもできます。 ここでは、その両方について説明します。  
  
-   相対パス式は、1 つまたは 2 つのスラッシュ記号 (/ または //) で区切られた 1 つ以上のステップで構成されます。 たとえば、`child::Features` は、コンテキスト ノードの子ノードのみを `Child` で参照している相対パス式です。 コンテキスト ノードとは、現在処理中のノードです。 式を取得、\<機能 > コンテキスト ノードの子要素ノード。  
  
-   絶対パス式では、1 つまたは 2 つのスラッシュ記号 (/ または //) が先頭にあり、その後に必要に応じて相対パスが続きます。 たとえば、式 `/child::ProductDescription` の先頭のスラッシュ記号は、この式が絶対パス式であることを示しています。 式の先頭のスラッシュ記号は、コンテキスト ノードのドキュメントのルート ノードを返すため、すべてを返します、式、 \<ProductDescription > ドキュメントのルートの子要素ノード。  
  
     絶対パス式の先頭が 1 つのスラッシュ記号の場合、その後に相対パスを指定することもしないことも可能です。 スラッシュ記号 (/) 1 つのみを指定した場合、この式はコンテキスト ノードのルート ノードを返します。 XML データ型の場合、これはドキュメント ノードになります。  
  
 パス式は、通常、ステップで構成されます。 たとえば、絶対パス式`/child::ProductDescription/child::Summary`、スラッシュ記号で区切られた 2 つの手順が含まれています。  
  
-   最初の手順の取得、 \<ProductDescription > ドキュメントのルートの子要素ノード。  
  
-   2 番目の手順の取得、\<概要 > 要素ノードの取得された各子\<ProductDescription > 要素ノードは、コンテキスト ノードになります。  
  
 パス式のステップには、軸ステップまたは汎用ステップを使用できます。  
  
## <a name="axis-step"></a>軸ステップ  
 パス式の軸ステップは、次の部分で構成されます。  
  
 [axis](../xquery/path-expressions-specifying-axis.md)  
 移動の方向を定義します。 パス式の軸ステップはコンテキスト ノードから始まり、軸ステップにより指定されている方向で到達可能なノードまで移動します。  
  
 [ノード テスト](../xquery/path-expressions-specifying-node-test.md)  
 選択するノードの型またはノード名を指定します。  
  
 0 個以上の述語 (省略可)。  
 ノードにフィルターを適用し、一部を選択して残りを破棄します。  
  
 次の例を使用して、 **axisstep**パス式で。  
  
-   絶対パス式 `/child::ProductDescription` には、ステップが 1 つしかありません。 これは、軸 (`child`) とノード テスト (`ProductDescription`) を指定しています。  
  
-   相対パス式 `child::ProductDescription/child::Features` には、1 つのスラッシュ記号で区切られた 2 つのステップがあります。 どちらのステップも、child を軸に指定しています。 ProductDescription と Features はノード テストです。  
  
-   相対パス式では、 `child::root/child::Location[attribute::LocationID=10]`、スラッシュ記号で区切られた 2 つの手順が含まれています。 最初のステップは、軸 (`child`) とノード テスト (`root`) を指定しています。 2 番目のステップは、軸ステップの 3 種類のコンポーネントをすべて指定しています。つまり、軸 (child)、ノード テスト (`Location`)、および述語 (`[attribute::LocationID=10]`) です。  
  
 軸ステップの詳細については、コンポーネントは、次を参照してください[パス式のステップで軸を指定する](../xquery/path-expressions-specifying-axis.md)、[パス式のステップでノード テストを指定する](../xquery/path-expressions-specifying-node-test.md)、および[で述語の指定、。パス式のステップ](../xquery/path-expressions-specifying-predicates.md)します。  
  
## <a name="general-step"></a>汎用ステップ  
 汎用ステップは、評価結果がノードのシーケンスになる必要のある式です。  
  
 SQL Server の XQuery 実装は、パス式の最初のステップとして汎用ステップをサポートしています。 汎用ステップを使用するパス式の例を次に示します。  
  
```  
(/a, /b)/c  
id(/a/b)  
```  
  
 Id 関数」を参照の詳細については[id 関数&#40;XQuery&#41;](../xquery/functions-on-sequences-id.md)します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [パス式のステップで軸を指定します。](../xquery/path-expressions-specifying-axis.md)  
 パス式での軸ステップの使い方について説明します。  
  
 [パス式のステップでノード テストの指定](../xquery/path-expressions-specifying-node-test.md)  
 パス式でのノード テストの使い方について説明します。  
  
 [パス式のステップで述語の指定](../xquery/path-expressions-specifying-predicates.md)  
 パス式での述語の使い方について説明します。  
  
 [パス式での省略構文の使用](../xquery/path-expressions-using-abbreviated-syntax.md)  
 パス式での省略構文の使い方について説明します。  
  
  
