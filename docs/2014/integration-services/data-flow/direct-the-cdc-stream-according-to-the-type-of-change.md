---
title: 変更の種類に応じた CDC ストリームのダイレクト | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3afa531e-f425-40a4-a1bf-1c3e1727287e
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fe0f98b373b736b9e7b97e9c5a599812210e4136
ms.sourcegitcommit: ceb7e1b9e29e02bb0c6ca400a36e0fa9cf010fca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2018
ms.locfileid: "52799654"
---
# <a name="direct-the-cdc-stream-according-to-the-type-of-change"></a>変更の種類に応じた CDC ストリームのダイレクト
  CDC スプリッター変換を追加して構成するには、パッケージに 1 つ以上のデータ フロー タスクと 1 つの CDC ソースが含まれている必要があります。  
  
 パッケージに追加する CDC ソースでは、NetCDC 処理モードが選択されている必要があります。 処理モードの選択の詳細については、[「CDC ソース エディター ([接続マネージャー] ページ)」](../cdc-source-editor-connection-manager-page.md) を参照してください。  
  
### <a name="to-direct-the-cdc-stream-according-to-the-type-of-change"></a>変更の種類に応じて CDC ストリームをダイレクトするには  
  
1.  [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]で、目的のパッケージが含まれている [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] プロジェクトを開きます。  
  
2.  ソリューション エクスプローラーで、パッケージをダブルクリックして開きます。  
  
3.  **[データ フロー]** タブをクリックし、次に **[ツールボックス]** で、CDC スプリッターをデザイン画面にドラッグします。  
  
4.  パッケージに含まれている CDC ソースを CDC スプリッターに接続します。  
  
5.  CDC スプリッターを、1 つまたは複数の変換先に接続します。 最大 3 つの出力に接続することができます。  
  
6.  以下の出力のいずれかを選択します。  
  
    -   出力を削除します。DELETE 変更行の出力がリダイレクトされます。  
  
    -   出力を挿入します。INSERT 変更行の出力がリダイレクトされます。  
  
    -   更新プログラムの出力:前に、/後の UPDATE 変更行と結合の変更が行の出力がリダイレクトされます。  
  
7.  必要に応じて、 **[詳細エディター]** ダイアログ ボックスを使用して、詳細なプロパティを構成できます。  
  
     **[詳細エディター]** ダイアログ ボックスには、プログラムによって設定できるプロパティが表示されます。  
  
     **[詳細エディター]** ダイアログ ボックスを開くには、次の操作を実行します。  
  
    -   **プロジェクトの** [データ フロー] [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 画面で、CDC スプリッターを右クリックし、 **[詳細エディターの表示]** をクリックします。  
  
     CDC スプリッターの使用方法の詳細については、「Microsoft SQL Server Integration Services の CDC コンポーネント」を参照してください。  
  
## <a name="see-also"></a>参照  
 [CDC スプリッター](cdc-splitter.md)  
  
  
