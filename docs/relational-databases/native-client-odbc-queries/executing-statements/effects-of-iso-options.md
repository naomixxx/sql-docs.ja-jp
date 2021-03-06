---
title: ISO オプションの効果 |マイクロソフトのドキュメント
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ISO options (ODBC)
- ODBC applications, ISO options
- ODBC applications, statements
- SQL Server Native Client ODBC driver, ISO options
- statements [ODBC], ISO options
ms.assetid: 813f1397-fa0b-45ec-a718-e13fe2fb88ac
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f15afaefdfe4eaf4ec1757ed0d4e202e035e8a58
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47614180"
---
# <a name="effects-of-iso-options"></a>ISO オプションの効果
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  ODBC 標準は、ISO 標準と密接に対応しています。ODBC アプリケーションは、ODBC ドライバーの動作が標準に準拠していることを前提としています。 ODBC 標準では、定義するとより厳密に準拠している動作を実現する、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバーは常に、接続先の SQL Server のバージョンで使用可能な ISO オプションを使用します。  
  
 ときに、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバーでのインスタンスに接続[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]、クライアントを使用しているサーバーの検出、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバーといくつかのオプションのセット。  
  
 ステートメント自体はドライバーが実行します。ODBC アプリケーションはステートメントの実行に対して何の要求も行いません。 ISO オプションを設定することで、SQL Native Client ODBC ドライバーを使用する ODBC アプリケーションの移植性が高まります。これは、サーバーの動作が ISO 標準に準拠するためです。  
  
 DB-Library ベースのアプリケーションは、通常これらのオプションを有効にしません。 違いが見られるサイト、同じ SQL ステートメントを実行している想定しないでくださいこので ODBC または Db-library クライアント間の動作が問題を指す、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバー。 必要があります最初ステートメントを再実行同じ SET オプションを Db-library 環境で使用されるよう、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバー。  
  
 SET オプションはユーザーやアプリケーションがいつでも有効または無効にできるので、ストアド プロシージャやトリガーの開発者は、上記の SET オプションを有効にした場合と無効にした場合の両方で、開発したプロシージャやトリガーをテストする必要があります。 これにより、プロシージャやトリガーの起動時に、接続に設定されているオプションに関係なく、プロシージャやトリガーが適切に動作することを確認できます。 これらのオプションのいずれかについて特定の設定が必要なトリガーやストアド プロシージャは、そのトリガーやストアド プロシージャの起動時に SET ステートメントを実行する必要があります。 この SET ステートメントは、トリガーやストアド プロシージャが実行されている間だけ有効になり、トリガーやストアド プロシージャが終了すると元の設定が復元されます。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] のインスタンスに接続しているときは、4 番目の SET オプションの CONCAT_NULL_YIELDS_NULL も有効になります。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC ドライバーは場合にこれらのオプションを設定していない AnsiNPW = NO またはいずれかのデータ ソースで指定が[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)または[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)します。  
  
 ISO オプションが、前述のような[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC ドライバーは、QUOTED_IDENTIFIER オプションが on の場合に有効にしません QuotedID = NO またはいずれかのデータ ソースで指定が**SQLDriverConnect**または**SQLBrowseConnect**します。  
  
 ドライバーが SET オプションの現在の状態を確認できるようにするために、ODBC アプリケーションでは、[!INCLUDE[tsql](../../../includes/tsql-md.md)] SET ステートメントを使用して SET オプションを設定しないようにします。 これらのオプションを設定する場合は、データ ソースまたは接続オプションのみを使用するようにします。 アプリケーションが SET ステートメントを実行した場合、ドライバーは不正な SQL ステートメントを生成する可能性があります。  
  
## <a name="see-also"></a>関連項目  
 [ステートメントを実行する&#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)   
 [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)   
 [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
  
