---
title: sys.dm_db_xtp_checkpoint_stats (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: =azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 43474055a9dd9d5f8b71a0c2ec008b02f4b847ed
ms.sourcegitcommit: e2fa721b6f46c18f1825dd1b0d56c0a6da1b2be1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211083"
---
# <a name="sysdmdbxtpcheckpointstats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  現在のデータベースのインメモリ OLTP チェックポイント操作に関する統計を返します。 データベースにインメモリ OLTP オブジェクトがない場合は、空の結果セットを返します。  
  
 詳細については、「[インメモリ OLTP &#40;インメモリ最適化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)」を参照してください。  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 新しいバージョンから大幅に異なるし、下位にあるトピックの説明は[SQL Server 2014](#bkmk_2014)します。**
  
## <a name="includesssql15includessssql15-mdmd-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] およびそれ以降  
 次の表に、列の記述`sys.dm_db_xtp_checkpoint_stats`以降 **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** します。  
  
|列名|型|説明|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|コント ローラーに表示される最後の LSN です。|  
|end_of_log_lsn|**numeric(38)**|ログの最後の LSN。|  
|bytes_to_end_of_log|**bigint**|ログ バイト数の間のバイト数に対応する、コント ローラーによって`last_lsn_processed`と`end_of_log_lsn`します。|  
|log_consumption_rate|**bigint**|トランザクション ログの使用量 (KB/秒) でのコント ローラーでの比率。|  
|active_scan_time_in_ms|**bigint**|トランザクション ログを自動的にスキャンすることでコント ローラーによって費やされた時間。|  
|total_wait_time_in_ms|**bigint**|ログをスキャンしない中には、コント ローラーの累積待機時間。|  
|waits_for_io|**bigint**|ログ、コント ローラーのスレッドによって発生する IO の待機の数です。|  
|io_wait_time_in_ms|**bigint**|コント ローラーのスレッドによって、ログ IO の待機に費やされた累積時間。|  
|waits_for_new_log_count|**bigint**|コント ローラーのスレッドを生成する新しいログは、によって発生する待機の数。|  
|new_log_wait_time_in_ms|**bigint**|コント ローラーのスレッドによって、新しいログ待機に費やされた累積時間。|  
|idle_attempts_count|**bigint**|アイドル状態に移行して、コント ローラーの数です。|  
|tx_segments_dispatched|**bigint**|コント ローラーに表示され、シリアライザーにディスパッチのセグメントの数。 セグメントは、シリアル化の単位を形成するログの連続する部分です。 現在のサイズが 1 MB ですが、後で変更できます。|  
|segment_bytes_dispatched|**bigint**|データベースを再起動するために、コント ローラーに、シリアライザーによってディスパッチされるバイト数の合計バイト数。|  
|bytes_serialized|**bigint**|バイトの合計数は、データベースを再起動するためにシリアル化されます。|  
|serializer_user_time_in_ms|**bigint**|ユーザー モードでのシリアライザーで費やされた時間。|  
|serializer_kernel_time_in_ms|**bigint**|カーネル モードでのシリアライザーで費やされた時間。|  
|xtp_log_bytes_consumed|**bigint**|以降、データベースに使用されたログのバイト数の合計数を再起動します。|  
|checkpoints_closed|**bigint**|チェックポイントの数は、データベースの再起動後に閉じられます。|  
|last_closed_checkpoint_ts|**bigint**|終了した前回のチェックポイントのタイムスタンプです。|  
|hardened_recovery_lsn|**numeric(38)**|回復は、この LSN から開始されます。|  
|hardened_root_file_guid|**uniqueidentifier**|完了した最後のチェックポイントの結果として書き込まれたルート ファイルの GUID です。|  
|hardened_root_file_watermark|**bigint**|**内部のみ**します。 どの程度が最大ルート ファイルの読み取りに有効な (これは、内部的に関連する型のみ - BSN と呼ばれます)。|  
|hardened_truncation_lsn|**numeric(38)**|切り捨てのポイントの LSN です。|  
|log_bytes_since_last_close|**bigint**|最後からのバイトは、ログの現在の末尾を閉じます。|  
|time_since_last_close_in_ms|**bigint**|以降、チェックポイントの最後の終了時刻。|  
|current_checkpoint_id|**bigint**|新しい現在のセグメントは、このチェックポイントに割り当てられているされます。 チェックポイントのシステムは、パイプラインです。 現在のチェックポイントはログからのセグメントに割り当てられている中です。 制限に達するとそれが、チェックポイントは、コント ローラーと新しいものを現在作成して解放されます。|  
|current_checkpoint_segment_count|**bigint**|現在のチェックポイントのセグメントの数。|  
|recovery_lsn_candidate|**bigint**|**内部でのみ**します。 Current_checkpoint_id を閉じるときに、recoverylsn として選択されるように候補。|  
|outstanding_checkpoint_count|**bigint**|終了するを待機しているパイプラインでのチェックポイントの数。|  
|closing_checkpoint_id|**bigint**|終了のチェックポイントの ID です。<br /><br /> シリアライザーが並列で作業しているため、完了したら、チェックポイントはスレッドを終了して、終了する候補です。 閉じるスレッドは 1 つずつを閉じるだけことができ、終了のチェックポイントが閉じるスレッドが実行されている 1 つであるため順番にあります。|  
|recovery_checkpoint_id|**bigint**|回復に使用されるチェックポイントの ID です。|  
|recovery_checkpoint_ts|**bigint**|チェックポイントの復旧のタイムスタンプ。|  
|bootstrap_recovery_lsn|**numeric(38)**|ブートス トラップの LSN を回復します。|  
|bootstrap_root_file_guid|**uniqueidentifier**|ブートス トラップのルート ファイルの GUID です。|  
|internal_error_code|**bigint**|コント ローラー、シリアライザー、close、およびマージのスレッドのいずれかに表示されるエラーが発生しました。|
|bytes_of_large_data_serialized|**bigint**|シリアル化されたデータの量。 |  
  
##  <a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 次の表に、列の記述`sys.dm_db_xtp_checkpoint_stats`の **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** します。  
  
|列名|型|説明|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|スレッドの現在のログ シーケンス番号 (LSN) と end-of-log 間の、ログのバイト数。|  
|total_log_blocks_processed|**bigint**|サーバーが起動した後で処理されたログ ブロックの合計数。|  
|total_log_records_processed|**bigint**|サーバーが起動した後で処理されたログ レコードの合計数。|  
|xtp_log_records_processed|**bigint**|サーバーが起動した後で処理されたインメモリ OLTP ログ レコードの合計数。|  
|total_wait_time_in_ms|**bigint**|累積待機時間 (ミリ秒)。|  
|waits_for_io|**bigint**|ログ IO の待機の数。|  
|io_wait_time_in_ms|**bigint**|ログ IO の待機に費やされた累積時間。|  
|waits_for_new_log|**bigint**|新しいログの生成に対する待機の数。|  
|new_log_wait_time_in_ms|**bigint**|新しいログの待機に費やされた累積時間。|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|前回のインメモリ OLTP チェックポイント以降に生成されたログの量。|  
|ms_since_last_checkpoint|**bigint**|前回のインメモリ OLTP チェックポイント以降の時間 (ミリ秒)。|  
|checkpoint_lsn|**数値 (38)**|前回完了したインメモリ OLTP チェックポイントに関連付けられている復旧ログ シーケンス番号 (LSN)。|  
|current_lsn|**数値 (38)**|現在処理中のログ レコードの LSN。|  
|end_of_log_lsn|**数値 (38)**|ログの末尾の LSN。|  
|task_address|**varbinary(8)**|SOS_Task のアドレス。 追加の情報を得るには、sys.dm_os_tasks と組み合わせます。|  
  
## <a name="permissions"></a>アクセス許可  
 サーバーに対する `VIEW DATABASE STATE` 権限が必要です。  
  
## <a name="see-also"></a>参照  
 [メモリ最適化テーブルの動的管理ビュー &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
