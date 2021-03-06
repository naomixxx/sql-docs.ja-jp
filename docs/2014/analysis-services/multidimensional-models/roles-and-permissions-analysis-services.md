---
title: ロールと権限 (Analysis Services) |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
ms.topic: conceptual
helpviewer_keywords:
- security [Analysis Services], about security
- security [Analysis Services - multidimensional data], about security
ms.assetid: bb885447-868b-4686-853c-8241f63d4370
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: bc36b27bea2c546582cb167046affcc0fbd0d5a6
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48114116"
---
# <a name="roles-and-permissions-analysis-services"></a>ロールと権限 (Analysis Services)
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] には、操作、オブジェクト、およびデータへのアクセスを許可する、ロールベースの承認モデルが用意されています。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] インスタンスまたはデータベースにアクセスするすべてのユーザーは、ロールのコンテキスト内でアクセスする必要があります。  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] システム管理者は、サーバーでの操作への無制限のアクセスが許可される **サーバー管理者ロール** へのメンバーシップの付与を担当します。 このロールの権限は固定されており、カスタマイズできません。 既定では、ローカルの Administrators グループのメンバーは、自動的に Analysis Services システム管理者になります。  
  
 データのクエリまたは処理を行う管理者以外のユーザーには、 **データベース ロール**によってアクセス権が付与されます。 システム管理者もデータベース管理者も、特定のデータベースでの異なるレベルのアクセスに対応するロールを作成し、アクセスを要するすべてのユーザーにメンバーシップを割り当てることができます。 各ロールには、特定のデータベース内のオブジェクトおよび操作にアクセスするための、カスタマイズされた一連の権限があります。 権限を割り当てられるレベルは、データベース、キューブやディメンションなどの内部オブジェクト (パースペクティブを除く)、および行です。  
  
 ロールの作成とメンバーシップの割り当ては、別の操作として行うのが一般的です。 多くの場合、モデル デザイナーは、デザイン フェーズ中にロールを追加します。 この方法で、すべてのロールの定義は、モデルを定義するプロジェクト ファイルに反映されます。 ロールのメンバーシップは、一般的には後でデータベースが実際に運用されるときに設定されます。通常は、データベース管理者が独立した操作として開発、テスト、および実行できるスクリプトを作成して設定します。  
  
 すべての承認は、有効な Windows ユーザー ID を前提としています。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は、ユーザー ID の認証に Windows 認証だけを使用します。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 独自の認証メソッドを提供しません。参照してください[Analysis Services でサポートされる認証方法](../instances/authentication-methodologies-supported-by-analysis-services.md)します。  
  
> [!IMPORTANT]  
>  権限は、データベース内のすべてのロールにわたって、Windows ユーザーまたはグループごとに加算的です。 1 つのロールで、あるユーザーまたはグループに対して特定のタスクの実行や、特定のデータの表示の権限を拒否しても、別のロールでこれらの権限がそのユーザーまたはグループに与えられる場合、ユーザーまたはグループはそのタスクの実行またはデータの表示の権限を持つことになります。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
-   [オブジェクトと操作へのアクセスの承認&#40;Analysis Services&#41;](authorizing-access-to-objects-and-operations-analysis-services.md)  
  
-   [データベースのアクセス許可を付与&#40;Analysis Services&#41;](grant-database-permissions-analysis-services.md)  
  
-   [キューブまたはモデル アクセス許可を付与&#40;Analysis Services&#41;](grant-cube-or-model-permissions-analysis-services.md)  
  
-   [プロセスのアクセス許可を付与&#40;Analysis Services&#41;](grant-process-permissions-analysis-services.md)  
  
-   [読み取りオブジェクト メタデータに対する定義のアクセス許可の付与&#40;Analysis Services&#41;](grant-read-definition-permissions-on-object-metadata-analysis-services.md)  
  
-   [データ ソース オブジェクトに対する権限の付与&#40;Analysis Services&#41;](grant-permissions-on-a-data-source-object-analysis-services.md)  
  
-   [データ マイニング構造およびモデルに対する権限の付与&#40;Analysis Services&#41;](grant-permissions-on-data-mining-structures-and-models-analysis-services.md)  
  
-   [ディメンションに対する権限を付与&#40;Analysis Services&#41;](grant-permissions-on-a-dimension-analysis-services.md)  
  
-   [ディメンション データへのカスタム アクセス権を付与&#40;Analysis Services&#41;](grant-custom-access-to-dimension-data-analysis-services.md)  
  
-   [セル データへのカスタム アクセス権を付与&#40;Analysis Services&#41;](grant-custom-access-to-cell-data-analysis-services.md)  
  
## <a name="see-also"></a>参照  
 [作成し、管理ロール&#40;SSAS 表形式&#41;](../tabular-models/roles-ssas-tabular.md)  
  
  
