---
title: '[サーバーへの接続](Reporting Services) | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql12.swb.connection.login.reportserver.f1
ms.assetid: ef81b658-8eb5-4636-ac81-eead10cc7b9f
author: mashamsft
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 41e0ca3ee7ccaa7bb57e5667092c0660e35c4c52
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48054192"
---
# <a name="connect-to-server-reporting-services"></a>[サーバーへの接続] \(Reporting Services)
  このダイアログを使用すると、[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] に接続するときのオプションを表示または指定できます。  
  
## <a name="options"></a>および  
 **サーバーの種類**  
 **オブジェクト エクスプローラー**からサーバーを登録するときに、接続するサーバーの種類 ( [!INCLUDE[ssDE](../includes/ssde-md.md)]、Analysis Services、Reporting Services、または Integration Services) を選択します。 ダイアログの残りの部分には、選択したサーバーの種類に該当するオプションだけが表示されます。 **[登録済みサーバー]** を使用してサーバーを登録する場合、 **[サーバーの種類]** ボックスは読み取り専用になり、 **[登録済みサーバー]** コンポーネントに表示されているサーバーの種類と一致する値が表示されます。 別の種類のサーバーを登録するには、新しいサーバーの登録を開始する前に、 [!INCLUDE[ssDE](../includes/ssde-md.md)][登録済みサーバー] **ツール バーの [** ]、[Analysis Services]、[Reporting Services]、または [Integration Services] を選択します。  
  
 **サーバー名**  
 接続するレポート サーバー インスタンスのサーバー モードによって、入力する必要がある値が決まります。  
  
 ネイティブ モードで実行されているレポート サーバーの場合、接続するレポート サーバー インスタンスを指定します。 既定のインスタンスを使用する場合、サーバー名はコンピューターの名前であるのが一般的です。 名前付きインスタンスをインストールする場合は、次の形式でサーバー名にインスタンス名を追加: \<servername >\\< InstanceName\>します。 Reporting Services では、円記号を使用してインスタンス名を区切ります。  
  
 SharePoint 統合モードで実行されているレポート サーバーの場合、SharePoint サイトを指定する必要があります。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]と統合されているサイト コレクション内の任意のサイトを指定できます。 指定する URL には、HTTP プレフィックスまたは HTTPS プレフィックスを含める必要があります。 Management Studio で SharePoint サイトに接続するには、SharePoint サイトにアクセスする権限が必要です。 割り当てられている権限レベルによって、表示および管理できるアイテムが決まります。 詳細については、次を参照してください。 [Management Studio でのレポート サーバーへの接続](../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)します。  
  
 **[認証]**  
 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] を、指定したカスタム認証拡張機能によって処理する Windows 認証要求またはフォーム認証要求を受け入れるように構成できます。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]に接続する際に、次のいずれかの認証モードを選択します。  
  
 **Windows 認証モード ([Windows 認証])**  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows 資格情報を使用して、レポート サーバー インスタンスに接続します。  
  
 **基本認証**  
 Reporting Services をインストールしたときに、基本認証を使用するように構成している場合は、 **基本認証** を使用して接続します。  
  
 **フォーム認証**  
 Reporting Services をインストールしたときに、カスタム認証拡張機能を使用するように構成している場合は、 **フォーム認証** を使用して接続します。  
  
 **[ユーザー名]**  
 接続に使用するログイン名を入力します。 このオプションは、 **[基本認証]** または **[フォーム認証]** を選択した場合にのみ使用できます。  
  
 **Password**  
 ユーザー名に対応するパスワードを入力します。 このオプションは、 **[基本認証]** または **[フォーム認証]** を選択した場合にのみ編集できます。  
  
 **Connect**  
 クリックすると、上記で選択したサーバーに接続します。  
  
 **[オプション]**  
 クリックすると、サーバーの登録やパスワードの保存など、追加のサーバー接続オプションが表示されます。  
  
## <a name="see-also"></a>参照  
 [レポート サーバー データベース接続の構成&#40;SSRS 構成マネージャー&#41;](../../2014/sql-server/install/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
 [Management Studio でレポート サーバーに接続する](../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [レポート サーバーでの認証](../reporting-services/security/authentication-with-the-report-server.md)  
  
  
