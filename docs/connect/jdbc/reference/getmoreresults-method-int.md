---
title: getMoreResults (int) メソッド |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerStatement.getMoreResults (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6419e5a8-8b3a-4d5b-8226-95865c52c723
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a7d4fe32d7d1ac4be9a20923fc4d230ac64d1c8d
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47667810"
---
# <a name="getmoreresults-method-int"></a>getMoreResults (int) メソッド
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) オブジェクトの次の結果に移動し、渡されたモードで指定された指示に従って、現在開いているすべての結果セット オブジェクトを処理します。  
  
## <a name="syntax"></a>構文  
  
```  
  
public final boolean getMoreResults(int mode)  
```  
  
#### <a name="parameters"></a>パラメーター  
 *mode*  
  
 現在開いている結果セット オブジェクトの処理方法を示す **int** です。 次のいずれかの定数を指定する必要があります。  
  
 CLOSE_CURRENT_RESULT  
  
 KEEP_CURRENT_RESULT (JDBC ドライバーではサポートされていません)  
  
 CLOSE_ALL_RESULTS  
  
## <a name="return-value"></a>戻り値  
 返された結果が結果セットである場合は **true** です。 それ以外の場合は、 **false**です。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 この getMoreResults メソッドは、java.sql.Statement インターフェイスで、getMoreResults メソッドによって指定されます。  
  
 結果を取得する前に getMoreResults メソッドを呼び出した場合、このメソッドは引数 *mode* で指定されたとおりに動作し、次の結果に移動します。  
  
> [!NOTE]  
>  JDBC ドライバーは、定数 KEEP_CURRENT_RESULT の使用をサポートしていません。 この定数を使用すると、例外がスローされます。  
  
## <a name="see-also"></a>参照  
 [getMoreResults メソッド &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)   
 [SQLServerStatement のメンバー](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement クラス](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
