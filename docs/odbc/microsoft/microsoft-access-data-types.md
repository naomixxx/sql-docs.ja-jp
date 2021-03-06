---
title: Microsoft Access データの種類 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 11f45698a5ad8b7fd05052cbb2d23520790c425a
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47692980"
---
# <a name="microsoft-access-data-types"></a>Microsoft Access データ型
次の表は、Microsoft Access データ型やテーブルを作成するために使用するデータ型、ODBC SQL データ型を示します。  
  
|Microsoft Access データ型|データ型 (CREATETABLE)|ODBC SQL データ型|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|文字列|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|カウンター|カウンター|SQL_INTEGER|  
|通貨|通貨|SQL_NUMERIC|  
|日付/時刻|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|長のバイナリ|文字列|SQL_LONGVARBINARY|  
|長いテキスト|長いテキスト|[2] の SQL_LONGVARCHAR SQL_WLONGVARCHAR [3]|  
|メモ|長いテキスト|[2] の SQL_LONGVARCHAR SQL_WLONGVARCHAR [3]|  
|数 (フィールド サイズ = 1 つ)|1 つ|SQL_REAL|  
|数 (フィールド サイズ = 倍)|DOUBLE|SQL_DOUBLE|  
|数 (フィールド サイズ = バイト)|符号なしバイト|SQL_TINYINT|  
|数 (フィールド サイズ = 整数)|短い|SQL_SMALLINT|  
|数 (フィールド長整数を =)|LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE●ole○|文字列|SQL_LONGVARBINARY|  
|[TEXT]|VARCHAR|[1] SQL_VARCHAR SQL_WVARCHAR [2]|  
ARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] 4.0 アプリケーションのみにアクセスします。 4,000 バイトの最大長。 文字列のような動作です。  
  
 [2] は、ANSI アプリケーションだけです。  
  
 [3] Unicode とアクセス 4.0 アプリケーションだけです。  
  
> [!NOTE]  
>  **SQLGetTypeInfo** ODBC データ型を返します。 1 つ以上の Microsoft Access の種類が同じ ODBC SQL データ型にマップされている場合は、すべての Microsoft Access データ型は返されません。 付録 d のすべての変換、 *ODBC プログラマ リファレンス*の前の表に示す SQL データ型はサポートされます。  
  
 次の表では、Microsoft Access データ型の制限事項を示します。  
  
|データ型|説明|  
|---------------|-----------------|  
|BINARY、VARBINARY、および VARCHAR|0 BINARY、VARBINARY、または VARCHAR 列を作成するか指定されていない長さが実際に 510 バイト列を返します。|  
|BYTE|フィールド サイズが BYTE に等しいと Microsoft アクセス番号フィールドが署名されていない場合でも、Microsoft Access ドライバーを使用する場合、負の数は、フィールドに挿入できます。|  
|CHAR、VARCHAR、LONGVARCHAR、|文字の文字列リテラルは、(1 ~ 255 の 10 進数) の任意の ANSI 文字を含めることができます。 2 つの連続する単一引用符 (") を使用して、1 つの単一引用符 (') を表します。<br /><br /> プロシージャは任意の特殊文字を文字データ型の列を使用する場合、文字データを渡すために使用する必要があります。|  
|[DATE]|日付の値では、する、ODBC 標準の日付形式に従って区切られたか、または datetime の区切り記号 (「#」) で区切られたする必要があります。 それ以外の場合、Microsoft Access は、算術式としては、値を処理し、警告またはエラーが発生しません。<br /><br /> たとえば、日付として「1996 年 3 月 5 日」を表す必要がある {d ' 1996-03-05'} または #03/05/1996 # です。それ以外の場合、03/05/1993 が送信される場合のみ Microsoft Access はこれを 1996年で割った値 5 で割ると 3 として評価します。 この値が 0、整数へ丸めます、0 日は、1899-12-31 にマップ、されるため使用される日付になります。<br /><br /> パイプ文字 (&#124;) 引用符で囲んだ戻る場合でも、日付の値では使用できません。|  
|GUID|データ型が Microsoft Access 4.0 に制限されます。|  
|NUMERIC|データ型が Microsoft Access 4.0 に制限されます。|  
  
 データ型の複数の制限事項が記載[データ型の制限事項](../../odbc/microsoft/data-type-limitations.md)します。
