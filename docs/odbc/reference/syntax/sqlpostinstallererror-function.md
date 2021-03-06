---
title: SQLPostInstallerError 関数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLPostInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPostInstallerError
helpviewer_keywords:
- SQLPostInstallerError function [ODBC]
ms.assetid: 4c60d827-b2d2-4f27-b220-daa9e1fcdd8d
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a189bd082bbf3d5f08080fccec48334165d5d15c
ms.sourcegitcommit: 6443f9a281904af93f0f5b78760b1c68901b7b8d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2018
ms.locfileid: "53208321"
---
# <a name="sqlpostinstallererror-function"></a>SQLPostInstallerError 関数
**準拠**  
 バージョンが導入されました。ODBC 3.0  
  
 **まとめ**  
 **SQLPostInstallerError**ドライバーまたはトランスレーター セットアップ ライブラリ エラーを報告するメカニズムを提供します、 **ConfigDriver**、 **ConfigDSN**、および**ConfigTranslator**インストーラー エラー キューに機能します。 アプリケーションは、この API を使用しません使用する**SQLInstallerError**エラーを取得します。  
  
## <a name="syntax"></a>構文  
  
```  
  
RETCODE SQLPostInstallerError(  
     DWORD    fErrorCode,  
     LPSTR    szErrorMsg);  
```  
  
## <a name="arguments"></a>引数  
 *fErrorCode*  
 [入力]インストーラーのエラー コード。  
  
 *後*  
 [入力]エラー メッセージ テキスト。  
  
## <a name="returns"></a>戻り値  
 SQL_SUCCESS や SQL_ERROR します。  
  
## <a name="diagnostics"></a>診断  
 **SQLPostInstallerError**自体のエラー値は通知されません。 エラーが正常にインストーラーのエラー キューにポストされたかどうか (を使用して取得可能**SQLInstallerError**)、SQL_SUCCESS が返されます。 場合、SQL_ERROR が返される値、 *dwErrorCode*引数が指定したインストーラーのエラー コードの 1 つはありません。  
  
## <a name="related-functions"></a>関連する関数  
  
|詳細|参照先|  
|---------------------------|---------|  
|追加、変更、またはドライバーを削除します。|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)|  
|追加、変更、またはデータ ソースの削除|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)|  
|翻訳オプションの設定|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|
