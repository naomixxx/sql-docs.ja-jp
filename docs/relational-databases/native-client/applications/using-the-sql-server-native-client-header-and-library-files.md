---
title: SQL Server Native Client ヘッダー ファイルとライブラリ ファイルを使用して |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- header files [SQL Server Native Client]
- SQLNCLI, header files
- OLE DB, header files
- library files [SQL Server Native Client]
- SQL Server Native Client, header files
- data access [SQL Server Native Client], header files
- SQL Server Native Client ODBC driver,header files
- data access [SQL Server Native Client], library files
- SQL Server Native Client, library files
- ODBC applications, header files
- SQLNCLI, library files
ms.assetid: 69889a98-7740-4667-aecd-adfc0b37f6f0
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bb4265ac502aa5d8a90cf4df3060b1d10d5b4f36
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47778022"
---
# <a name="using-the-sql-server-native-client-header-and-library-files"></a>SQL Server Native Client ヘッダー ファイルとライブラリ ファイルの使用
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルとライブラリ ファイルは [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] と共にインストールされます。 アプリケーションを開発するときは、開発に必要なすべてのファイルを開発環境にコピーしてインストールすることが重要です。 インストールと再配布の詳細については[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client を参照してください[SQL Server Native Client をインストールする](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)します。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルとライブラリ ファイルは、次の場所にインストールされます。  
  
 *%PROGRAM FILES%* \Microsoft SQL Server\110\SDK  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイル (sqlncli.h) は、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client データ アクセス機能をカスタム アプリケーションに追加するために使用できます。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルには、[!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] で導入された新しい機能を使用するために必要なすべての定義、属性、プロパティ、およびインターフェイスが含まれています。  
  
 また、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルだけでなく、sqlncli11.lib ライブラリ ファイルもあります。このライブラリ ファイルは、ODBC の [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] BCP (一括コピー プログラム) 機能のエクスポート ライブラリです。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルは、MDAC (Microsoft Data Access Components) と共に使用する sqloledb.h ヘッダー ファイルと odbcss.h ヘッダー ファイルの両方に対して下位互換性がありますが、SQLOLEDB (MDAC 付属の OLE DB provider for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) の CLSID や ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client でサポートされない) XML 機能のシンボルは含まれていません。  
  
 ODBC アプリケーションでは、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイル (sqlncli.h) と odbcss.h を同一のプログラムから参照できません。 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] で導入された機能を使用しない場合でも、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルが以前の odbcss.h の代わりに機能します。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB プロバイダーを使用する OLE DB アプリケーションは、sqlncli.h のみを参照する必要があります。 1 つのアプリケーションで MDAC (SQLOLEDB) と [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB プロバイダーの両方を使用する場合、sqloledb.h と sqlncli.h の両方を参照できますが、sqloledb.h を先に参照する必要があります。  
  
## <a name="using-the-sql-server-native-client-header-file"></a>SQL Server Native Client ヘッダー ファイルの使用  
 使用する、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルが使用する必要があります、**含める**C/C++ プログラミング コード内のステートメント。 次のセクションでは、OLE DB と ODBC の両方のアプリケーションでこの操作を行う方法について説明します。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルとライブラリ ファイルは、Visual Studio C++ 2002 以降を使用しないとコンパイルできません。  
  
### <a name="ole-db"></a>OLE DB (OLE DB)  
 OLE DB アプリケーションで [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルを使用するには、次のプログラミング コードを使用します。  
  
```  
#define _SQLNCLI_OLEDB_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  アプリケーションで OLE DB と ODBC の両方の API を使用する場合は、上記の 1 行目のコードを省略する必要があります。 さらに、アプリケーションがある場合、**含める**sqloledb.h、ステートメント、**を含める**後に sqlncli.h 用のステートメントを記述する必要があります。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client によってデータ ソースに接続するときは、プロバイダー名文字列として "SQLNCLI11" を使用します。  
  
### <a name="odbc"></a>ODBC  
 ODBC アプリケーションで [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ヘッダー ファイルを使用するには、次のプログラミング コードを使用します。  
  
```  
#define _SQLNCLI_ODBC_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  OLE DB および ODBC Api の両方が、アプリケーションによって使用されている場合、上記のコードの最初の行を省略する必要があります。 また、アプリケーションに odbcss.h 用の `#include` ステートメントがある場合は、そのステートメントを削除する必要があります。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client によってデータ ソースに接続するときは、ドライバー名文字列として "SQL Server Native Client 11.0" を使用します。  
  
## <a name="component-names-and-properties-by-version"></a>各バージョンのコンポーネント名とプロパティ  
  
|プロパティ|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 10.0<br /><br /> SQL Server 2008:|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]|MDAC|  
|--------------|--------------------------------------------------|-------------------------------------------------------|---------------------------------------------------------------|----------|  
|ODBC ドライバー名|SQL Native Client (SQL Native Client)|SQL Server Native Client 10.0|SQL Server Native Client 11.0|SQL Server|  
|ODBC ヘッダー ファイル名|Sqlncli.h|Sqlncli.h|Sqlncli.h|Odbcss.h|  
|ODBC ドライバー DLL|Sqlncli.dll|Sqlncl10.dll|Sqlncl11.dll|sqlsrv32.dll|  
|BCP API の ODBC lib ファイル|Sqlncli.lib|Sqlncli10.lib|Sqlncli11.lib|Odbcbcp.lib|  
|BCP API の ODBC DLL|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Odbcbcp.dll|  
|OLE DB PROGID|SQLNCLI|SQLNCLI10|SQLNCLI11|SQLOLEDB|  
|OLE DB ヘッダー ファイル名|Sqlncli.h|Sqlncli.h|Sqlncli.h|Sqloledb.h|  
|OLE DB プロバイダー DLL|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Sqloledb.dll|  
  
 sqlncli.h では、SQLNCLI_VER マクロを使用して、複数のバージョンの [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client をサポートしています。 既定では、SQLNCLI_VER は最新バージョンの [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client になっています。 sqlncli11.dll ではなく sqlncli10.dll を使用するアプリケーションをビルドするには、SQLNCLI_VER を 10 に設定します。  
  
## <a name="static-linking-and-bcp-functions"></a>静的リンクと BCP 関数  
 BCP 関数を使用するアプリケーションでは、アプリケーションのコンパイルに使用されたヘッダー ファイルおよびライブラリに付属するのと同じバージョンのドライバーを接続文字列で指定することが重要です。  
  
 たとえば、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (および \Program Files\Microsoft SQL Server\110\SDK の関連するライブラリ ファイル (sqlncli11.lib) とヘッダー ファイル (sqlncli.h)) を使用してアプリケーションをコンパイルする場合は、接続文字列で "DRIVER={SQL Server Native Client 11.0}" (ODBC の場合) を指定する必要があります。  
  
 詳細については、実行を参照してください。[一括コピー操作を実行する](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md)します。  
  
## <a name="see-also"></a>参照  
 [SQL Server Native Client を使用したアプリケーションのビルド](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
