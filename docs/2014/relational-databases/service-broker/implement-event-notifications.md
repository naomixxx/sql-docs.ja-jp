---
title: イベント通知の実装 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], target service
- target service [SQL Server]
- event notifications [SQL Server], creating
ms.assetid: 29ac8f68-a28a-4a77-b67b-a8663001308c
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8c5b17b45b50634806c60e5064efc6ebd9d03f8b
ms.sourcegitcommit: 334cae1925fa5ac6c140e0b2c38c844c477e3ffb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53353760"
---
# <a name="implement-event-notifications"></a>イベント通知の実装
  イベント通知を実装するには、最初にイベント通知を受け取る通知先サービスを作成してから、イベント通知を作成する必要があります。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] ダイアログ セキュリティを構成する必要があります。 ダイアログ セキュリティは完全なセキュリティ モデルに基づいて手動で構成する必要があります。  
  
## <a name="creating-the-target-service"></a>通知先サービスの作成  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)]には次の特定のメッセージ型とイベント通知用コントラクトがあるため、 [!INCLUDE[ssSB](../../includes/sssb-md.md)] によって開始されるサービスを作成する必要はありません。  
  
```  
https://schemas.microsoft.com/SQL/Notifications/PostEventNotification  
```  
  
 イベント通知を受け取る対象サービスは、この既存のコントラクトに従う必要があります。  
  
 **通知先サービスを作成するには**:  
  
1.  メッセージを受信するキューを作成します。  
  
    > [!NOTE]  
    >  キューでは、 `https://schemas.microsoft.com/SQL/Notifications/QueryNotification`で定義されているメッセージ型が受信されます。  
  
2.  このキューにイベント通知コントラクトを参照するサービスを作成します。  
  
3.  サービスにルートを作成し、そのサービスに対するメッセージを [!INCLUDE[ssSB](../../includes/sssb-md.md)] から送信する送信先のアドレスを定義します。 イベント通知の送信先が同じデータベース内のサービスの場合は、 `ADDRESS = 'LOCAL'`を指定します。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssSB](../../includes/sssb-md.md)] のルーティングにより、通知メッセージを受け取るサービスが特定されます。 イベント通知の通知先がリモート サーバーのサービスの場合、通知側と通知先の両方のサーバーでルートを定義し、双方向の通信を確立する必要があります。  
  
 次に、キューを作成し、そのキューにサービスを作成し、そのサービスのルートを作成して、イベント通知コントラクトからのメッセージを処理する例を示します。  
  
```  
CREATE QUEUE NotifyQueue ;  
GO  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
(  
[https://schemas.microsoft.com/SQL/Notifications/PostEventNotification]  
);  
GO  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO  
```  
  
## <a name="creating-the-event-notification"></a>イベント通知の作成  
 イベント通知の作成には [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE EVENT NOTIFICATION ステートメントを、削除には DROP EVENT NOTIFICATION ステートメントを使用します。 イベント通知を変更するには、イベント通知を削除して再作成する必要があります。  
  
 次の例では、イベント通知 `CreateDatabaseNotification`を作成します。 この通知は、サーバー上で発生するすべての `CREATE_DATABASE` イベントに関するメッセージを、以前に作成した `NotifyService` サービスに送信します。  
  
```  
CREATE EVENT NOTIFICATION CreateDatabaseNotification  
ON SERVER  
FOR CREATE_DATABASE  
TO SERVICE 'NotifyService', '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
> [!CAUTION]  
>  イベント通知では、CREATE_SCHEMA イベントと、CREATE SCHEMA ステートメントの <schema_element> 定義を別々のイベントとして認識します。 たとえば、CREATE_SCHEMA イベントと CREATE_TABLE イベントの両方でイベント通知が作成され、次のバッチを実行します。  
>   
>  `CREATE SCHEMA s`  
>   
>  `CREATE TABLE t1 (col1 int)`  
>   
>  この場合、イベント通知には、2 回が発生します。CREATE_SCHEMA イベントの発生時、Onne 時間し、もう一度、CREATE_TABLE イベントの発生時です。 CREATE_SCHEMA イベントと、対応するすべての CREATE SCHEMA 定義の <schema_element> テキストの両方でイベント通知が作成されないようにするか、または不要なイベント データのキャプチャを防止するためのロジックをアプリケーションに組み込むことをお勧めします。  
  
 **イベント通知を作成するには**  
  
-   [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](/sql/t-sql/statements/create-event-notification-transact-sql)  
  
 **イベント通知を削除するには**  
  
-   [DROP EVENT NOTIFICATION &#40;Transact-SQL&#41;](/sql/t-sql/statements/drop-event-notification-transact-sql)  
  
## <a name="see-also"></a>参照  
 [イベント通知に関する情報の取得](event-notifications.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](/sql/t-sql/functions/eventdata-transact-sql)  
  
  
