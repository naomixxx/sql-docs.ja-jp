---
title: IOpenRowset を使用した行セットの作成 |マイクロソフトのドキュメント
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- IOpenRowset interface
- rowsets [OLE DB], creating
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets, creating
ms.assetid: e8bc3de7-4b97-4de9-8df8-e11947d24045
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f13332d63fb592238f21b744f9e6c5833659980
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47688830"
---
# <a name="creating-a-rowset-with-iopenrowset"></a>IOpenRowset を使用した行セットの作成
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB プロバイダーのサポート、 **iopenrowset::openrowset**メソッドは、次の制限。  
  
-   *pTableID* パラメーターが指すデータベース ID (DBID) 構造体に、ベース テーブルまたはビューを指定する必要があります。  
  
-   DBID の *eKind* メンバーに DBKIND_NAME を指定する必要があります。  
  
-   DBID の *uName* メンバーには、既存のベース テーブルまたはビューの名前を Unicode 文字列で指定する必要があります。  
  
-   **OpenRowset** の *pIndexID* パラメーターには NULL を指定する必要があります。  
  
 **IOpenRowset::OpenRowset** の結果セットには、1 つの行セットが含まれます。 1 つの行セットが含まれる結果セットは、[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] カーソルでサポートできます。 開発者はカーソル サポートによって、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] のコンカレンシー メカニズムを使用できます。  
  
## <a name="see-also"></a>参照  
 [行セット](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
