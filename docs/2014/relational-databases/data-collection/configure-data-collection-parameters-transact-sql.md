---
title: データ コレクションのパラメーターの構成 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
ms.assetid: 850905b6-35d2-4ed1-ab51-de64daa832b2
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 51e0b8360c0b5aa6662fb317a62d7ce52bf4fc41
ms.sourcegitcommit: ceb7e1b9e29e02bb0c6ca400a36e0fa9cf010fca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2018
ms.locfileid: "52748904"
---
# <a name="configure-data-collection-parameters-transact-sql"></a>データ コレクションのパラメーターの構成 (Transact-SQL)
  カスタム コレクション セットを作成する前に、データ コレクションのパラメーターを構成しておく必要があります。 これには、データ コレクターで用意されているストアド プロシージャを使用します。 この作業には、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] のクエリ エディターを使用した次の手順の実行も含まれます。  
  
> [!NOTE]  
>  データ コレクションのパラメーターは一度構成するだけです。 構成後に作成した追加のコレクション セットには、すべてこれらのパラメーターが使用されます。  
  
### <a name="configure-data-collection-parameters"></a>データ コレクションのパラメーターの構成  
  
1.  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]で、カスタム コレクション セットを作成するデータベースに接続します。  
  
2.  クエリ エディターで、次のステートメントを実行します。  
  
    ```tsql  
    USE msdb;  
    EXEC sp_syscollector_set_warehouse_instance_name N'INSTANCE_NAME';-- where instance name is the name of the SQL Server instance  
    EXEC sp_syscollector_set_warehouse_database_name N'MDW';  
    EXEC sp_syscollector_set_cache_directory N'D:\tempdata';  
    ```  
  
## <a name="see-also"></a>参照  
 [データ コレクション](data-collection.md)   
 [データ コレクター ストアド プロシージャ &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql)  
  
  
