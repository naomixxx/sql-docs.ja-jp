---
title: sys.fn_hadr_backup_is_preferred_replica (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: a6bba59d680a9f7ba1595808eed27762670aab97
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47597667"
---
# <a name="sysfnhadrbackupispreferredreplica--transact-sql"></a>sys.fn_hadr_backup_is_preferred_replica (TRANSACT-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  現在のレプリカが推奨されるバックアップ レプリカであるかどうかを決定するために使用されます。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>引数  
 '*dbname*'  
 バックアップするデータベースの名前です。 *dbname*が sysname 型。  
  
## <a name="returns"></a>戻り値  
 現在のインスタンス上のデータベースが優先レプリカに存在する場合は 1 を返します。 それ以外の場合 0 を返します。  
  
## <a name="remarks"></a>コメント  
 この関数は、現在のデータベースがバックアップに推奨されるレプリカ上にあるかどうかを判別するために、バックアップ スクリプトで使用します。 すべての可用性レプリカでスクリプトを実行できます。 これらの各ジョブは、同じデータをどのジョブが実行する必要がありますを決定するため、スケジュールされたジョブの 1 つだけが実際にバックアップ ステージに進みます。 サンプル コードは次のようになります。  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>使用例  
  
### <a name="a-using-sysfnhadrbackupispreferredreplica"></a>A. sys.fn_hadr_backup_is_preferred_replica を使用する  
 次の例では、現在のデータベースが推奨されるバックアップ レプリカの場合は 1 を返します。  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="RelatedTasks"></a> 関連タスク  
  
-   [可用性レプリカでのバックアップの構成 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>参照  
 [Always On 可用性グループの関数&#40;TRANSACT-SQL&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [AlwaysOn 可用性グループ &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [アクティブなセカンダリ: セカンダリ レプリカでバックアップの&#40;Always On 可用性グループ&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)[Always On 可用性グループ カタログ ビュー &#40;TRANSACT-SQL    &#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)  
  
  
