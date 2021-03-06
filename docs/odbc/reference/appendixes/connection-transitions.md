---
title: 接続の遷移 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f808460a1421a9ab4cb3a76c2810d810b9636b11
ms.sourcegitcommit: 2429fbcdb751211313bd655a4825ffb33354bda3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52537498"
---
# <a name="connection-transitions"></a>接続の遷移
ODBC 接続では、次の状態があります。  
  
|状態|説明|  
|-----------|-----------------|  
|C0|未割り当ての環境では、未割り当ての接続|  
|C1|割り当てられている環境では、未割り当ての接続|  
|C2|環境では、接続の割り当てください。 の割り当てください。|  
|C3|接続関数には、データが必要があります。|  
|C4|接続の接続|  
|C5|ステートメントが割り当てられている接続の接続|  
|C6|接続されている接続では、進行中のトランザクション。 C6 の状態にする、接続に割り当てられているステートメントと接続のことができます。 たとえば、C4 の状態で、手動コミット モードでは、接続します。 ステートメントが割り当てられ、(トランザクションを開始して)、実行、解放し、トランザクションはアクティブなままが接続でのステートメントはありません。|  
  
 次の表では、各 ODBC 関数が、接続状態にどのように影響する方法を示します。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> ない Env します。|C1 の未割り当てください。|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(組み込み)[2]|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(組み込み)[3]|(組み込み)|(08003)|(08003)|C5|--[5]|--[5]|  
|(組み込み)[4]|(組み込み)|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] この行は、移行を示しています。 ときに*HandleType* sql_handle_env としてでした。  
  
 [2] この行は、移行を示しています。 ときに*HandleType* sql_handle_dbc としてでした。  
  
 [3] この行は、移行を示しています。 ときに*HandleType* sql_handle_stmt としてでした。  
  
 [4] この行は、移行を示しています。 ときに*HandleType* SQL_HANDLE_DESC でした。  
  
 [5] 呼び出し**SQLAllocHandle**で*OutputHandlePtr*有効なハンドルを指す、前の内容 ofthat ハンドル用に関係なく、そのハンドルを上書きし、ODBC ドライバーの問題が発生する可能性があります。 呼び出す ODBC アプリケーション プログラミングを正しくないが**SQLAllocHandle**に対して定義されている同じアプリケーション変数と 2 回 *\*OutputHandlePtr*呼び出さず**SQLFreeHandle**に再割り当てする前に、ハンドルを解放します。 ODBC を上書きするようにハンドルは一貫性のない動作やエラーの ODBC ドライバーの方につながります。  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|[D] C3 C4 [s]|-[d]、C2 [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--|--C5 [1] [2]|  
  
 [1] の接続は、手動コミット モードでされました。  
  
 [2] の接続は自動コミット モードでされました。  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges、SQLColumns、SQLForeignKeys、SQLGetTypeInfo、SQLPrimaryKeys、SQLProcedureColumns、SQLProcedures、SQLSpecialColumns、SQLStatistics、SQLTablePrivileges、および SQLTables  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--C6 [1] [2]|--|  
  
 [1] で、接続は自動コミット モードまたはデータ ソースは、トランザクションを開始できませんでした。  
  
 [手動コミット モードで 2] で、接続、データ ソースは、トランザクションを開始します。  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc、SQLGetDescField、SQLGetDescRec、SQLSetDescField および SQLSetDescRec  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--[1]|--|--|  
  
 [この状態で 1]、アプリケーションで使用できる唯一の記述子は記述子を明示的に割り当てられます。  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources と SQLDrivers  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|C4 s - n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)[1]|--[3]|--[3]|--[3]|--|--|--または [4] ([5] [6]、[8] と) C4 [5] と [7] C5 [5] [6]、[9]|  
|(組み込み)[2]|(組み込み)|(08003)|(08003)|--|--|C5|  
  
 [1] この行は、移行を示しています。 ときに*HandleType* sql_handle_env としてでした。  
  
 [2] この行は、移行を示しています。 ときに*HandleType* sql_handle_dbc としてでした。  
  
 [接続状態で、接続がないため、3] は、トランザクションによる影響はありません。  
  
 [接続では、4] のコミットまたはロールバックが失敗しました。 関数は、ここでは SQL_ERROR を返します。  
  
 [5]、commit または rollback は、接続に成功しました。 関数は、commit または rollback は、別の接続が失敗するか、commit または rollback が、すべての接続に成功した場合、関数が SQL_SUCCESS を返します、SQL_ERROR を返します。  
  
 [6]、接続に割り当てられている少なくとも 1 つのステートメントがあります。  
  
 [7]、接続に割り当てられているステートメントがありません。  
  
 [8] で、接続が対象の 1 つステートメントがありますが、開いているカーソル、少なくとも、データ ソースでは、トランザクションがコミットまたはロールバック時にカーソルが保持されますと、どちらか該当する (かどうかに応じて*CompletionType*が sql _コミットまたは SQL_ROLLBACK)。 詳細についてで SQL_CURSOR_COMMIT_BEHAVIOR と SQL_CURSOR_ROLLBACK_BEHAVIOR 属性を参照してください。 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)します。  
  
 [9] の接続を開いているカーソルがすべてのステートメントがある場合、トランザクションがコミットまたはロールバック時に、カーソルが保持されません。  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect と SQLExecute  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--C6 の C6 [1] [2] [3]|--|  
  
 [1] で、接続は自動コミット モードと、ステートメントを実行できなかった、*カーソル**仕様*(SELECT ステートメントなど) あるか、接続が手動コミット モード、およびステートメント。実行されるトランザクションを開始できませんでした。  
  
 [2] の接続は自動コミット モードと実行されたステートメントだった、*カーソル**仕様*(SELECT ステートメント) など。  
  
 [手動コミット モードで 3] で、接続、データ ソースは、トランザクションを開始します。  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)[1]|C0|(HY010)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(組み込み)[2]|(組み込み)|(C1)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(組み込み)[3]|(組み込み)|(組み込み)|(組み込み)|(組み込み)|C4 [5]-[6]|--C4 [7] [5]、[8] C5 [6] と [8]|  
|(組み込み)[4]|(組み込み)|(組み込み)|(組み込み)|--|--|--|  
  
 [1] この行は、移行を示しています。 ときに*HandleType* sql_handle_env としてでした。  
  
 [2] この行は、移行を示しています。 ときに*HandleType* sql_handle_dbc としてでした。  
  
 [3] この行は、移行を示しています。 ときに*HandleType* sql_handle_stmt としてでした。  
  
 [4] この行は、移行を示しています。 ときに*HandleType* SQL_HANDLE_DESC でした。  
  
 [5]、接続に割り当てられている 1 つだけのステートメントがあります。  
  
 [6]、接続に割り当てられている複数のステートメントがあります。  
  
 [7]、接続は、手動コミット モードででした。  
  
 [8] の接続は自動コミット モードでされました。  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)[1]|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--|C5 [3]: [4]|  
|(組み込み)[2]|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--|--|  
  
 [1] この行は、トランザクションを示しています。 ときに、*オプション*引数が SQL_CLOSE します。  
  
 [2] この行は、トランザクションを示しています。 ときに、*オプション*引数が SQL_UNBIND または SQL_RESET_PARAMS します。  
  
 [3] で、接続は自動コミット モードと、この 1 つを除くすべてのステートメントで開いているカーソルがありませんでした。  
  
 [4]、接続は手動コミット モードまたは自動コミット モードし、カーソルが他に少なくとも 1 つのステートメントで開かれていた。  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|組み込み|組み込み|--[1] 08003[2]|HY010|--|--|--|  
  
 [1]、*属性*引数が SQL_ATTR_ACCESS_MODE、SQL_ATTR_AUTOCOMMIT、SQL_ATTR_LOGIN_TIMEOUT、SQL_ATTR_ODBC_CURSORS、SQL_ATTR_TRACE、または SQL_ATTR_TRACEFILE、または接続属性の値が設定されています。  
  
 [2]、*属性*SQL_ATTR_ACCESS_MODE、SQL_ATTR_AUTOCOMMIT、SQL_ATTR_LOGIN_TIMEOUT、SQL_ATTR_ODBC_CURSORS、SQL_ATTR_TRACE、または SQL_ATTR_TRACEFILE、引数がありませんでしたし、値は、接続に設定されていない必要があります。属性。  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField と SQLGetDiagRec  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)[1]|--|--|--|--|--|--|  
|(組み込み)[2]|(組み込み)|--|--|--|--|--|  
|(組み込み)[3]|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--|--|  
|(組み込み)[4]|(組み込み)|(組み込み)|(組み込み)|--|--|--|  
  
 [1] この行は、移行を示しています。 ときに*HandleType* sql_handle_env としてでした。  
  
 [2] この行は、移行を示しています。 ときに*HandleType* sql_handle_dbc としてでした。  
  
 [3] この行は、移行を示しています。 ときに*HandleType* sql_handle_stmt としてでした。  
  
 [4] この行は、移行を示しています。 ときに*HandleType* SQL_HANDLE_DESC でした。  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|組み込み|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|組み込み|組み込み|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|組み込み|組み込み|--[1] 08003[2]|08003|--|--|--|  
  
 [1]、*情報の種類*引数が SQL_ODBC_VER します。  
  
 [2]、*情報の種類*引数 SQL_ODBC_VER がありませんでした。  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--C6 [1] [2]|--C5 [3] [1]|  
  
 [1] で、接続が自動コミット モード、および呼び出し**SQLMoreResults**がカーソルの仕様の結果セットの処理が初期化されていません。  
  
 [2] の接続が自動コミット モード、および呼び出し**SQLMoreResults**がカーソルの仕様の結果セットの処理を初期化します。  
  
 [3] の接続は、手動コミット モードでされました。  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--C6 [1] [2]|--|  
  
 [1] で、接続は自動コミット モードまたはデータ ソースは、トランザクションを開始できませんでした。  
  
 [手動コミット モードで 2] で、接続、データ ソースは、トランザクションを開始します。  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|組み込み|組み込み|--[1] 08003[2]|HY010|--08002 [3] [4] HY011 [5]|--08002 [3] [4] HY011 [5]|-[3] と [6] C5 [8] [4] HY011 08002 [5] または [7]|  
  
 [1]、*属性*SQL_ATTR_TRANSLATE_LIB または SQL_ATTR_TRANSLATE_OPTION 引数がありませんでした。  
  
 [2]、*属性*引数は、SQL_ATTR_TRANSLATE_LIB または SQL_ATTR_TRANSLATE_OPTION がでした。  
  
 [3]、*属性*SQL_ATTR_ODBC_CURSORS または SQL_ATTR_PACKET_SIZE 引数がありませんでした。  
  
 [4]、*属性*引数が SQL_ATTR_ODBC_CURSORS します。  
  
 [5]、*属性*引数から SQL_ATTR_PACKET_SIZE がでした。  
  
 [6]、*属性*引数が、SQL_ATTR_AUTOCOMMIT または*属性*引数が SQL_ATTR_AUTOCOMMIT と、この属性を設定して、トランザクションをコミットできません。  
  
 [7]、*属性*引数は、SQL_ATTR_TXN_ISOLATION をでした。  
  
 [8]、*属性*引数が、SQL_ATTR_AUTOCOMMIT とトランザクションをコミットするこの属性を設定します。  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|--|--|(HY010)|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>他のすべての ODBC 関数  
  
|C0<br /><br /> ない Env します。|C1<br /><br /> 未割り当て|C2<br /><br /> 割り当てられました。|C3<br /><br /> データが必要|C4<br /><br /> 接続済み|C5<br /><br /> ステートメントから削除してください。|C6<br /><br /> トランザクション|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(組み込み)|(組み込み)|(組み込み)|(組み込み)|(組み込み)|--|--|
