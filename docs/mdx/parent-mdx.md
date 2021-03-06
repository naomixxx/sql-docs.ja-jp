---
title: 親 (MDX) |Microsoft ドキュメント
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 16de50e05bf139d590f90630e338fdb7cbb428e5
ms.sourcegitcommit: 97bef3f248abce57422f15530c1685f91392b494
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34742761"
---
# <a name="parent-mdx"></a>Parent (MDX)


  メンバーの親メンバーを返します。  
  
## <a name="syntax"></a>構文  
  
```  
  
Member_Expression.Parent   
```  
  
## <a name="arguments"></a>引数  
 *メンバー式*  
 メンバーを 1 つ返す有効な多次元式 (MDX) 式です。  
  
## <a name="remarks"></a>コメント  
 **親**関数が、指定されたメンバーの親メンバーを返します。  
  
## <a name="examples"></a>使用例  
 次の 2 つの例では、July 1, 2001 メンバーの親メンバーを返しています。 この例では、Date 属性階層のコンテキストでこのメンバーを指定して、All Periods メンバーを返します。  
  
```  
SELECT [Date].[Date].[July 1, 2001].Parent ON 0  
FROM [Adventure Works]  
```  
  
 次の例では、Calendar 階層のコンテキストで July 1, 2001 メンバーを指定します。  
  
```  
SELECT [Date].[Calendar].[July 1, 2001].Parent ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>参照  
 [MDX 関数リファレンス&#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
