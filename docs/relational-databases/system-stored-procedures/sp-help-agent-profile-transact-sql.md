---
title: sp_help_agent_profile (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_help_agent_profile
- sp_help_agent_profile_TSQL
helpviewer_keywords:
- sp_help_agent_profile
ms.assetid: 5637b671-4aa3-497e-9a1c-c99798a1afb4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d75fde4ff1ccabd56243e1a1ccdff8051923fefb
ms.sourcegitcommit: ceb7e1b9e29e02bb0c6ca400a36e0fa9cf010fca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2018
ms.locfileid: "52794894"
---
# <a name="sphelpagentprofile-transact-sql"></a>sp_help_agent_profile (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  指定されたエージェントのプロファイルを表示します。 このストアド プロシージャは、ディストリビューター側で任意のデータベースについて実行されます。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```  
  
sp_help_agent_profile [ [ @agent_type = ] agent_type ]   
    [ , [ @profile_id = ] profile_id ]  
```  
  
## <a name="arguments"></a>引数  
 [  **@agent_type=**] *agent_type*  
 エージェントの種類を指定します。 *agent_type*は**int**、既定値は**0**、これらの値のいずれかを指定できます。  
  
|値|説明|  
|-----------|-----------------|  
|**1**|スナップショット エージェント|  
|**2**|ログ リーダー エージェント (Log Reader Agent)|  
|**3**|ディストリビューション エージェント|  
|**4**|[マージ エージェント]|  
|**9**|キュー リーダー エージェント (Queue Reader Agent)|  
  
 [  **@profile_id=**] *profile_id*  
 表示対象のプロファイルの ID を指定します。 *profile_id*は**int**、既定値は **-1**、内のすべてのプロファイルを返す、 **MSagent_profiles**テーブル。  
  
## <a name="result-sets"></a>結果セット  
  
|列名|データ型|説明|  
|-----------------|---------------|-----------------|  
|**profile_id**|**int**|プロファイルの ID です。|  
|**profile_name**|**sysname**|エージェント タイプに一意です。|  
|**agent_type**|**int**|**1** = スナップショット エージェント<br /><br /> **2** = ログ リーダー エージェント<br /><br /> **3** = ディストリビューション エージェント<br /><br /> **4** = マージ エージェント<br /><br /> **9** = キュー リーダー エージェント|  
|**型**|**int**|**0** = システム<br /><br /> **1** = カスタム|  
|**description**|**varchar(3000)**|プロファイルの説明です。|  
|**def_profile**|**bit**|このプロファイルがこのエージェント タイプの既定値かどうかを示します。|  
  
## <a name="return-code-values"></a>リターン コードの値  
 **0** (成功) または**1** (失敗)  
  
## <a name="remarks"></a>コメント  
 **sp_help_agent_profile**はあらゆる種類のレプリケーションで使用します。  
  
## <a name="permissions"></a>アクセス許可  
 メンバーのみ、 **sysadmin**固定サーバー ロールまたは**replmonitor**固定データベース ロールが実行できる**sp_help_agent_profile**します。  
  
## <a name="see-also"></a>参照  
 [レプリケーション エージェント プロファイルの操作](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [sp_add_agent_profile &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_drop_agent_profile &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_parameter &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)  
  
  
