---
title: Reporting Services の移行 (SharePoint モード) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- database-engine
ms.topic: conceptual
ms.assetid: 61290949-690a-4e19-b078-57c99b6b30fa
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: a3cf7c5fd54d1742c862dacb4e207ea5a618b896
ms.sourcegitcommit: 334cae1925fa5ac6c140e0b2c38c844c477e3ffb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53363744"
---
# <a name="migrate-a-reporting-services-installation-sharepoint-mode"></a>Reporting Services の移行 (SharePoint モード)
  このトピックでは、SharePoint モードで配置した [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] を SharePoint 環境間で移行するために必要な手順の概要について説明します。 具体的な手順は、以降元の SharePoint のバージョンによって異なる場合があります。 SharePoint モードのアップグレードと移行のシナリオの詳細については、「 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)」を参照してください。 1 つのサーバーから別のサーバーにのみレポート アイテムをコピーする場合は、「 [Sample Reporting Services rs.exe Script to Migrate Content between Report Servers](../tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)」を参照してください。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ネイティブ モードの配置の移行の詳細については、「[Reporting Services のインストールの移行 &#40;ネイティブ モード&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)」を参照してください。  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010 および SharePoint 2013|  
  
 移行を実行する一般的な理由として、配置した SharePoint 2010 を SharePoint 2013 にアップグレードする場合があります。 SharePoint 2013 では、SharePoint 2010 からのインプレース アップグレードがサポートされていないため、 **データベース アタッチ アップグレード** またはコンテンツのみの移行の手順を完了する必要があります。  
  
 SharePoint 2013 のアップグレードの詳細については、以下を参照してください。  
  
-   [SharePoint 2013 へのアップグレード プロセスの概要](https://go.microsoft.com/fwlink/p/?LinkId=256688)。  
  
-   [SharePoint 2010 から SharePoint 2013 にデータベースをアップグレードする](https://go.microsoft.com/fwlink/p/?LinkId=256690)。  
  
-   [コンテンツ データベースを移動する (SharePoint 2013)](https://technet.microsoft.com/library/cc262792.aspx)  
  
 
  
##  <a name="bkmk_prior_versions"></a> SQL Server 2012 よりも前のバージョンの Reporting Services SharePoint モードからの移行  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint モード アーキテクチャは、サービス アプリケーション データベース スキーマを含め、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]で変更されました。  [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] より前のバージョンから [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]SharePoint モードに移行する場合は、最初に、SharePoint と [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint モードをインストールして、新しい SharePoint 環境を作成してください。 詳細については、次を参照してください。 [Reporting Services SharePoint モードのインストール&#40;SharePoint 2010 および SharePoint 2013&#41;](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)します。  
  
 新しい SharePoint 環境が実行されていると、コンテンツ データベースを含むデータベース レベルで、コンテンツのみの移行またはすべてのコンテンツの移行を選択できます。  
  
###  <a name="bkmk_content_only_migration"></a> コンテンツのみの移行  
 **Reporting Services コンテンツのみの移行。** コピーする場合、[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]コンテンツを新しいファーム必要などのツールを使用する**rs.exe**新しい SharePoint インストールにコンテンツをコピーします。 コンテンツのみの移行の詳細については、以下を参照してください。  
  
-   **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] RSS スクリプト:** これらのスクリプトを使用して、ネイティブ モードのレポート サーバーと SharePoint モードのレポート サーバーの間で、コンテンツとリソースを移行することができます。 詳細については、次を参照してください[Sample Reporting Services rs.exe Script to Content between Report Servers 移行](../tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)と[別を1つのレポートサーバーからコンテンツを移行するReportingServicesRS.exeスクリプト](http://azuresql.codeplex.com/releases/view/115207).  
  
-   **Reporting Services 移行ツール。** このツールを使用すると、ネイティブ モードのサーバーにあるレポート アイテムを SharePoint モードのサーバーにコピーできます。 詳細については、「 [Reporting Services 移行ツール](https://www.microsoft.com/download/details.aspx?id=29560)」を参照してください。  
  
###  <a name="bkmk_full_migration"></a> すべてのコンテンツの移行  
 **完全な移行:** SharePoint コンテンツ データベースを [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] カタログ データベースと共に新しいファームに移行する場合は、このトピックで説明しているバックアップと復元の一連のオプションを実行します。 場合によっては、バックアップ時に使用したツールとは異なるツールを復元時に使用する必要があります。 たとえば、前のバージョンの [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] の暗号化キーをバックアップする場合には [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 構成マネージャーを使用できますが、 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint モードに暗号化キーを復元する場合には SharePoint サーバーの全体管理または PowerShell を使用する必要があります。  
  
####  <a name="bkmk_databases"></a> 移行の完了時に存在するデータベース  
 次の表に、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint を正常に移行した後に存在する [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 関連の SQL Server データベースを示します。  
  
|[データベース]|名前の例||  
|--------------|------------------|-|  
|カタログ データベース|ReportingService_[service application GUID] **(\*)**|ユーザーが移行。|  
|一時データベース|ReportingService_[service application GUID]TempDB **(\*)**|ユーザーが移行。|  
|警告データベース|ReportingService_[service application GUID]_Alerting|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションの作成時に作成。|  
  
 **(\*)** 表に示した名前の例は、新しい SSRS サービス アプリケーションを作成する際に SSRS によって使用される名前付け規則に従っています。 別のサーバーから移行する場合、カタログ データベースと一時データベースの名前は元のインストールと同じになります。  
  
####  <a name="bkmk_backup_operations"></a> バックアップ操作  
 ここでは、移行に必要な情報の種類と、バックアップの実行に使用するツールやプロセスについて説明します。  
  
 ![SSRS SharePoint 移行の基本図](../../../2014/sql-server/install/media/rs-sharepoint-migration.gif "SSRS SharePoint 移行の基本図")  
  
||オブジェクト|方法|メモ|  
|-|-------------|------------|-----------|  
|**1**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 暗号化キー|**Rskeymgmt.exe** または [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 構成マネージャー。 「 [Reporting Services の暗号化キーのバックアップと復元](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)」を参照してください。|このツールはバックアップ操作だけでなく、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションの管理ページまたは PowerShell を使用した復元操作にも使用できます。|  
|**2**|SharePoint コンテンツ データベース||データベースをバックアップし、デタッチします。<br /><br /> 「[アップグレード戦略を決定する (SharePoint Server 2010)](https://technet.microsoft.com/library/cc263447.aspx)」 (https://technet.microsoft.com/library/cc263447.aspx) の「データベース接続アップグレード」を参照してください。|  
|**3**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]カタログ データベースである SQL Server データベース。|SQL Server データベースのバックアップと復元<br /><br /> または<br /><br /> SQL Server データベースのデタッチとアタッチ。||  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 構成ファイル|単純なファイル コピー。|ファイルをカスタマイズしていた場合に、単純に rsreportserver.config をコピーします。 ファイルの既定の場所の例:C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting<br /><br /> RSReportServer.config<br /><br /> Rssvrpolicy.config<br /><br /> レポート サーバーの ASP.NET アプリケーション用の Web.config<br /><br /> ASP.NET 用の Machine.config|  
  
####  <a name="bkmk_restore_operations"></a> 復元操作  
 ここでは、移行に必要な情報の種類と、復元の実行に使用するツールやプロセスについて説明します。 復元に使用するツールは、バックアップ時に使用するツールと異なる場合があります。  
  
 復元の手順を実行する前に、新しい SharePoint ファームと [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint モードをインストールして構成しておく必要があります。 基本的なインストールの詳細については[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]SharePoint モードを参照してください[Reporting Services SharePoint モードのインストール&#40;SharePoint 2010 および SharePoint 2013&#41;](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)します。  
  
||オブジェクト|方法|メモ|  
|-|-------------|------------|-----------|  
|**1**|SharePoint コンテンツ データベースを新しいファームに復元する。|SharePoint の "データベース アタッチ アップグレード" の手法。|基本的な手順:<br /><br /> 1) データベースを新しいサーバーに復元します。<br /><br /> 2) URL を指定して、コンテンツ データベースを Web アプリケーションにアタッチします。<br /><br /> 3) Get-SPWebapplication を実行して、すべての Web アプリケーションと URL を一覧表示します。<br /><br /> [アップグレード戦略の決定 (SharePoint Server 2010)](https://technet.microsoft.com/library/cc263447.aspx) に関するページ (https://technet.microsoft.com/library/cc263447.aspx) の "データベース接続アップグレード" に関するセクションと、[データベースの接続と SharePoint Server 2010 へのアップグレード](https://technet.microsoft.com/library/cc263299.aspx)に関するページ (https://technet.microsoft.com/library/cc263299.aspx) をご覧ください。|  
|**2**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] カタログ データベースである SQL データベースを復元する (ReportServer)。|SQL データベースのバックアップと復元。<br /><br /> **または**<br /><br /> SQL Server データベースのデタッチとアタッチ。|データベースを最初に使用するときに、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] によってデータベース スキーマが必要に応じて更新され、データベース スキーマが [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 環境で正常に動作できるようになります。|  
|**3**|新しい [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションを作成する。|SharePoint サーバーの全体管理。|新しいサービス アプリケーションを作成するときに、コピーしたレポート サーバー データベースを使用するように構成します。<br /><br /> SharePoint サーバーの全体管理の使用に関する詳細については、次を参照してください。、"手順 3。Reporting Services サービス アプリケーションを作成するには」の項[Install Reporting Services SharePoint Mode for SharePoint 2013](../../../2014/sql-server/install/install-reporting-services-sharepoint-mode-for-sharepoint-2013.md)します。<br /><br /> PowerShell の使用例については、「[Reporting Services の SharePoint サービスとサービス アプリケーション](../../../2014/reporting-services/reporting-services-sharepoint-service-and-service-applications.md)」の「PowerShell を使用して Reporting Services サービス アプリケーションを作成するには」をご覧ください。|  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 構成ファイルを復元する。|単純なファイル コピー。|ファイルの既定の場所の例:C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting。|  
|**5**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 暗号化キーを復元します。|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションの "SystemSettings" ページを使用して、キー バックアップ ファイルを復元します。<br /><br /> **または**<br /><br /> PowerShell を使用します。|「[Reporting Services SharePoint サービス アプリケーションの管理](../../../2014/reporting-services/manage-a-reporting-services-sharepoint-service-application.md)」の「キー管理」を参照してください。|  
  
#####  <a name="bkmk_additional_configuration"></a> その他の構成  
 以前の SharePoint 環境の構成方法によっては、次の手順のどちらか、またはこの両方を実行する必要がある場合があります。  
  
1.  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションの NTLM 認証を構成する。 詳細については、「[Reporting Services サービス アプリケーションの電子メールの構成 &#40;SharePoint 2010 および SharePoint 2013&#41;](../../reporting-services/install-windows/configure-e-mail-for-a-reporting-services-service-application.md)」を参照してください。  
  
##  <a name="bkmk_migrate_from_ctp"></a> SQL Server 2012 配置からの移行します。  
 マルチサーバー ファームでは、コンテンツ データベースとカタログ データベースが別々のコンピューターに置かれていることが多い場合があります。そのような場合は、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービスがインストールされた新しいサーバーを SharePoint ファームに追加した後で、古いサーバーを削除するだけでかまいません。 データベースをコピーする必要はありません。  
  
### <a name="backup-operations"></a>バックアップ操作  
  
1.  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 暗号化キーをバックアップします。  
  
2.  SharePoint サーバーの全体管理で (または PowerShell を使用して)、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションをバックアップします。 これによって、SharePoint のサービス アプリケーション データベースもバックアップされます。 「  [Reporting Services SharePoint サービス アプリケーションのバックアップと復元](../../../2014/reporting-services/backup-and-restore-reporting-services-sharepoint-service-applications.md)」を参照してください。  
  
3.  自動実行アカウント (UEA) と Windows 認証を使用している場合、その資格情報を記録しておき、復元プロセスでも使用できるようにしてください。  
  
4.  詳細については、「 [SharePoint 2013 でサービス アプリケーションをバックアップする](https://technet.microsoft.com/library/ee428318.aspx)」を参照してください。  
  
### <a name="restore-operations"></a>復元操作  
  
1.  SharePoint サーバーの全体管理を使用して、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] サービス アプリケーションを復元します。 PowerShell を使用することもできます。  
  
2.  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 暗号化キーを復元します。  
  
     「[Reporting Services SharePoint サービス アプリケーションの管理](../../../2014/reporting-services/manage-a-reporting-services-sharepoint-service-application.md)」の「キー管理」を参照してください  
  
3.  サービス アプリケーションに、UEA と Windows 資格情報を構成します。  
  
4.  詳細については、「 [SharePoint 2013 でサービス アプリケーションを復元する](https://technet.microsoft.com/library/ee428305.aspx)」を参照してください。  
  
##  <a name="bkmk_additional_resources"></a> その他のリソース  
  
-   [SharePoint 2013 へのアップグレードの概要 (https://technet.microsoft.com/library/ee833948.aspx)](https://technet.microsoft.com/library/ee833948.aspx)。  
  
-   [SharePoint 2013 へのアップグレード プロセスの概要 (https://technet.microsoft.com/library/cc262483.aspx)](https://technet.microsoft.com/library/cc262483.aspx)。  
  
## <a name="see-also"></a>参照  
 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
 [Reporting Services のインストールの移行 &#40;ネイティブ モード&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  
  
  
