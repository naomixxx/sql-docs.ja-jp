---
title: マルチサーバー環境の作成 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, multiserver environments
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- multiserver environments [SQL Server]
ms.assetid: edc2b60d-15da-40a1-8ba3-f1d473366ee6
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0c5c59a8802597b893110a5f2c26c919c16c8e83
ms.sourcegitcommit: ceb7e1b9e29e02bb0c6ca400a36e0fa9cf010fca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2018
ms.locfileid: "52795618"
---
# <a name="create-a-multiserver-environment"></a>マルチサーバー環境の作成
  マルチサーバー管理では、マスター サーバー (MSX) 1 台と、対象サーバー (TSX) 1 台以上を設定する必要があります。 すべての対象サーバーで処理されるジョブは、まずマスター サーバーで定義されてから対象サーバーにダウンロードされます。  
  
 既定では、マスター サーバーと対象サーバーの間の接続では、完全な SSL (Secure Sockets Layer) 暗号化と証明書の検証が有効になります。 詳しくは、「 [対象サーバーでの暗号化オプションの設定](set-encryption-options-on-target-servers.md)」をご覧ください。  
  
 対象サーバーが多数ある場合、他の [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 機能から多くのパフォーマンス要求を受け取る実稼動サーバー上には、マスター サーバーを定義しないでください。対象サーバーのトラフィックによって実稼動サーバーのパフォーマンスが低下する可能性があります。 また、専用のマスター サーバーにイベントを転送すると、1 つのサーバーに管理を集中することができます。 詳しくは、「 [イベントの管理](manage-events.md)」をご覧ください。  
  
> [!NOTE]  
>  マルチサーバー ジョブの処理を使用するには、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] エージェント サービスのアカウントがマスター サーバーの **msdb** データベースの **TargetServersRole** ロールのメンバーでなければなりません。 マスター サーバー ウィザードを使用すると、登録処理の一環としてこのロールがサービス アカウントに自動的に追加されます。  
  
## <a name="considerations-for-multiserver-environments"></a>マルチサーバー環境に関する注意点  
 サポートされている MSX/TSX 構成については、以下の表を参照してください。  
  
||**TSX = 7.0**|**TSX = 8.0 &LT; SP3**|**TSX = 8.0 SP3 以降**|**TSX = 9.0**|**TSX = 10.0**|**TSX = 10.5**|**TSX = 11.0**|  
|-|--------------------|---------------------------|----------------------------------|--------------------|--------------------|---------------------|---------------------|  
|**MSX = 7.0**|はい|[はい]|いいえ|いいえ|いいえ|いいえ|いいえ|  
|**MSX = 8.0 &LT; SP3**|はい|[はい]|いいえ|いいえ|いいえ|いいえ|いいえ|  
|**MSX = 8.0 SP3 以降**|いいえ|いいえ|はい|[はい]|[はい]|[はい]|はい|  
|**MSX = 9.0**|いいえ|いいえ|いいえ|はい|[はい]|[はい]|はい|  
|**MSX = 10.0**|いいえ|いいえ|いいえ|いいえ|はい|[はい]|はい|  
|**MSX = 10.5**|いいえ|いいえ|いいえ|いいえ|いいえ|はい|はい|  
|**MSX = 11.0**|いいえ|いいえ|いいえ|いいえ|いいえ|いいえ|はい|  
  
 マルチサーバー環境を作成するときは、次の点を考慮してください。  
  
-   各対象サーバーは、1 つのマスター サーバーのみに対してレポートを行います。 対象サーバーを別のマスター サーバーに参加させるには、現在のマスター サーバーからその対象サーバーの参加を解除する必要があります。  
  
-   対象サーバーの名前を変更する場合は、名前を変更する前に参加を解除し、変更を行ってからもう一度参加させる必要があります。  
  
-   マルチサーバー構成を取り消す場合は、マスター サーバーからすべての対象サーバーの参加を解除する必要があります。  
  
-   SQL Server Integration Services は、マスター サーバーのバージョンと同じバージョンまたはそれ以降のバージョンの対象サーバーのみサポートします。  
  
## <a name="related-tasks"></a>Related Tasks  
 次のトピックでは、マルチサーバー環境を作成するための一般的な作業について説明します。  
  
|説明|トピック|  
|-----------------|-----------|  
|マスター サーバーを作成する方法について説明します。|[マスター サーバーの作成](make-a-master-server.md)|  
|対象サーバーを作成する方法について説明します。|[対象サーバーの作成](make-a-target-server.md)|  
|マスター サーバーに対象サーバーを参加させる方法について説明します。|[マスター サーバーへの対象サーバーの参加](enlist-a-target-server-to-a-master-server.md)|  
|マスター サーバーから対象サーバーの参加を解除する方法について説明します。|[マスター サーバーからの対象サーバーの参加の解除](defect-a-target-server-from-a-master-server.md)|  
|マスター サーバーから複数の対象サーバーの参加を解除する方法について説明します。|[マスター サーバーからの複数の対象サーバーの参加の解除](defect-multiple-target-servers-from-a-master-server.md)|  
|対象サーバーの状態を確認する方法について説明します。|[sp_help_targetserver &#40;TRANSACT-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql)<br /><br /> [sp_help_targetservergroup &#40;TRANSACT-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-help-targetservergroup-transact-sql)|  
  
## <a name="see-also"></a>参照  
 [プロキシを使用するマルチサーバー ジョブのトラブルシューティング](troubleshoot-multiserver-jobs-that-use-proxies.md)  
  
  
