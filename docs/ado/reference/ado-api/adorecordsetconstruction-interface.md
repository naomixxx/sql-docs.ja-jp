---
title: ADORecordsetConstruction インターフェイス |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- ADORecordsetConstruction
helpviewer_keywords:
- ADORecordsetConstruction interface [ADO]
ms.assetid: 08386eba-f1f7-4879-8ffd-8733930ecb2f
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 078b48c36d0ee2a1b3f368b8e6baf7346ed343fa
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47634390"
---
# <a name="adorecordsetconstruction-interface"></a>ADORecordsetConstruction インターフェイス
**ADORecordsetConstruction**インターフェイスは、ADO の構築に使用**レコード セット**から OLE DB オブジェクト**行セット**C/C++ アプリケーション内のオブジェクト。  
  
 このインターフェイスには、次のプロパティがサポートされています。  
  
## <a name="properties"></a>[プロパティ]  
  
|||  
|-|-|  
|[」の章](../../../ado/reference/ado-api/chapter-property-ado.md)|読み取り/書き込みです。<br />OLE DB を取得/設定**章**オブジェクトとの間にこの ADO **Recordset**オブジェクト。|  
|[RowPosition](../../../ado/reference/ado-api/rowposition-property-ado.md)|読み取り/書き込みです。<br />OLE DB を取得/設定**RowPosition**オブジェクトとの間にこの ADO **Recordset**オブジェクト。|  
|[行セット](../../../ado/reference/ado-api/rowset-property-ado.md)|読み取り/書き込みです。<br />OLE DB を取得/設定**行セット**オブジェクトとの間にこの ADO **Recordset**オブジェクト。|  
  
## <a name="methods"></a>メソッド  
 [なし] :  
  
## <a name="events"></a>イベント  
 [なし] :  
  
## <a name="remarks"></a>コメント  
 OLE DB を指定された**行セット**オブジェクト (`pRowset`)、ADO の構築**Recordset**オブジェクト (`adoRs`) に次の 3 つの基本的な操作。  
  
1.  ADO の作成**Recordset**オブジェクト。  
  
    ```  
    Recordset20Ptr adoRs;  
    adoRs.CreateInstance(__uuidof(Recordset));  
    ```  
  
2.  クエリ、 **IADORecordsetConstruction**インターフェイスを**Recordset**オブジェクト。  
  
    ```  
    adoRecordsetConstructionPtr adoRsConstruct=NULL;  
    adoRs->QueryInterface(__uuidof(ADORecordsetConstruction),  
                         (void**)&adoRsConstruct);  
    ```  
  
3.  呼び出す、`IADORecordsetConstruction::put_Rowset`プロパティを設定するメソッド、OLE DB `Rowset` ADO 上のオブジェクト`Recordset`オブジェクト。  
  
    ```  
    IUnknown *pUnk=NULL;  
    pRowset->QueryInterface(IID_IUnknown, (void**)&pUnk);  
    adoRsConstruct->put_Rowset(pUnk);  
    ```  
  
 結果として得られる`adoRs`オブジェクトが、ADO を表すようになりました**レコード セット**OLE DB から構築されたオブジェクト**行セット**オブジェクト。  
  
 ADO を構築することもできます。 **Recordset**から OLE DB オブジェクト**章**または**RowPosition**オブジェクト。  
  
## <a name="requirements"></a>要件  
 **バージョン:** ADO 2.0 以降  
  
 **ライブラリ:** msado15.dll  
  
 **UUID:** 00000283-0000-0010-8000-00AA006D2EA4  
  
## <a name="see-also"></a>参照  
 [RecordSet オブジェクト (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Rowset プロパティ (ADO)](../../../ado/reference/ado-api/rowset-property-ado.md)
