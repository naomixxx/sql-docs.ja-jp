---
title: 手順 3:フィールドのリスト ボックスに入力 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 315c32dc-aeb1-4629-b30e-87b44e8f84d1
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1aecb94fd7367b12ed2c1aaca06ffe26f586e604
ms.sourcegitcommit: 6443f9a281904af93f0f5b78760b1c68901b7b8d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2018
ms.locfileid: "53206891"
---
# <a name="step-3-populate-the-fields-list-box"></a>手順 3:フィールド リスト ボックスを設定します。
フィールドのリスト ボックスを設定するには、クリック イベント ハンドラーに次のコードを挿入`lstMain`:  
  
```  
Private Sub lstMain_Click()  
    Dim rec As Record  
    Dim rs As Recordset  
    Set rec = New Record  
    Set rs = New Recordset  
    grs.MoveFirst  
    grs.Move lstMain.ListIndex  
    lstDetails.Clear  
    rec.Open grs  
    Select Case rec.RecordType  
        Case adCollectionRecord:  
            Set rs = rec.GetChildren  
            While Not rs.EOF  
                lstDetails.AddItem rs(0)  
                rs.MoveNext  
            Wend  
        Case adSimpleRecord:  
            recFields rec, lstDetails, txtDetails  
  
        Case adStructDoc:  
    End Select  
  
End Sub  
```  
  
 このコードは、宣言し、ローカルのレコードとレコード セット オブジェクトをインスタンス化`rec`と`rs`、それぞれします。  
  
 選択したリソースに対応する行`lstMain`の現在の行が行われた`grs`します。 詳細のリスト ボックスをオフにし、`rec`の現在の行でが開きます。`grs`ソースとして。  
  
 コレクションのレコードで指定されたかどうか、リソースには[RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md)、ローカル レコード セット`rs`rec の子で開かれます。`lstDetails`の行から値が入力`rs`します。  
  
 リソースが、単純なレコードの場合`recFields`が呼び出されます。 詳細については`recFields`、次の手順を参照してください。  
  
 リソースが構造化ドキュメントの場合は、コードは実装されません。  
  
## <a name="see-also"></a>参照  
 [インターネットのシナリオへの発行](../../../ado/guide/data/internet-publishing-scenario.md)   
 [手順 2:メイン リスト ボックスを初期化します。](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)   
 [手順 4:詳細情報のテキスト ボックスに入力します。](../../../ado/guide/data/step-4-populate-the-details-text-box.md)
