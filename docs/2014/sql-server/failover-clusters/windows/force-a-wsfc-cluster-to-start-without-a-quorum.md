---
title: クォーラムを使用せずに WSFC クラスターを強制的に起動する | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 674f6f53610c8bf864aba5a2b5c7310c10f969c2
ms.sourcegitcommit: 334cae1925fa5ac6c140e0b2c38c844c477e3ffb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53376734"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>クォーラムを使用せずに WSFC クラスターを強制的に起動する
  このトピックでは、クォーラムを使用せずに Windows Server フェールオーバー クラスタリング (WSFC) クラスター ノードを強制的に起動する方法について説明します。  この処理が必要になるのは、ディザスター リカバリーとマルチサブネットのシナリオにおいて、[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]および [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] フェールオーバー クラスター インスタンスのデータを復旧し、高可用性を完全に再確立する場合です。  
  
-   **開始する前に。**[推奨事項](#Recommendations)、[セキュリティ](#Security)  
  
-   **クラスター クォーラムを使用せずを起動させるには。**[フェールオーバー クラスター マネージャーを使用して](#FailoverClusterManagerProcedure)、 [Powershell を使用して](#PowerShellProcedure)、 [Net.exe の使用](#CommandPromptProcedure)  
  
-   **フォローしてください：**[補足情報:クラスターを強制的にクォーラムを使用せずに開始した後](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> 開始前の準備  
  
###  <a name="Recommendations"></a> 推奨事項  
 明示的に指定されている場合を除き、このトピックの手順は、WSFC クラスター内のノードから実行する必要があります。  ただし、クォーラムを使用せずに強制的に起動する対象となるノードからこれらの手順を実行することにより、適切な結果が得られ、ネットワークの問題の発生を回避できる場合もあります。  
  
###  <a name="Security"></a> セキュリティ  
 ユーザーは、WSFC クラスターの各ノードのローカル Administrators グループのメンバーであるドメイン アカウントを使用する必要があります。  
  
##  <a name="FailoverClusterManagerProcedure"></a> フェールオーバー クラスター マネージャーの使用  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>クォーラムを使用せずにクラスターを強制的に起動するには  
  
1.  フェールオーバー クラスター マネージャーを開き、強制的にオンラインにする目的のクラスター ノードに接続します。  
  
2.  **[アクション]** ペインで、**[クラスターの強制起動]** をクリックし、**[はい - クラスターを強制起動します]** をクリックします。  
  
3.  左ペインにある **[フェールオーバー クラスター マネージャー]** ツリーで、クラスター名をクリックします。  
  
4.  、[概要] ウィンドウで確認する、現在**クォーラム構成**値します。**警告:クラスターは ForceQuorum 状態で実行されている**します。  
  
##  <a name="PowerShellProcedure"></a> PowerShell の使用  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>クォーラムを使用せずにクラスターを強制的に起動するには  
  
1.  **[実行管理者として実行]** から高度な権限で Windows PowerShell を起動します。  
  
2.  `FailoverClusters` モジュールをインポートしてクラスター コマンドレットを有効にします。  
  
3.  `Stop-ClusterNode` を使用して、クラスター サービスが停止していることを確認します。  
  
4.  `Start-ClusterNode` を指定した `-FixQuorum` を使用して、クラスター サービスを強制的に起動します。  
  
5.  `Get-ClusterNode` を指定した `-Propery NodeWieght = 1` を使用して、ノードがクォーラムの投票メンバーであることを保証する値を設定します。  
  
6.  クラスター ノードのプロパティを判読可能な形式で出力します。  
  
### <a name="example-powershell"></a>例 (PowerShell)  
 次の例では、クォーラムを使用せずに AlwaysOnSrv02 ノードのクラスター サービスを強制的に起動します。また、 `NodeWeight = 1`を設定し、新しく強制的に起動されたノードからクラスター ノードの状態を列挙します。  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "AlwaysOnSrv02"  
Stop-ClusterNode -Name $node  
Start-ClusterNode -Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="CommandPromptProcedure"></a> net.exe の使用  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>クォーラムを使用せずにクラスターを強制的に起動するには  
  
1.  リモート デスクトップを使用して、強制的にオンラインにする目的のクラスター ノードに接続します。  
  
2.  **[実行管理者として実行]** から高度な権限でコマンド プロンプトを起動します。  
  
3.  **net.exe** を使用して、ローカルのクラスター サービスが停止していることを確認します。  
  
4.  **を指定した** net.exe `/forcequorum` を使用して、ローカルのクラスター サービスを強制的に起動します。  
  
### <a name="example-netexe"></a>例 (net.exe)  
 次の例では、クォーラムを使用せずにノードのクラスター サービスを強制的に起動します。また、 `NodeWeight = 1`を設定し、新しく強制的に起動されたノードからクラスター ノードの状態を列挙します。  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="FollowUp"></a> フォローしてください：クラスターを強制的にクォーラムを使用せずに開始した後  
  
-   他のノードをオンラインに戻す前に、NodeWeight の値を再評価および再構成して、新しいクォーラムを正しく構築する必要があります。 この処理を行わないと、クラスターが再びオフラインに戻る場合があります。  
  
     詳細については、「[WSFC クォーラム モードと投票の構成 &#40;SQL Server&#41;](wsfc-quorum-modes-and-voting-configuration-sql-server.md)」を参照してください。  
  
-   このトピックの手順は、クォーラムに予定外の障害が発生した場合に、WSFC クラスターをオンラインに戻すことのできる唯一の手順です。  他の WSFC クラスター ノードが新しいクォーラム構成に影響を与えないようにするために、追加の手順の実行が必要になる場合もあります。  
  
-   また、データを復旧し、高可用性を完全に再確立するために、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] のその他の機能 ( [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]、データベース ミラーリング、ログ配布など) にも追加の操作が必要になる場合があります。  
  
     **詳細:**  
  
     [可用性グループの強制手動フェールオーバーの実行 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [データベース ミラーリング セッションでのサービスを強制する &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [ログ配布のセカンダリへのフェールオーバー &#40;SQL Server&#41;](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="RelatedContent"></a> 関連コンテンツ  
  
-   [フェールオーバー クラスターのイベントおよびログを表示する](https://technet.microsoft.com/en-us/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog フェールオーバー クラスター コマンドレット](https://technet.microsoft.com/library/ee461045.aspx)  
  
## <a name="see-also"></a>参照  
 [WSFC の強制クォーラムによる災害復旧 &#40;SQL Server&#41;](wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [クラスター クォーラムの NodeWeight の設定の構成](configure-cluster-quorum-nodeweight-settings.md)   
 [タスク フォーカスによって一覧表示される Windows PowerShell でのフェールオーバー クラスター コマンドレット](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
  
