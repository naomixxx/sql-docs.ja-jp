---
title: RS.exe ユーティリティ (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- automatic report server tasks
- rs utility
- command prompt utilities [Reporting Services]
- report servers [Reporting Services], automating tasks
- command prompt utilities [SQL Server], rs
- scripts [Reporting Services], command prompt
- deploying reports [Reporting Services]
ms.assetid: bd6f958f-cce6-4e79-8a0f-9475da2919ce
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: 3cbc3a76c1f6e5c67297f44c312fe0497666a9b8
ms.sourcegitcommit: 2429fbcdb751211313bd655a4825ffb33354bda3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52505641"
---
# <a name="rsexe-utility-ssrs"></a>RS.exe Utility (SSRS)
  rs.exe ユーティリティは入力ファイル内に指定したスクリプトを処理します。 このユーティリティを使用して、レポート サーバーの配置と管理タスクを自動化します。  
  
> [!NOTE]  
>  [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]以降では、 **rs** ユーティリティは、SharePoint 統合モード用に構成されているレポート サーバー、およびネイティブ モードで構成されているレポート サーバーで使用できます。 以前のバージョンでは、ネイティブ モードの構成のみがサポートされていました。  
  
 **このトピックの内容:**  
  
-   [ファイルの場所](#bkmk_filelocation)  
  
-   [引数](#bkmk_arguments)  
  
-   [権限](#bkmk_permissions)  
  
-   [使用例](#bkmk_examples)  
  
## <a name="syntax"></a>構文  
  
```  
  
      rs {-?}  
{-i input_file=}  
{-s serverURL}  
{-u username}  
{-p password}  
{-e endpoint}  
{-l time_out}  
{-b batchmode}  
{-v globalvars=}  
{-t trace}  
```  
  
##  <a name="bkmk_filelocation"></a> ファイルの場所  
 **RS.exe** は **\Program Files\Microsoft SQL Server\110\Tools\Binn**にあります。 このユーティリティは、ファイル システム上の任意のフォルダーから実行できます。  
  
##  <a name="bkmk_arguments"></a> 引数  
 **-?**  
 (省略可) **rs** の引数の構文を表示します。  
  
 `-i` *入力ファイル*  
 (必須) 実行する .rss ファイルを指定します。 この値は、.rss ファイルへの相対パスまたは完全修飾パスになります。  
  
 `-s` *serverURL*  
 (必須) Web サーバー名とレポート サーバーの仮想ディレクトリ名を指定して、対象ファイルを実行します。 レポート サーバーの URL は、 `http://examplewebserver/reportserver`のように指定します。 サーバー名の先頭のプレフィックス http:// または https:// は省略可能です。 プレフィックスを省略した場合、レポート サーバーのスクリプト ホストは、まず https の使用を試み、https が使用できない場合は、http を使用します。  
  
 `-u` [*ドメイン*\\]*ユーザー名*  
 (省略可) レポート サーバーへの接続に使用するユーザー アカウントを指定します。 `-u` および `-p` を省略した場合、現在の Windows ユーザー アカウントが使用されます。  
  
 `-p` *パスワード*  
 (`-u` を指定した場合は必須) `-u` 引数で使用するパスワードを指定します。 この値は、大文字と小文字が区別されます。  
  
 `-e`  
 (省略可) スクリプトを実行する SOAP エンドポイントを指定します。 有効な値は次のとおりです。  
  
-   Mgmt2010  
  
-   Mgmt2006  
  
-   Mgmt2005  
  
-   Exec2005  
  
 値が指定されていない場合は、Mgmt2005 エンドポイントが使用されます。 SOAP エンドポイントの詳細については、「 [Report Server Web Service Endpoints](../report-server-web-service/methods/report-server-web-service-endpoints.md)」を参照してください。  
  
 `-l` *time_out*  
 (省略可) サーバーへの接続がタイムアウトするまでの期間を秒数で指定します。既定値は 60 秒です。 タイムアウト値を指定しない場合、既定値が使用されます。 値が `0` の場合は、接続はタイムアウトしません。  
  
 **-b**  
 (省略可) スクリプト ファイル内のコマンドをバッチで実行します。 いずれかのコマンドが失敗すると、バッチはロールバックされます。 バッチで実行できないコマンドがありますが、それらのコマンドは通常どおり実行されます。 スクリプト内でスローされるが処理されない例外だけが、ロールバックされます。 スクリプトによって例外が処理され、`Main` から正常に値が返された場合、バッチはコミットされます。 このパラメーターを省略した場合、バッチを作成せずにコマンドが実行されます。 詳細については、「 [Batching Methods](../report-server-web-service-net-framework-soap-headers/batching-methods.md)」をご参照ください。  
  
 `-v` *globalvar*  
 (省略可) スクリプト内で使用するグローバル変数を指定します。 スクリプトでグローバル変数を使用する場合は、この引数を指定する必要があります。 .rss ファイルで定義したグローバル変数に対して有効な値を指定する必要があります。 **-v** 引数ごとに 1 つのグローバル変数を指定できます。  
  
 `-v` 引数はコマンド ラインで指定され、ユーザーのスクリプトに定義されているグローバル変数の値を実行時に設定するために使用されます。 たとえば、スクリプトに *parentFolder*という名前の変数が含まれている場合、そのフォルダーの名前をコマンド ラインで指定することができます。  
  
 `rs.exe -i myScriptFile.rss -s http://myServer/reportserver -v parentFolder="Financial Reports"`  
  
 指定した値に設定された名前を使用して、グローバル変数を作成します。 たとえば、 **-v を =**"`1`" **-v b =**"`2`"という名前の変数に結果`a`の値を持つ"`1`"変数と、 **b**の値を持つ"`2`"。  
  
 グローバル変数は、スクリプト内のすべての関数で使用できます。 円記号と引用符 (**\\"**) は、二重引用符と解釈されます。 引用符は、文字列にスペースが含まれている場合のみ必要です。 変数名は、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]で有効な名前であることが必要です。また変数名は、アルファベットやアンダースコアで始まり、アルファベット文字、数字、およびアンダースコアを含めることができます。 予約語は、変数名として使用できません。 グローバル変数の使用の詳細については、「[式で使用される組み込みコレクション &#40;レポート ビルダーおよび SSRS&#41;](../report-design/built-in-collections-in-expressions-report-builder.md)」を参照してください。  
  
 **-t**  
 (省略可) エラー メッセージをトレース ログに出力します。 この引数は値を取りません。 詳細については、「 [Report Server Service Trace Log](../report-server/report-server-service-trace-log.md)」を参照してください。  
  
##  <a name="bkmk_permissions"></a> Permissions  
 ツールを実行するには、スクリプトの実行対象であるレポート サーバー インスタンスに接続するための権限が必要です。 スクリプトを実行して、ローカル コンピューターまたはリモート コンピューターに変更を加えることができます。 リモート コンピューターにインストールしたレポート サーバーを変更するには、`-s` 引数でリモート コンピューターを指定します。  
  
##  <a name="bkmk_examples"></a> 使用例  
 次の例では、実行する [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET スクリプト、および Web サービス メソッドを含むスクリプト ファイルの指定方法を示しています。  
  
```  
rs -i c:\scriptfiles\script_copycontent.rss -s http://localhost/reportserver  
```  
  
 詳細な例については、「 [レポート サーバー間でコンテンツをコピーするサンプル Reporting Services rs.exe スクリプト](sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)」を参照してください。  
  
 その他の例については、「[Reporting Services スクリプト ファイルを実行する](run-a-reporting-services-script-file.md)」を参照してください。  
  
## <a name="remarks"></a>コメント  
 スクリプトを定義して、システム プロパティの設定、レポートのパブリッシュなどが行えます。 作成したスクリプトには、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] API の任意のメソッドを含めることができます。 使用可能なメソッドおよびプロパティの詳細については、「 [Report Server Web Service](../report-server-web-service/report-server-web-service.md)」を参照してください。  
  
 スクリプトは、 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET のコードで記述し、.rss ファイル名拡張子が付いた Unicode または UTF-8 テキスト ファイルとして保存する必要があります。 **rs** ユーティリティを使用して、スクリプトをデバッグすることはできません。 スクリプトをデバッグするには、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]内でコードを実行します。  
  
> [!TIP]  
>  詳細な例については、「 [レポート サーバー間でコンテンツをコピーするサンプル Reporting Services rs.exe スクリプト](sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)」を参照してください。  
  
## <a name="see-also"></a>参照  
 [Reporting Services スクリプト ファイルを実行する](run-a-reporting-services-script-file.md)   
 [配置タスクおよび管理タスクのスクリプト作成](script-deployment-and-administrative-tasks.md)   
 [rs.exe ユーティリティと Web サービスを使用したスクリプト](script-with-the-rs-exe-utility-and-the-web-service.md)   
 [レポート サーバーのコマンド プロンプト ユーティリティ &#40;SSRS&#41;](report-server-command-prompt-utilities-ssrs.md)  
  
  
