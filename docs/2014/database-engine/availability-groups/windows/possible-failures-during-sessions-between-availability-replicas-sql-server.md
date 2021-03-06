---
title: 可用性レプリカ間のセッション中に発生する可能性のあるエラー (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server, HADR]
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 28198abe0fe417ea29d5a10409e141a7a2ee2f1a
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48150572"
---
# <a name="possible-failures-during-sessions-between-availability-replicas-sql-server"></a>可用性レプリカ間のセッション中に発生する可能性のあるエラー (SQL Server)
  物理的な問題、オペレーティング システムの問題、または [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] の問題により、2 つの可用性レプリカ間のセッションが失敗する場合があります。 可用性レプリカでは、Sqlservr.exe が依存しているコンポーネントを定期的にチェックして、それらのコンポーネントが正常に機能しているのか失敗したのかを確認する処理は行われません。 ただし、失敗の種類によっては、影響を受けたコンポーネントからエラーが Sqlservr.exe に報告されます。 他のコンポーネントから報告されるエラーを *ハード エラー*といいます。 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] では、通知されないその他の失敗を検出するために、独自のセッション タイムアウト メカニズムを実装しています。 セッション タイムアウト期間を秒単位で指定します。 このタイムアウト時間は、別のインスタンスからの PING メッセージを受信するために、サーバー インスタンスが待機する最大時間です。この時間を過ぎると、待機していたインスタンスは接続解除されたものと見なされます。 2 つの可用性レプリカ間でセッション タイムアウトが発生すると、可用性レプリカはエラーが発生したと想定し、 *ソフト エラー*を宣言します。  
  
> [!IMPORTANT]  
>  プライマリ データベース以外のデータベースで発生した障害は検出されません。 さらに、データ ディスクの障害によりデータベースが再起動した場合を除き、データ ディスクの障害はほとんど検出されません。  
  
 したがって、エラー検出の速度と、失敗への反応時間は、ハード エラーかソフト エラーかによって異なります。 ハード エラーの中には、ネットワーク障害など、直ちに報告されるものもあります。 しかし、場合によっては、コンポーネント固有のタイムアウト時間のために報告が遅くなるハード エラーもあります。 ソフト エラーの場合は、セッション タイムアウト期間の長さによってエラー検出の速度が決まります。 既定では、この時間は 10 秒間です。 これは推奨最小値です。  
  
## <a name="failures-due-to-hard-errors"></a>ハード エラーによるエラー  
 次のような状況が、ハード エラーの原因になる可能性があります (ただし、これだけではありません)。  
  
-   接続またはケーブルの切断  
  
-   不適切なネットワーク カード  
  
-   ルーターの変更  
  
-   ファイアウォールの設定変更  
  
-   エンドポイントの再構成  
  
-   トランザクション ログの保存先ドライブの消失  
  
-   オペレーティング システムまたはプロセスのエラー  
  
 たとえば、プライマリ データベースのログ ドライブが応答を停止し障害が発生すると、オペレーティング システムから Sqlservr.exe に、深刻なエラーが発生したことが通知されます。  
  
 ネットワーク コンポーネントや一部の IO サブシステムなど、コンポーネントによっては、障害を特定するための独自のタイムアウトを実装しているものもあります。 このようなタイムアウトは [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]とは独立して機能します。そのタイムアウトが認識されたり、その動作が完全に把握されることはありません。 このような場合、タイムアウトの遅延により、障害が発生してから、可用性レプリカがハード エラーを受け取るまでの時間が増加します。  
  
> [!NOTE]  
>  ソフト エラーの場合には、可用性レプリカに対して実行されるアクティブなエラー チェックだけが発生します。 詳細については、このトピックの後半の「ソフト エラーによるエラー」を参照してください。  
  
 ネットワーク上で発生しているエラー状態を把握できるように、次のようなイベントが TCP 接続で発生した場合にはどのようなエラー メッセージがポートに送信されるのかを、ネットワーク エンジニアに問い合わせてください。  
  
-   DNS が動作していません。  
  
-   ケーブルが接続されていません。  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows に存在します。  
  
-   ポートを監視しているアプリケーションで障害が発生しています。  
  
-   Windows ベースのサーバーの名前が変更されています。  
  
-   Windows ベースのサーバーが再起動されています。  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)]は、サーバーにアクセスするクライアントに固有の問題からは保護されません。 たとえば、プライマリ レプリカへのクライアント接続をパブリック ネットワーク アダプターが処理していて、可用性グループのレプリカをホストしているサーバー インスタンス間のトラフィックをプライベート ネットワーク インターフェイス カードが処理している場合を考えます。 この場合、パブリック ネットワーク アダプターに障害が生じると、クライアントはデータベースにアクセスできなくなります。  
  
## <a name="failures-due-to-soft-errors"></a>ソフト エラーによるエラー  
 次のような状況が、セッション タイムアウトの原因になる可能性があります (ただし、これだけではありません)。  
  
-   TCP リンクのタイムアウト、パケットの紛失または破損、不正な順序のパケットなどのネットワーク エラー。  
  
-   オペレーティング システム、サーバー、またはデータベースの停止状態。  
  
-   Windows サーバーのタイムアウト。  
  
-   CPU やディスクの過負荷、いっぱいになったトランザクション ログ、メモリやスレッドが不足しているシステムなど、コンピューティング リソースの不足。 このような場合は、タイムアウト期間を長くするか、ワークロードを軽減するか、ハードウェアを交換してワークロードを処理できるようにする必要があります。  
  
### <a name="the-session-timeout-mechanism"></a>セッション タイムアウトのメカニズム  
 サーバー インスタンスはソフト エラーを直接検出できないため、ソフト エラーが原因で、可用性レプリカがセッションでの他の可用性レプリカからの応答を無限に待機する可能性があります。 このような状態を防ぐため、 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ではセッション タイムアウト メカニズムが実装されており、接続されている可用性レプリカは、一定の間隔で、開いている各接続に対して ping を送信します。 タイムアウト時間内に ping を受信した場合は、その接続がまだ開いており、サーバー インスタンスがその接続により通信していることを示します。 ping を受信すると、レプリカはその接続に対するタイムアウト カウンターをリセットします。 可用性モードとセッションのタイムアウトの関係については、次を参照してください。[可用性モード (AlwaysOn 可用性グループ)](availability-modes-always-on-availability-groups.md)します。  
  
 プライマリ レプリカとセカンダリ レプリカは、アクティブであることを通知するために互いに ping を実行します。セッション タイムアウト制限を設定することにより、相手レプリカからの ping を受信するまで無期限に待機することがなくなります。 セッション タイムアウト制限はユーザーが構成できるレプリカ プロパティで、既定値は 10 秒です。 タイムアウト時間内に ping を受信した場合は、その接続がまだ開いており、サーバー インスタンスがその接続により通信していることを示します。 可用性レプリカは、ping を受信すると、接続のタイムアウト カウンターをリセットします。  
  
 セッション タイムアウト期間中に相手のレプリカから ping を受信しなかった場合、接続はタイムアウトします。レプリカの接続は閉じられ、タイムアウトしたレプリカは DISCONNECTED 状態になります。 切断されたレプリカが同期コミット モード用に構成されている場合でも、トランザクションは、そのレプリカが再接続および再同期するのを待機しません。  
  
## <a name="responding-to-an-error"></a>エラーへの対応  
 エラーを検出したサーバー インスタンスは、エラーの種類に関係なく、インスタンスのロール、セッションの可用性モード、およびセッション内の他のすべての接続の状態に基づいて、適切な応答を行います。 パートナーの損失にどうなるかについては、次を参照してください。[可用性モード (AlwaysOn 可用性グループ)](availability-modes-always-on-availability-groups.md)します。  
  
## <a name="related-tasks"></a>Related Tasks  
 **タイムアウト値を変更するには (同期コミット可用性モードのみ)**  
  
-   [可用性レプリカのセッション タイムアウト期間の変更 &#40;SQL Server&#41;](change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **現在のタイムアウト値を表示するには**  
  
-   「[sys.availability_replicas &#40;Transact-SQL&#41;](/sql/relational-databases/system-catalog-views/sys-availability-replicas-transact-sql)」で **session_timeout** クエリを実行します。  
  
## <a name="see-also"></a>関連項目  
 [AlwaysOn 可用性グループの概要&#40;SQL Server&#41;](overview-of-always-on-availability-groups-sql-server.md)  
  
  
