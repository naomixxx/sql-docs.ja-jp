---
title: 用語抽出変換エディター (詳細 タブ) |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- integration-services
ms.topic: conceptual
f1_keywords:
- sql12.dts.designer.termextraction.advanced.f1
helpviewer_keywords:
- Term Extraction Transformation Editor
ms.assetid: 87118281-6e3c-499e-bac4-fa4c24bb12c6
author: douglaslms
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ce44e2709013a6d56d47ad30c90ecf5d92825503
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48185332"
---
# <a name="term-extraction-transformation-editor-advanced-tab"></a>[用語抽出変換エディター] ([詳細設定] タブ)
  **[用語抽出変換エディター]** ダイアログ ボックスの **[詳細設定]** タブを使用すると、頻度、長さ、語または句の抽出の有無など、抽出に関するプロパティを指定できます。  
  
 用語抽出変換の詳細については、「 [Term Extraction Transformation](data-flow/transformations/term-extraction-transformation.md)」を参照してください。  
  
## <a name="options"></a>および  
 **[名詞]**  
 変換によって個別の名詞のみを抽出するように指定します。  
  
 **[名詞句]**  
 変換によって個別の名詞句のみを抽出するように指定します。  
  
 **[名詞と名詞句]**  
 変換によって名詞と名詞句を両方とも抽出するように指定します。  
  
 **頻度**  
 スコアが用語の頻度であることを指定します。  
  
 **[TFIDF]**  
 スコアが用語の TFIDF 値であることを指定します。 TFIDF スコアは、Term Frequency と Inverse Document Frequency の積です。"用語 T の TFIDF = (T の頻度) * log( (入力の行数) / (T を含む行数) )" として定義されます。  
  
 **[頻度のしきい値]**  
 語または句を抽出する前の語または句の出現回数を指定します。 既定値は 2 です。  
  
 **[用語の最大長]**  
 句の最大長を語数で指定します。 このオプションは、名詞句のみに影響を与えます。 既定値は 12 です。  
  
 **[用語抽出で大文字と小文字を区別する]**  
 抽出で大文字と小文字を区別するかどうかを指定します。 既定値は `False` です。  
  
 **エラー出力の構成**  
 [[エラー出力の構成]](../../2014/integration-services/configure-error-output.md) ダイアログ ボックスは、エラーが発生した行に対するエラー処理を指定するために使用します。  
  
## <a name="see-also"></a>参照  
 [Integration Services のエラーおよびメッセージのリファレンス](../../2014/integration-services/integration-services-error-and-message-reference.md)   
 [用語抽出変換エディター&#40;用語抽出 タブ&#41;](../../2014/integration-services/term-extraction-transformation-editor-term-extraction-tab.md)   
 [用語抽出変換エディター &#40;[除外] タブ&#41;](../../2014/integration-services/term-extraction-transformation-editor-exclusion-tab.md)   
 [用語参照変換](data-flow/transformations/lookup-transformation.md)  
  
  
