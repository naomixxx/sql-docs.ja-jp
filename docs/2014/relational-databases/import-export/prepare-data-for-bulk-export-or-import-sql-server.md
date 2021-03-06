---
title: 一括エクスポートまたは一括インポートのデータの準備 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], planning
- bulk importing [SQL Server], from a CSV file
- data formats [SQL Server], planning operations
- CSV files [SQL Server]
- quoted fields in CSV files [SQL Server]
ms.assetid: 783fd581-2e5f-496b-b79c-d4de1e09ea30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d0836d835b77241a27dfccc65528e8cda440559c
ms.sourcegitcommit: 334cae1925fa5ac6c140e0b2c38c844c477e3ffb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53358134"
---
# <a name="prepare-data-for-bulk-export-or-import-sql-server"></a>一括エクスポートまたは一括インポートのデータの準備 (SQL Server)
  ここでは、一括エクスポート操作の計画に関する考慮事項と一括インポート操作の要件について説明します。  
  
> [!NOTE]  
>  一括インポート用にデータ ファイルの書式を設定する方法が不明確な場合は、 **bcp** ユーティリティを使用して、データをテーブルからデータ ファイルにエクスポートできます。 このファイル内の各データ フィールドの書式設定では、データを対応するテーブル列に一括インポートする際に必要な書式設定が示されています。 データ ファイルのフィールドに同じデータの書式設定を使用してください。  
  
## <a name="data-file-format-considerations-for-bulk-export"></a>一括エクスポートのデータ ファイル形式に関する注意点  
 **bcp** コマンドを使用して一括エクスポート操作を実行する前に、次のことを考慮してください:  
  
-   データをファイルにエクスポートする際、 **bcp** コマンドにより、指定したファイル名のデータ ファイルが自動的に作成されます。 指定の名前が既に使用されている場合、そのファイルの既存のコンテンツは、そのデータ ファイルに一括コピーするデータで上書きされます。  
  
-   テーブルまたはビューからデータを一括エクスポートする場合、一括コピーするテーブルまたはビューに SELECT 権限が必要になります。  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] では、データの取得に並列スキャンを使用できます。 そのため、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] インスタンスから一括エクスポートされるテーブル行は、通常、エクスポート先のデータ ファイルで特定の順序で表示されるとは保証されません。 一括エクスポートされたテーブル行がデータ ファイル内に特定の順序で表示されるようにするには、 **queryout** オプションを使用してクエリから一括エクスポートを行い、ORDER BY 句を指定します。  
  
## <a name="data-file-format-requirements-for-bulk-import"></a>一括インポートのデータ ファイル形式の要件  
 データ ファイルからデータをインポートするには、データ ファイルは以下の基本要件を満たしている必要があります。  
  
-   データは、行と列の形式になっている必要があります。  
  
> [!NOTE]  
>  一括インポート処理では、列がスキップされたり順番が再変更されることがあるため、データ ファイルの構造は、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] テーブルの構造と同一である必要はありません。  
  
-   データ ファイルのデータは、文字列形式やネイティブ形式など、サポートされている形式である必要があります。  
  
-   データは、Unicode を含む文字列形式またはネイティブ バイナリ形式にすることができます。  
  
-   **bcp** コマンド、BULK INSERT ステートメント、または INSERT ...SELECT * FROM OPENROWSET(BULK...) ステートメントを使用してデータをインポートするには、インポート先のテーブルが既に存在している必要があります。  
  
-   データ ファイルの各フィールドは、インポート先テーブルの対応する列と互換性がある必要があります。 たとえば、`int` フィールドを `datetime` 列に読み込むことはできません。 詳細については、「[一括インポートまたは一括エクスポートのデータ形式 &#40;SQL Server&#41;](data-formats-for-bulk-import-or-bulk-export-sql-server.md)」および「[bcp を使用した互換性のためのデータ形式の指定 &#40;SQL Server&#41;](specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)」を参照してください。  
  
    > [!NOTE]  
    >  ファイル全体をインポートするのではなく、データ ファイルからインポートする行のサブセットを指定するには、**bcp** コマンドと **-F** *first_row* スイッチまたは **-L** *last_row* スイッチ、あるいは両方のスイッチを組み合わせて使用します。 詳しくは、「 [bcp Utility](../../tools/bcp-utility.md)」をご覧ください。  
  
-   固定長フィールドまたは固定幅フィールドを含むデータ ファイルからデータをインポートするには、フォーマット ファイルを使用する必要があります。 詳細については、「 [XML フォーマット ファイル &#40;SQL Server&#41;](xml-format-files-sql-server.md)です。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] の一括インポート操作では、コンマ区切り (CSV) ファイルがサポートされていません。 ただし、場合によっては、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]に対してデータを一括インポートする際、CSV ファイルをデータ ファイルとして使用できます。 CSV ファイルのフィールド ターミネータは必ずしもコンマである必要はありません。 一括インポートのデータ ファイルとして利用できるようにするには、CSV ファイルが次の条件を満たしている必要があります。  
  
    -   データ フィールドにフィールド ターミネータが含まれていないこと。  
  
    -   データ フィールドの値を囲む引用符 ("") の有無がすべての値で統一されていること。  
  
     データを [!INCLUDE[msCoName](../../includes/msconame-md.md)] FoxPro や Visual FoxPro テーブル (.dbf) ファイル、または [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] ワークシート (.xls) ファイルから一括インポートするには、前述の制限に準拠した CSV ファイルにデータを変換する必要があります。 通常、ファイル拡張子は .csv です。 そうすると、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一括インポート操作で .csv ファイルをデータ ファイルとして使用できます。  
  
     32 ビット システムでは、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OPENROWSET [を OLE DB Provider for Jet と共に使用することにより、一括インポートの最適化を使用せずに CSV データを](/sql/t-sql/functions/openrowset-transact-sql) テーブルにインポートできます。 Jet は、データ ソースと同じディレクトリ内にある schema.ini ファイルで定義されたスキーマを使用して、テキスト ファイルをテーブルとして扱います。  CSV データの場合、schema.ini ファイル内のパラメーターの 1 つが "FORMAT=CSVDelimited" となります。 この解決方法を使用するには、Jet Test IISAMm 操作、その接続文字列の構文、schema.ini の使用法、レジストリ設定オプションなどについて理解する必要があります。  この情報については、Microsoft Access ヘルプとサポート技術情報 (KB) の資料を参照することをお勧めします。 詳細については、次を参照してください[テキスト データ ソースのドライバーの初期化](https://go.microsoft.com/fwlink/?LinkId=128503)、[セキュリティで保護された Access データベースへのリンク サーバーで SQL Server 7.0 分散クエリを使用する方法](https://go.microsoft.com/fwlink/?LinkId=128504)、 [HOW TO:。Jet OLE DB プロバイダー 4.0 を使用して ISAM データベースに接続](https://go.microsoft.com/fwlink/?LinkId=128505)、および[Jet プロバイダーの Text IIsam を使用して区切られたテキスト ファイルを開く方法](https://go.microsoft.com/fwlink/?LinkId=128501)します。  
  
 また、データ ファイルのデータをテーブルに一括インポートするには、以下の要件も満たしている必要があります。  
  
-   ユーザーは、インポート先のテーブルに対する INSERT 権限および SELECT 権限を持っている必要があります。 また、制約を無効にするなどデータ定義言語 (DDL) 操作が必要なオプションを使用する場合は、ALTER TABLE 権限も必要になります。  
  
-   BULK INSERT または INSERT ...SELECT * FROM OPENROWSET(BULK...) を使用してデータを一括インポートする際は、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] プロセスのセキュリティ プロファイル (ユーザーが [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] で提供されたログインを使用してログインしている場合) またはデリゲートされたセキュリティに基づいて使用されている [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows ログインから、このデータ ファイルにアクセスし、読み取り操作ができる状態でなければなりません。 また、ファイルを読み取るには、ユーザーに ADMINISTER BULK OPERATIONS 権限が必要です。  
  
> [!NOTE]  
>  パーティション ビューへのデータの一括インポートはサポートされません。パーティション ビューにデータを一括インポートするとエラーになります。  
  
## <a name="external-resources"></a>外部リソース  
 [[HOWTO] DTS: Excel から SQL Server にデータをインポートする方法](https://support.microsoft.com/kb/321686)  
  
## <a name="change-history"></a>変更履歴  
  
|変更内容|  
|---------------------|  
|OLE DB Provider for Jet を使用した CSV データのインポートについての情報を追加しました。|  
  
## <a name="see-also"></a>参照  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](/sql/t-sql/statements/bulk-insert-transact-sql)   
 [データ型 &#40;Transact-SQL&#41;](/sql/t-sql/data-types/data-types-transact-sql)   
 [文字形式を使用したデータのインポートまたはエクスポート &#40;SQL Server&#41;](use-character-format-to-import-or-export-data-sql-server.md)   
 [ネイティブ形式を使用したデータのインポートまたはエクスポート &#40;SQL Server&#41;](use-native-format-to-import-or-export-data-sql-server.md)  
  
  
