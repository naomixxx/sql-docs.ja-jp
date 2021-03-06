---
title: SQL Server のレプリケーション | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], about
- replication [SQL Server]
ms.assetid: 3a5f4592-3c61-4b4d-9ceb-39716aeeba41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9c2e9c5b1a0bf136e6b21f5b3ad6f12107d1f9b9
ms.sourcegitcommit: 7aa6beaaf64daf01b0e98e6c63cc22906a77ed04
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2019
ms.locfileid: "54126072"
---
# <a name="sql-server-replication"></a>SQL Server のレプリケーション
  レプリケーションとは、あるデータベースから別のデータベースにデータやデータベース オブジェクトをコピーおよび配布し、それらのデータベースを同期させて一貫性を保つための一連のテクノロジです。 レプリケーションを使用すると、ローカル エリア ネットワーク、ワイド エリア ネットワーク、ダイヤルアップ接続、ワイヤレス接続、インターネットなどを経由して、別の場所や、リモート ユーザーまたはモバイル ユーザーにデータを配布することができます。  
  
 トランザクション レプリケーションは、高いスループットが必要とされるサーバー間のシナリオで使用されるのが一般的です。たとえば、スケーラビリティと可用性の向上、データ ウェアハウジングとレポート、複数サイトからのデータの統合、異種データの統合、バッチ処理のオフロードなどのシナリオで使用されます。 マージ レプリケーションは、データの競合の可能性があるモバイル アプリケーションや分散サーバー アプリケーションを主な対象としています。 モバイル ユーザーとのデータ交換、店舗販売時点管理 (POS) アプリケーション、複数サイトからのデータの統合などのシナリオが一般的です。 スナップショット レプリケーションは、トランザクション レプリケーションとマージ レプリケーションに初期データセットを提供するために使用されます。データの完全な更新が必要な場合にも使用できます。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] では、この 3 種類のレプリケーションにより、企業全体のデータの同期のための強力かつ柔軟なシステムが提供されます。 SQLCE 3.5 および SQLCE 4.0 に対するレプリケーションは [!INCLUDE[win8srv](../../includes/win8srv-md.md)] と [!INCLUDE[win8](../../includes/win8-md.md)]の両方でサポートされます。  
  
 レプリケーションの代替手段として、Microsoft Sync Framework を使用してデータベースを同期できます。 Sync Framework には、SQL Server、SQL Server Express、SQL Server Compact、および SQL Azure の各データベース間での同期を容易にするコンポーネントと、直感的かつ柔軟性の高い API が含まれています。 また、Sync Framework には、SQL Server データベースと、ADO.NET と互換性があるその他のデータベースとの間で同期を行うために使用できるクラスもあります。 Sync Framework のデータベース同期コンポーネントの詳細については、「 [データベースの同期](https://go.microsoft.com/fwlink/?LinkId=209079)」を参照してください。 Sync Framework の概要の詳細については、「 [Microsoft Sync Framework デベロッパー センター](https://go.microsoft.com/fwlink/?LinkId=209078)」を参照してください。 Sync Framework とマージ レプリケーションとの比較の詳細については、データベースの同期の「 [概要とシナリオ](https://msdn.microsoft.com/library/bb902818\(SQL.110\).aspx)」を参照してください。  
  

## <a name="whats-new"></a>新機能 
- SQL Server 2017 では、SQL Server レプリケーションに重要な新機能が導入できません。 
- SQL Server 2016 では、SQL Server レプリケーションに重要な新機能が導入できません。 

旧バージョンとの互換性情報」を参照して[レプリケーションの旧バージョンとの互換性](replication-backward-compatibility.md) 


 ## <a name="replication-security"></a>レプリケーションのセキュリティ
  
-   [レプリケーションのセキュリティ設定の表示および変更](security/view-and-modify-replication-security-settings.md)  
-   [パブリケーション アクセス リストのログインの管理](security/manage-logins-in-the-publication-access-list.md)  
  
## <a name="publishing-and-distribution"></a>パブリッシングおよびディストリビューション  
  
-   [パブリッシングおよびディストリビューションの構成](configure-publishing-and-distribution.md)   
-   [パブリケーション プロパティの表示および変更](publish/view-and-modify-publication-properties.md)   
-   [パブリッシングおよびディストリビューションの無効化](disable-publishing-and-distribution.md)  
  
## <a name="publications-and-articles"></a>パブリケーションおよびアーティクル 
  
-   [パブリケーションの作成](publish/create-a-publication.md)    
-   [アーティクルの定義](publish/define-an-article.md)   
-   [パブリケーション プロパティの表示および変更](publish/view-and-modify-publication-properties.md)   
-   [アーティクルのプロパティの表示と変更](publish/view-and-modify-article-properties.md)    
-   [パブリケーションの削除](publish/delete-a-publication.md)   
-   [アーティクルの削除](publish/delete-an-article.md)    
-   [Oracle データベースからのパブリケーションの作成](publish/create-a-publication-from-an-oracle-database.md)   
-   [サブスクリプションの有効期限の設定](publish/set-the-expiration-period-for-subscriptions.md)  
-   [スキーマ オプションの指定](publish/specify-schema-options.md)  
-   [スキーマ変更のレプリケート](publish/replicate-schema-changes.md)    
-   [ID 列の管理](publish/manage-identity-columns.md)   
-   [マージ パブリケーションの互換性レベルの設定](publish/set-the-compatibility-level-for-merge-publications.md)  
  
### <a name="snapshot-options"></a>スナップショット オプション  
  
-   [スナップショットのプロパティを構成します。](publish/configure-snapshot-properties-replication-transact-sql-programming.md)    
-   [FTP でのスナップショットの配信](publish/deliver-a-snapshot-through-ftp.md) 
  
### <a name="filter-data"></a>データをフィルター処理します。  
  
-   [列フィルターの定義および変更](publish/define-and-modify-a-column-filter.md)    
-   [静的行フィルターの定義および変更](publish/define-and-modify-a-static-row-filter.md)    
-   [Define and Modify a Parameterized Row Filter for a Merge Article](publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)    
-   [パラメーター化された行フィルターの最適化](publish/optimize-parameterized-row-filters.md)    
-   [マージ アーティクル間の結合フィルターの定義および変更](publish/define-and-modify-a-join-filter-between-merge-articles.md)  
  
### <a name="transactional-replication-options"></a>トランザクション レプリケーション オプション  
  
-   [データの変更をトランザクション アーティクルに反映する方法の設定](publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)    
-   [トランザクション パブリケーションの更新可能なサブスクリプションの有効化](publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
### <a name="merge-replication-options"></a>マージ レプリケーション オプション  
  
-   [マージ テーブル アーティクル間に論理レコード リレーションシップを定義する](publish/define-a-logical-record-relationship-between-merge-table-articles.md)    
-   [マージ レプリケーションのプロパティを指定します](publish/specify-merge-replication-properties.md)    
-   [マージ アーティクル競合回避モジュールの指定](publish/specify-a-merge-article-resolver.md)    

  
## <a name="manage-subscriptions"></a>サブスクリプションを管理する  
  
-   [Create a Pull Subscription](create-a-pull-subscription.md)    
-   [プル サブスクリプションのプロパティの表示または変更](view-and-modify-pull-subscription-properties.md)    
-   [プル サブスクリプションの削除](delete-a-pull-subscription.md)    
-   [プッシュ サブスクリプションの作成](create-a-push-subscription.md)   
-   [プッシュ サブスクリプションのプロパティの表示または変更](view-and-modify-push-subscription-properties.md)   
-   [プッシュ サブスクリプションの削除](delete-a-push-subscription.md)   
-   [同期スケジュールの指定](specify-synchronization-schedules.md)    
-   [トランザクション パブリケーションの更新可能なサブスクリプションの作成](publish/create-an-updatable-subscription-to-a-transactional-publication.md)  
-   [SQL Server 以外のサブスクライバーのサブスクリプションの作成](create-a-subscription-for-a-non-sql-server-subscriber.md)  
  
## <a name="synchronize-subscriptions"></a>サブスクリプションを同期します。  
  
-   [初期スナップショットの作成および適用](create-and-apply-the-initial-snapshot.md)   
-   [パラメーター化されたフィルターを使用したパブリケーションのスナップショットの作成](create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)    
-   [バックアップからトランザクション サブスクリプションを初期化します。](initialize-a-transactional-subscription-from-a-backup.md)    
-   [手動によるサブスクリプションの初期化](initialize-a-subscription-manually.md)    
-   [プル サブスクリプションの同期](synchronize-a-pull-subscription.md)    
-   [プッシュ サブスクリプションの同期](synchronize-a-push-subscription.md)   
-   [サブスクリプションの再初期化](reinitialize-a-subscription.md)    
-   [同期中にスクリプトを実行します。](execute-scripts-during-synchronization-replication-transact-sql-programming.md)    
-   [マージ アーティクルのビジネス ロジック ハンドラーの実装](implement-a-business-logic-handler-for-a-merge-article.md)  
-   [ビジネス ロジック ハンドラーのデバッグ&#40;レプリケーション プログラミング&#41;](debug-a-business-logic-handler-replication-programming.md)    
-   [同期中にトリガーと制約の動作を制御します。](control-behavior-of-triggers-and-constraints-in-synchronization.md)    
-   [マージ アーティクルのカスタム競合回避モジュールの実装](implement-a-custom-conflict-resolver-for-a-merge-article.md)  
  
## <a name="administeration"></a>Administeration 
  
-   [レプリケーション エージェント プロファイルの操作](agents/work-with-replication-agent-profiles.md)   
-   [サブスクライバーでのデータの検証](validate-data-at-the-subscriber.md)    
-   [パラメーター化されたフィルターによるマージ パブリケーションのパーティションの管理](publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)    
-   [マージ パブリケーション内のテーブルにデータを一括読み込み](bulk-load-data-into-tables-in-a-merge-publication.md)    
-   [マージ メタデータをクリーンアップします。](administration/clean-up-merge-metadata-replication-transact-sql-programming.md)    
-   [マージ アーティクルのダミー更新を実行します。](administration/perform-a-dummy-update-for-a-merge-article-replication-transact-sql-programming.md)    
-   [レプリケートされたコマンドとディストリビューション データベースの他の情報を表示](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [トランザクション レプリケーションの連携バックアップを有効にします。](administration/enable-coordinated-backups-for-transactional-replication.md)   
-   [ピア ツー ピア トポロジを管理します。](administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)    
-   [レプリケーション トポロジの停止](administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)    
-   [Oracle パブリッシャーのトランザクション セット ジョブを構成します。](administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
-   [レプリケーション スクリプトをアップグレードします。 ](administration/upgrade-replication-scripts-replication-transact-sql-programming.md)  
  
## <a name="monitor"></a>モニター
  
-   [管理者以外のユーザーがレプリケーション モニターを使用できるようにする](monitor/allow-non-administrators-to-use-replication-monitor.md)    
-   [プログラムによるレプリケーションの監視](monitor/programmatically-monitor-replication.md)    
-   [レプリケートされたコマンドとディストリビューション データベースの他の情報を表示 ](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [マージ パブリケーションの競合情報の表示 ](view-conflict-information-for-merge-publications.md) 
-   [トランザクション レプリケーションの待機時間の計測および接続の検証](monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  