---
title: ReportViewer (SSRS チュートリアル) を使用してパラメーターでドリルスルー (RDLC) レポートを作成 |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- reporting-services-native
ms.topic: conceptual
ms.assetid: 628c8775-c62d-45ac-b349-23db86fa4e6c
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: f04a797540abdde31411978ca05b776ee7693bd5
ms.sourcegitcommit: 334cae1925fa5ac6c140e0b2c38c844c477e3ffb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53352074"
---
# <a name="create-a-drillthrough-rdlc-report-with-parameters-using-reportviewer-ssrs-tutorial"></a>ReportViewer を使用してパラメーターを含む詳細 (RDLC) レポートを作成する (SSRS チュートリアル)
  [詳細](https://technet.microsoft.com/library/ff519554.aspx)レポートは、ユーザーが別のレポート内のリンクをクリックすることで開かれるレポートのことです。 詳細レポートには、通常、元の要約レポートに含まれるアイテムについての詳細が含まれています。 このチュートリアルでは、次のレッスンを通じて、 [ローカル モード レポート](local-vs-connected-mode-report-viewer-reporting-services-sharepoint-mode.md)でパラメーターとクエリを使用した詳細レポートを作成します。  
  
## <a name="requirements"></a>必要条件  
 このチュートリアルを使用するにはへのアクセスが必要、 **AdventureWorks2008**サンプル データベース。 このチュートリアルで使用されるクエリが使用しても**AdventureWorks2012**データベース。 詳細を取得する方法については、 **AdventureWorks2008**サンプル データベースは、「[チュートリアル。AdventureWorks データベースのインストール](https://msdn.microsoft.com/library/aa992075\(v=vs.100\).aspx)for Microsoft Visual Studio 2010。  
  
 このチュートリアルでは、Transaction-SQL クエリおよび ADO.NET [DataSet](https://msdn.microsoft.com/library/system.data.dataset\(v=vs.100\).aspx) および [DataTable](https://msdn.microsoft.com/library/system.data.datatable\(v=vs.100\).aspx) オブジェクトを理解していることを前提とします。  
  
 Visual Studio 2010 または Visual Studio 2012 と ASP.NET Web サイト テンプレートを使用して、ReportViewer コントロールを含む ASP.NET Web ページを作成します。 このコントロールは、ここで作成するレポートを表示するように構成されています。 このチュートリアルでは、Microsoft Visual C# でアプリケーションを作成します。  
  
## <a name="tasks"></a>処理手順  
 [レッスン 1:新しい Web サイトを作成します。](../reporting-services/lesson-1-create-a-new-web-site.md)   
 [レッスン 2:親レポートのデータ接続とデータ テーブルを定義します。](../reporting-services/lesson-2-define-a-data-connection-and-data-table-for-parent-report.md)   
 [レッスン 3:レポート ウィザードを使用して、親レポートをデザインします。](../reporting-services/lesson-3-design-the-parent-report-using-the-report-wizard.md)   
 [レッスン 4:子レポートのデータ接続とデータ テーブルを定義します。](../reporting-services/lesson-4-define-a-data-connection-and-data-table-for-child-report.md)   
 [レッスン 5:レポート ウィザードを使用して、子レポートをデザインします。](../reporting-services/lesson-5-design-the-child-report-using-the-report-wizard.md)   
 [レッスン 6:アプリケーションに ReportViewer コントロールを追加します。](../reporting-services/lesson-6-add-a-reportviewer-control-to-the-application.md)   
 [レッスン 7:親レポートにドリルスルー アクションを追加します。](../reporting-services/lesson-7-add-drillthrough-action-on-parent-report.md)   
 [レッスン 8:データのフィルターを作成します。](../reporting-services/lesson-8-create-a-data-filter.md)   
 [レッスン 9:ビルドして、アプリケーションの実行](../reporting-services/lesson-9-build-and-run-the-application.md)  
  
## <a name="see-also"></a>参照  
 [Reporting Services チュートリアル &#40;SSRS&#41;](../reporting-services/reporting-services-tutorials-ssrs.md)   
 [レポート デザイナーを使用してレポートをデザインする &#40;SSRS&#41;](tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md)  
  
  
