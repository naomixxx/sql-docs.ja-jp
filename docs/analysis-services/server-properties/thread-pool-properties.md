---
title: Analysis Services のスレッド プールのプロパティ |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ee8f8c4a222b2949f49c8be019b6e4f6724cfa04
ms.sourcegitcommit: f46fd79fd32a894c8174a5cb246d9d34db75e5df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785963"
---
# <a name="thread-pool-properties"></a>スレッド プール プロパティ
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は、多くの操作でマルチスレッドを使用し、複数のジョブを並列実行することによって、サーバーの全体的なパフォーマンスを向上させます。 スレッドをより効率的に管理するために、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] はスレッド プールを使用してスレッドの事前割り当てを実行し、次のジョブがスレッドを容易に利用できるようにします。  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] の各インスタンスは、スレッド プールの独自セットを維持します。 テーブル インスタンスと多次元インスタンスでは、スレッド プールの使用方法が異なります。 たとえば、多次元インスタンスのみが **IOProcess** スレッド プールを使用します。 そのため、 **PerNumaNode**テーブル インスタンスにとって意味は、この記事で説明されているプロパティ。 後の [プロパティ リファレンス](#bkmk_propref) セクションでは、各プロパティについてモードの要件が示されています。
  
> [!NOTE]  
>  NUMA システムに対する表形式の配置は、このトピックの範囲外です。 NUMA システムに対して表形式のソリューションを正常に配置できますが、表形式のモデルが採用しているインメモリ データベース テクノロジのパフォーマンス特性による利点は、高度にスケールアップされたアーキテクチャでは制約を受ける可能性があります。 詳細については、次を参照してください。 [Analysis Services ケース スタディ。大規模な商用ソリューションにおける表形式モデルを使用して](http://msdn.microsoft.com/library/dn751533.aspx)と[ハードウェア サイズ決定の表形式ソリューション](http://go.microsoft.com/fwlink/?LinkId=330359)します。  
  
##  <a name="bkmk_threadarch"></a> Analysis Services におけるスレッド管理  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] はマルチスレッドを使用して、並列実行されるタスクの数を増やすことにより、使用可能な CPU リソースを活用します。 ストレージ エンジンはマルチスレッド化されています。 ストレージ エンジン内で実行されるマルチ スレッドのジョブには、オブジェクトの並列処理、ストレージ エンジンにパブリッシュされた独立したクエリの処理、クエリによって要求されたデータ値を返す処理などがあります。 数式エンジンは、実行する計算に順次処理という性質があるため、シングル スレッド化されています。 各クエリは主にシングル スレッド内で実行され、データを要求し、多くの場合はストレージ エンジンから返されるデータを待ちます。 クエリ スレッドは実行に長い時間を要する傾向があり、クエリ全体が完了した後にのみ解放されます。  
  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 以降のバージョンの既定では、上位エディションの Windows および SQL Server で実行されている [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は、640 個を最大として、システム上で使用可能なすべての論理プロセッサを使用します。 起動時に、msmdsrv.exe プロセスは 1 つの特定プロセッサ グループに割り当てられますが、時間の経過に伴って、スレッドは任意のプロセッサ グループ内にある任意の論理プロセッサに対してスケジュールされる可能性があります。  
  
 多数のプロセッサを使用した場合の副次的な影響の 1 つとして、クエリと処理の負荷が複数のプロセッサに分散され、共有データ構造に関する競合が増加するために、パフォーマンスの低下が発生する可能性があることが挙げられます。 この現象が発生する可能性があるのは、特に NUMA アーキテクチャを使用するハイエンド システムですが、データを集中的に使用する複数のアプリケーションを同じハードウェア上で実行する非 NUMA システムにも当てはまります。  
  
 この問題を緩和するために、特定の [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 操作の種類と、特定の論理プロセッサ セットの間で関係を設定することができます。 **GroupAffinity** プロパティを使用すると、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]の管理対象であるスレッド プールの種類ごとに、どのシステム リソースを使用するかを指定するカスタム関係マスクを作成できます。
 
表形式インスタンスで **GroupAffinity** を設定する場合は、SQL Server 2016 Cumulative Update 1 (CU1) 以降をお勧めします。 
  
 **GroupAffinity** は、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] のさまざまなワークロードに使用されるどのスレッド プールでも設定できるプロパティです。  
  
-   **ThreadPool \ Parsing \ Short**  は、短い要求用の解析プールです。 単一のネットワーク メッセージ内に収まる要求は、短い要求と見なされます。 
  
-   **ThreadPool \ Parsing \ Long**  は、単一のネットワーク メッセージ内に収まらない他のすべての要求用の解析プールです。 
  
    > [!NOTE]  
    >  いずれかの解析プールに属するスレッドは、クエリの実行に使用できます。 高速検出要求やキャンセル要求など、短時間で実行されるクエリは、クエリ スレッド プールにキュー登録される代わりに、直ちに実行されます。 
  
-   **ThreadPool \ Query** は、解析スレッド プールの処理対象にならないすべての要求を実行するスレッド プールです。 このスレッド プールに属するスレッドは、検出、MDX、DAX、DMX、DDL コマンドなど、あらゆる種類の操作を実行します。 A
  
-   **ThreadPool \ IOProcess** は、多次元エンジン内のストレージ エンジン クエリに関連付けられた IO ジョブで使用されます。 これらのスレッドによって実行される作業は、他のスレッドに依存しないことが予期されます。 これらのスレッドは通常、パーティションの単一セグメントをスキャンや、セグメント データに対するフィルター処理と集計を実行します。 **IOProcess** スレッドは、特に NUMA ハードウェア構成の影響を受けます。 したがって、このスレッド プールには、必要に応じてパフォーマンスをチューニングする目的で使用できる **PerNumaNode** 構成プロパティがあります。 
  
-   **ThreadPool \ Process** は、集計、インデックス作成、コミット操作を含む、継続時間の長いストレージ エンジン ジョブを対象にしています。 ROLAP ストレージ モードも、Processing スレッド プールに属するスレッドを使用します。  

- **VertiPaq \ ThreadPool** は、表形式モデルでテーブル スキャンを実行するためのスレッド プールです。
  
 要求を処理する際に、作業の実行に必要とされる場合は、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] はスレッド プールの上限を超えて追加のスレッドを要求することもできます。 ただし、スレッドが自らのタスクの実行を完了した時点で、現在のスレッド数が上限を超えている場合は、そのスレッドはスレッド プールに返される代わりに、単純に終了されます。  
  
> [!NOTE]  
>  特定のデッドロック状態が発生する場合のみ、スレッド プールの最大数を上回ると、保護が開始されます。 最大値を超えるランナウェイ スレッド作成を防止するために、最大値に達した後は (短い遅延の後) スレッドが徐々に作成されます。 スレッドの最大数を超えると、タスクの実行速度が低下する可能性があります。 パフォーマンス カウンターによって示されるスレッド数が常にスレッド プールの最大サイズを上回っている場合は、システムによって要求されるコンカレンシーの度合いに比べてスレッド プールのサイズが小さすぎることを示すインジケーターと見なすこともできます。  
  
 既定では、スレッド プールのサイズは [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]によって決まり、コアの数に基づく値になります。 サーバーの起動後に msmdsrv.log ファイルを参照すると、選択済みの既定値を確認できます。 パフォーマンス チューニングの練習として、クエリまたは処理のパフォーマンスを向上させる目的で、スレッド プールのサイズを大きくする方針を選択することもできます。  
  
##  <a name="bkmk_propref"></a> スレッド プール プロパティに関するリファレンス  
 ここでは、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] の各インスタンスの msmdsrv.ini ファイル内で検索するスレッド プール プロパティについて説明します。 これらのプロパティのサブセットは、SQL Server Management Studio 内でも表示されます。  
  
 プロパティは、アルファベット順に示しています。  
  
|名前|型|説明|既定値|ガイダンス|  
|----------|----------|-----------------|-------------|--------------|  
|**IOProcess** \ **Concurrency**|double|一度にキューに登録できるスレッド数のターゲットを設定するアルゴリズムを決定する、倍精度浮動小数点値です。|2.0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> Concurrency はスレッド プールを初期化するために使用され、それらのプールは Windows 内の IO 完了ポートを使用して実装されます。 詳しくは、「 [I/O 完了ポート](http://msdn.microsoft.com/library/windows/desktop/aa365198\(v=vs.85\).aspx) 」をご覧ください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。IOProcess スレッド プールのスレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **MaxThreads**|ssNoversion|スレッド プールに含めるスレッドの最大数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、サーバーはこの値を 64、または論理プロセッサ数の 10 倍のうち、どちらか大きい方に設定します。 たとえば、ハイパースレッディングが有効な 4 コアのシステムでは、スレッド プールの最大値は 80 スレッドです。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、MaxThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **MinThreads**|ssNoversion|スレッド プールに事前に割り当てるスレッドの最小数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、最小値は 1 です。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **PerNumaNode**|ssNoversion|msmdsrv プロセスを対象にして作成されるスレッド プールの数を決定する、符号付き 32 ビット整数です。|-1|有効な値は、-1、0、1、2 です。<br /><br /> -1 = NUMA ノードの数に基づいて、サーバーが選択する IO スレッド プールの方針が異なります。 NUMA ノード数が 4 未満のシステムでは、サーバーの動作は 0 の場合と同じです (1 つの IOProcess スレッド プールがシステム用に作成されます)。 ノード数が 4 以上のシステムでは、値が 1 の場合の動作と同じです (各ノードに対して IOProcess スレッド プールが作成されます)。<br /><br /> 0 = NUMA ノードごとのスレッド プールを無効にし、msmdsrv.exe プロセスが使用する IOProcess スレッド プールは 1 つだけ存在するようになります。<br /><br /> 1 = NUMA ノードごとに 1 つの IOProcess スレッド プールを有効にします。<br /><br /> 2 = 論理プロセッサごとに 1 つの IOProcess スレッド プール。 各スレッド プール内のスレッドは、論理プロセッサの NUMA ノードに関連付けられ、論理プロセッサに対して最適なプロセッサが設定されます。<br /><br /> 詳細については、「 [NUMA ノード内のプロセッサに IO スレッドを関連付けるための PerNumaNode の設定](#bkmk_pernumanode) 」をご覧ください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **PriorityRatio**|ssNoversion|優先順位の高いキューが空でない場合も、優先順位の低いスレッドが時折実行されるようにする目的で使用できる、32 ビット符号付き整数です。|2|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**IOProcess** \ **StackSizeKB**|ssNoversion|スレッド実行中にメモリ割り当てを調整するために使用できる、符号付き 32 ビット整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> 多次元モデルにのみ適用されます。|  
|**Parsing**  \ **Long** \ **Concurrency**|double|一度にキューに登録できるスレッド数のターゲットを設定するアルゴリズムを決定する、倍精度浮動小数点値です。|2.0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> Concurrency はスレッド プールを初期化するために使用され、それらのプールは Windows 内の IO 完了ポートを使用して実装されます。 詳しくは、「 [I/O 完了ポート](http://msdn.microsoft.com/library/windows/desktop/aa365198\(v=vs.85\).aspx) 」をご覧ください。|  
|**Parsing**  \ **Long** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。解析スレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。|  
|**Parsing**  \ **Long** \ **NumThreads**|ssNoversion|長いコマンドに対して作成可能なスレッドの数を定義する、符号付き 32 ビット整数のプロパティです。|0|0 は、サーバーが既定値を決定することを表します。 既定の動作では、 **NumThreads** を、4 という絶対値、または論理プロセッサ数の 2 倍のうち、どちらか大きい方に設定します。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、NumThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。|  
|**Parsing**  \ **Long** \ **PriorityRatio**|ssNoversion|優先順位の高いキューが空でない場合も、優先順位の低いスレッドが時折実行されるようにする目的で使用できる、32 ビット符号付き整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Parsing**  \ **Long** \ **StackSizeKB**|ssNoversion|スレッド実行中にメモリ割り当てを調整するために使用できる、符号付き 32 ビット整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Parsing**  \ **Short** \ **Concurrency**|double|一度にキューに登録できるスレッド数のターゲットを設定するアルゴリズムを決定する、倍精度浮動小数点値です。|2.0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> Concurrency はスレッド プールを初期化するために使用され、それらのプールは Windows 内の IO 完了ポートを使用して実装されます。 詳しくは、「 [I/O 完了ポート](http://msdn.microsoft.com/library/windows/desktop/aa365198\(v=vs.85\).aspx) 」をご覧ください。|  
|**Parsing**  \ **Short** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。解析スレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。|  
|**Parsing**  \ **Short** \ **NumThreads**|ssNoversion|短いコマンドに対して作成可能なスレッドの数を定義する、符号付き 32 ビット整数のプロパティです。|0|0 は、サーバーが既定値を決定することを表します。 既定の動作では、 **NumThreads** を、4 という絶対値、または論理プロセッサ数の 2 倍のうち、どちらか大きい方に設定します。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、NumThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。|  
|**Parsing**  \ **Short** \ **PriorityRatio**|ssNoversion|優先順位の高いキューが空でない場合も、優先順位の低いスレッドが時折実行されるようにする目的で使用できる、32 ビット符号付き整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Parsing**  \ **Short** \ **StackSizeKB**|ssNoversion|スレッド実行中にメモリ割り当てを調整するために使用できる、符号付き 32 ビット整数です。|64 ×論理プロセッサ数|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Process** \ **Concurrency**|double|一度にキューに登録できるスレッド数のターゲットを設定するアルゴリズムを決定する、倍精度浮動小数点値です。|2.0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> Concurrency はスレッド プールを初期化するために使用され、それらのプールは Windows 内の IO 完了ポートを使用して実装されます。 詳しくは、「 [I/O 完了ポート](http://msdn.microsoft.com/library/windows/desktop/aa365198\(v=vs.85\).aspx) 」をご覧ください。|  
|**Process** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。処理スレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。|  
|**Process** \ **MaxThreads**|ssNoversion|スレッド プールに含めるスレッドの最大数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、サーバーはこの値を、64 という絶対値、または論理プロセッサ数のうち、どちらか大きい方に設定します。 たとえば、ハイパースレッディングが有効な 64 コアのシステム (つまり、128 個の論理プロセッサが存在) では、スレッド プールの最大値は 128 スレッドです。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、MaxThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。|  
|**Process** \ **MinThreads**|ssNoversion|スレッド プールに事前に割り当てるスレッドの最小数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、最小値は 1 です。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。|  
|**Process** \ **PriorityRatio**|ssNoversion|優先順位の高いキューが空でない場合も、優先順位の低いスレッドが時折実行されるようにする目的で使用できる、32 ビット符号付き整数です。|2|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Process** \ **StackSizeKB**|ssNoversion|スレッド実行中にメモリ割り当てを調整するために使用できる、符号付き 32 ビット整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Query**  \ **Concurrency**|double|一度にキューに登録できるスレッド数のターゲットを設定するアルゴリズムを決定する、倍精度浮動小数点値です。|2.0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。<br /><br /> Concurrency はスレッド プールを初期化するために使用され、それらのプールは Windows 内の IO 完了ポートを使用して実装されます。 詳しくは、「 [I/O 完了ポート](http://msdn.microsoft.com/library/windows/desktop/aa365198\(v=vs.85\).aspx) 」をご覧ください。|  
|**Query** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。処理スレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。|  
|**Query**  \ **MaxThreads**|ssNoversion|スレッド プールに含めるスレッドの最大数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、サーバーはこの値を、10 という絶対値、または論理プロセッサ数の 2 倍のうち、どちらか大きい方に設定します。 たとえば、ハイパースレッディングが有効な 4 コアのシステムでは、最大スレッド数は 16 です。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、MaxThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。|  
|**Query** \ **MinThreads**|ssNoversion|スレッド プールに事前に割り当てるスレッドの最小数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、最小値は 1 です。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。<br /><br /> スレッド プールの設定のチューニングに関する詳細については、「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」をご覧ください。|  
|**Query** \ **PriorityRatio**|ssNoversion|優先順位の高いキューが空でない場合も、優先順位の低いスレッドが時折実行されるようにする目的で使用できる、32 ビット符号付き整数です。|2|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**Query**  \ **StackSizeKB**|ssNoversion|スレッド実行中にメモリ割り当てを調整するために使用できる、符号付き 32 ビット整数です。|0|詳細プロパティです。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] サポートの指示がない限り、変更しないでください。|  
|**VertiPaq** \ **CPUs**|ssNoversion|表形式クエリに使用するプロセッサの最大数を指定する、符号付き 32 ビット整数です。|0|0 は、サーバーが既定値を決定することを表します。 既定では、サーバーはこの値を、10 という絶対値、または論理プロセッサ数の 2 倍のうち、どちらか大きい方に設定します。 たとえば、ハイパースレッディングが有効な 4 コアのシステムでは、最大スレッド数は 16 です。<br /><br /> この値を負の値に設定した場合は、サーバーはこの値に論理プロセッサ数を掛けます。 たとえば、32 個の論理プロセッサが存在するサーバーでこの値を -10 に設定すると、最大値は 320 スレッドになります。<br /><br /> 最大値は、以前に定義したカスタムの関係マスクごとに使用可能なプロセッサに適用されます。 たとえば、32 個のプロセッサのうち 8 個を使用するようにスレッド プールの関係を既に設定している場合、MaxThreads を -10 に設定すると、スレッド プールの上限は 10 x 8 で 80 スレッドになります。<br /><br /> このスレッド プールのプロパティで使用する実際の値は、サービスの起動時に msmdsrv log ファイルに書き込まれます。|  
  |**VertiPaq** \ **GroupAffinity**|string|システムのプロセッサ グループに対応する 16 進値の配列です。処理スレッドと各プロセッサ グループの論理プロセッサとの関係を設定するために使用されます。|なし|このプロパティを使用して、カスタム関係を作成することもできます。 既定では、このプロパティは空です。<br /><br /> 詳細については、「 [プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定](#bkmk_groupaffinity) 」をご覧ください。 表形式のみに適用されます。| 
    
##  <a name="bkmk_groupaffinity"></a> プロセッサ グループ内のプロセッサにスレッドを関連付けるための GroupAffinity の設定  
 **GroupAffinity** は、高度なチューニングの目的で用意されています。 **GroupAffinity** プロパティを使用して、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] のスレッド プールと特定のプロセッサ間の関係を設定できます。ただし、ほとんどのインストール環境では、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は、使用可能な論理プロセッサをすべて使用できる場合にパフォーマンスが最も高くなります。 したがって、既定ではグループ関係は指定されていません。  
  
 パフォーマンス テストの結果、CPU 最適化が必要なことが示された場合は、Windows Server リソース マネージャーを使用して論理プロセッサとサーバー プロセスの間で関係を設定するなど、より高度なアプローチを考慮することもできます。 このようなアプローチは、個別のスレッド プールに対してカスタム関係を定義する場合より、実装と管理が容易です。  
  
 このアプローチが不十分な場合は、スレッド プールに対してカスタム関係を定義することで、精度を高めることができます。 多くの場合、関係設定のカスタマイズは、スレッド プールが非常に多くのプロセッサに分散されることによってパフォーマンスの低下が発生する大規模なマルチコア システム (NUMA または NUMA 以外) で推奨されます。 論理プロセッサ数が 64 未満のシステムで **GroupAffinity** を設定することもできますが、利点はごくわずかであり、パフォーマンスが低下する可能性さえあります。  
  
> [!NOTE]  
>  **のエディションごとに使用できるコアの数が制限されており、** GroupAffinity [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]はその制約を受けます。 起動時に、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] はエディション情報と **GroupAffinity** プロパティを使用して、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]が管理する各スレッド プールの関係マスクを計算します。 Standard Edition では最大 24 個のコアを使用できます。 24 個よりも多くのコアを使用する大規模なマルチコア システムに [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] をインストールした場合、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] が使用するコアは 24 個に限られます。 プロセッサの最大値の詳細については、「 [SQL Server 2016 の各エディションでサポートされる機能](https://msdn.microsoft.com/library/cc645993.aspx)」でクロス ボックスのスケール制限を参照してください。  
  
### <a name="syntax"></a>構文  
 この値は、各プロセッサ グループに対応する 16 進数であり、この 16 進数は、特定のスレッド プールに対応するスレッドを割り当てるときに [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] が最初に使用しようとする論理プロセッサを表します。  
  
 **論理プロセッサに対応するビットマスク**  
  
 単一のプロセッサ グループ内で最大 64 個の論理プロセッサを使用できます。 グループ内の各論理プロセッサに対応するビットマスクが 1 (または 0) である場合は、スレッド プールがその論理プロセッサを使用します (または、使用しません)。 ビットマスクを計算した後、 **GroupAffinity**の値として、それに対応する 16 進値を計算します。  
  
 **複数のプロセッサ グループ**  
  
 プロセッサ グループは、システムの起動時に決定されます。 **GroupAffinity** は各プロセッサ グループに対応する 16 進値をコンマ区切りリストの形で受け入れます。 複数のプロセッサ グループ (ハイエンド システムでは最大 10) が存在している場合は、0x0 を指定することにより、個別のグループをバイパスすることができます。 たとえば、4 つのプロセッサ グループ (0、1、2、3) が存在する 1 台のシステムでは、1 番目および 3 番目の値として 0x0 と入力すると、グループ 0 とグループ 2 を除外できます。  
  
 `<GroupAffinity>0x0, 0xFF, 0x0, 0xFF</GroupAffinity>`  
  
### <a name="steps-for-computing-the-processor-affinity-mask"></a>プロセッサの affinity mask を計算する手順  
 msmdsrv.ini 内、または SQL Server Management Studio のサーバー プロパティ ページで、 **GroupAffinity** を設定できます。  
  
1.  **プロセッサ数とプロセッサ グループ数を特定します。**  
  
     [winsysinternals から Coreinfo ユーティリティ](http://technet.microsoft.com/sysinternals/cc835722.aspx)をダウンロードできます。  
  
     **coreinfo** を実行し、Logical Processor to Group Map セクションからこの情報を取得します。 論理プロセッサごとに単独の行が生成されます。  
  
2.  次のように、右から左に向かってプロセッサに番号を付けます。 `7654 3210`  
  
     この例では 8 個のプロセッサ (0 ～ 7) のみを示していますが、1 つのプロセッサ グループは最大 64 の論理プロセッサを使用することができ、1 台のエンタープライズ クラス Windows サーバーで最大 10 個のプロセッサ グループを存在させることができます。  
  
3.  **使用するプロセッサ グループに対応するビットマスクの計算**  
  
     `7654 3210`  
  
     論理プロセッサを除外するか使用するかに応じて、各桁を 0 または 1 で置き換えます。 8 個のプロセッサが存在する 1 台のシステムで、Analysis Services でプロセッサ 7、6、5、4、および 1 を使用する場合は、計算は次のようになります。  
  
     `1111 0010`  
  
4.  **2 進数から 16 進値への変換**  
  
     16 進対応の計算機または変換ツールを使用し、2 進数をそれに対応する 16 進数に変換します。 この例では、 `1111 0010` を `0xF2`に変換します。  
  
5.  **GroupAffinity プロパティに、この 16 進値を入力します。**  
  
     msmdsrv.ini 内、または SQL Server Management Studio のサーバー プロパティ ページで、 **GroupAffinity** を、手順 4 で計算した値に設定します。  
  
> [!IMPORTANT]  
>  **GroupAffinity** の設定は、複数の手順から成る手動のタスクです。 **GroupAffinity**を計算するときは、計算結果を注意深くご確認ください。 マスク全体が無効な場合は [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] からエラーが返されますが、有効な設定と無効な設定が組み合わされている場合は、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] がプロパティを無視する結果になります。 たとえば、ビットマスクに余分な値が含まれている場合は、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] はその設定を無視し、システム上にあるすべてのプロセッサを使用します。 このアクションが発生したことを通知するエラーや警告はありませんが、msmdsrv.log ファイルを調べることで実際の関係の設定方法を確認できます。  
  
##  <a name="bkmk_pernumanode"></a> NUMA ノード内のプロセッサに IO スレッドを関連付けるための PerNumaNode の設定  
 多次元の Analysis Services インスタンスでは、 **IOProcess** スレッド プールに対して **PerNumaNode** を設定して、スレッドのスケジューリングと実行をさらに最適化することができ案す。 **GroupAffinity** は特定のスレッド プールでどの論理プロセッサ セットを使用するかを識別するのに対し、 **PerNumaNode** は複数のスレッド プールを作成するかどうかを指定し、さらに、許容される論理プロセッサの特定のサブセットに対して関連付けることで、1 歩先へ進んだ設定と言えます。  
  
> [!NOTE]  
>  Windows Server 2012 でタスク マネージャーを起動し、コンピューター上の NUMA ノード数を表示します。 タスク マネージャーの [パフォーマンス] タブで、 **[CPU]** を選択し、グラフ領域を右クリックして NUMA ノードを表示します。 もう 1 つの方法として、Windows Sysinternals から Coreinfo ユーティリティを [ダウンロード](http://technet.microsoft.com/sysinternals/cc835722.aspx) し、NUMA ノードと各ノードの論理プロセッサを返す `coreinfo -n` を実行します。  
  
 このトピックの「 **スレッド プール プロパティに関するリファレンス** 」で説明したように、 [PerNumaNode](#bkmk_propref) の有効な値は -1、0、1、2 です。  
  
### <a name="default-recommended"></a>既定 (推奨)  
 NUMA ノードが存在するシステムでは、PerNumaNode=-1 という既定の設定を使用し、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] がノード数に基づいてスレッド プール数とスレッド関係を調整できるようにすることをお勧めします。 システムのノード数が 4 未満の場合、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は **PerNumaNode**=0 で示される動作を実装します。これに対して、 **PerNumaNode**=1 は、4 つ以上のノードが存在するシステムで使用されます。  
  
### <a name="choosing-a-value"></a>値の選択  
 既定値をオーバーライドして別の有効な値を使用することもできます。  
  
 **PerNumaNode=0 の設定**  
  
 NUMA ノードは無視されます。 IOProcess スレッド プールは 1 つしか存在しなくなり、そのスレッド プール内のすべてのスレッドがすべての論理プロセッサに関連付けられます。 既定 (PerNumaNode=-1) では、コンピューターの NUMA ノード数が 4 未満の場合はこれが有効な設定です。  
  
 ![Numa、プロセッサ、およびスレッド プールの一致](../../analysis-services/server-properties/media/ssas-threadpool-numaex0.PNG "Numa、プロセッサ、およびスレッド プールの一致")  
  
 **PerNumaNode=1 の設定**  
  
 NUMA ノードごとに IOProcess スレッド プールが作成されます。 個別のスレッド プールを用意すると、NUMA ノード上のローカル キャッシュなど、ローカル リソースへの連携アクセスが改善されます。  
  
 ![Numa、プロセッサ、およびスレッド プールの一致](../../analysis-services/server-properties/media/ssas-threadpool-numaex1.PNG "Numa、プロセッサ、およびスレッド プールの一致")  
  
 **PerNumaNode=2 の設定**  
  
 この設定を使用するのは、集中的な [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ワークロードを実行している高度なハイエンド システムのみです。 このプロパティは IOProcess スレッド プールの関係を非常に細かなレベルで設定し、個別のスレッド プールを作成して論理プロセッサ レベルで関連付けます。  
  
 次の例では、4 つの NUMA ノード、および 32 個の論理プロセッサが存在する 1 台のシステムで **PerNumaNode** を 2 に設定すると、32 個の IOProcess スレッド プールが作成される結果になります。 最初の 8 個のスレッド プール内にあるスレッドは、いずれも NUMA ノード 0 内の論理プロセッサに関連付けられますが、最適なプロセッサは 0、1、2、...最大で 7 まで設定されます。 続く 8 個のスレッド プールは、いずれも NUMA ノード 1 内の論理プロセッサに関連付けられますが、最適なプロセッサは 8、9、10、...最大で 15 まで設定されます。  
  
 ![Numa、プロセッサ、およびスレッド プールの一致](../../analysis-services/server-properties/media/ssas-threadpool-numaex2.PNG "Numa、プロセッサ、およびスレッド プールの一致")  
  
 この関係レベルでは、スケジューラは必ず、優先 NUMA ノード内にある最適な論理プロセッサを最初に使用しようとします。 その論理プロセッサが使用できない場合は、スケジューラは同じノード内にある別のプロセッサを選択するか、他のスレッドが使用できない場合は同じプロセッサ グループ内にある別のプロセッサを選択します。 詳細と例については、「 [Analysis Services 2012 の構成設定 (Wordpress ブログ)](http://go.microsoft.com/fwlink/?LinkId=330387)」をご覧ください。  
  
###  <a name="bkmk_workdistrib"></a> IOProcess スレッド間での作業の分散  
 **PerNumaNode** プロパティを設定するかどうかを検討するときは、 **IOProcess** スレッドがどのように使用されるのかを理解しておくと、十分な情報に基づいた意思決定を行うことができます。  
  
 **IOProcess** は、多次元エンジン内のストレージ エンジン クエリに関連付けられている IO ジョブで使用されることに注意してください。  
  
 セグメントをスキャンするときに、エンジンはセグメントが属するパーティションを特定し、パーティションで使用されるスレッド プールのキューにセグメント ジョブを登録しようとします。 通常は、パーティションに属するすべてのセグメントが同じスレッド プールのキューにタスクを登録します。 NUMA システムでは、パーティションのすべてのスキャンで、その NUMA ノードにローカルで割り当てられているファイル システム キャッシュのメモリが使用されるため、この動作は特に便利です。  
  
 以下のシナリオに示す調整を行うと、NUMA システムでクエリのパフォーマンスを向上させることができる場合があります。  
  
-   パーティション分割が不十分な (たとえば、単一のパーティションを持つ) メジャー グループでは、パーティションの数を増やします。 パーティションを 1 つだけ使用すると、エンジンは常に 1 つのスレッド プール (スレッド プール 0) のキューにタスクを登録します。 パーティションをさらに追加すると、エンジンは追加のスレッド プールを使用できるようになります。  
  
     追加のパーティションを作成できない場合、スレッド プール 0 で使用できるスレッドの数を増やす 1 つの方法として、 **PerNumaNode**=0 を設定します。  
  
-   セグメント スキャンが複数のパーティションに均等に分散されるデータベースでは、 **PerNumaNode** を 1 または 2 に設定すると、システムで使用する **IOProcess** スレッド プールの総数が増えるため、クエリのパフォーマンスを向上させることができます。  
  
-   複数のパーティションを使用するソリューションで、1 つのパーティションだけが集中的にスキャンされる場合は、 **PerNumaNode**=0 を設定して、パフォーマンスが向上するかどうかを確認します。  
  
 パーティション スキャンとディメンション スキャンでは、いずれも **IOProcess** スレッド プールが使用されますが、ディメンション スキャンではスレッド プール 0 だけが使用されます。 そのため、このスレッド プールにやや不均等な負荷がかかる可能性があります。ただし、ディメンション スキャンは非常に高速で頻度も低い傾向があるため、不均衡は一時的なものです。  
  
> [!NOTE]  
>  サーバー プロパティを変更するときは、現在のインスタンスで実行されているすべてのデータベースに構成オプションが適用されることに注意してください。 最も重要なデータベースまたは最も数が多いデータベースにメリットがもたらされる設定を選択します。 データベース レベルでプロセッサの関係を設定することはできません。また、個々のパーティションと特定のプロセッサ間の関係を設定することもできません。  
  
 ジョブ アーキテクチャの詳細については、「 [SQL Server Analysis Services パフォーマンス ガイド](http://www.microsoft.com/download/details.aspx?id=17303)」のセクション 2.2 をご覧ください。  
  
##  <a name="bkmk_related"></a> 依存プロパティまたは関連プロパティ  
 「 [Analysis Services 操作ガイド](http://msdn.microsoft.com/library/hh226085.aspx)」のセクション 2.4 で説明されているように、処理スレッド プールを増やす場合は、 **CoordinatorExecutionMode** 設定と **CoordinatorQueryMaxThreads** 設定に十分注意し、増やされたスレッド プール サイズを最大限活用できるようにしてください。  
  
 Analysis Services はコーディネーター スレッドを使用して、処理またはクエリの要求を完了するために必要なデータを収集します。 コーディネーターは最初に、各パーティションに対して、操作する必要のある 1 つのジョブをキューに登録します。 その後、パーティション内でスキャンする必要のあるセグメントの総数に応じて、これらのジョブのそれぞれは引き続き他のジョブをキューに登録します。  
  
 **CoordinatorExecutionMode** の既定値は -4 であり、コアあたり 4 つの並列ジョブに制限され、ストレージ エンジン内のサブキューブ要求によって並列実行できるコーディネーター ジョブの総数が制限されることを意味します。  
  
 **CoordinatorQueryMaxThreads** の既定値は 16 であり、各パーティションに対して並列実行できるセグメント ジョブの数が制限されます。  
  
##  <a name="bkmk_currentsettings"></a> 現在のスレッド プールの設定の確認  
 各サービスの起動時に、[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] は現在のスレッド プール設定を msmdsrv.log ファイルに出力し、その中には最小スレッド数と最大スレッド数、プロセッサ関係マスク、およびコンカレンシー数が含まれます。  
  
 次の例は、ログ ファイルからの抽出であり、ハイパースレッディングが有効になっている 4 コアのシステムでの Query スレッド プールに関する既定の設定 (MinThread=0、MaxThread=0、Concurrency=2) を示しています。 関係マスクは 0xFF であり、8 つの論理プロセッサを表しています。 マスクに対して先頭の 0 が追加されることに注意してください。 先頭の 0 は無視できます。  
  
 `"10/28/2013 9:20:52 AM) Message: The Query thread pool now has 1 minimum threads, 16 maximum threads, and a concurrency of 16.  Its thread pool affinity mask is 0x00000000000000ff. (Source: \\?\C:\Program Files\Microsoft SQL Server\MSAS11.MSSQLSERVER\OLAP\Log\msmdsrv.log, Type: 1, Category: 289, Event ID: 0x4121000A)"`  
  
 **MinThread** と **MaxThread** を設定するアルゴリズムが、システム構成、特にプロセッサ数を組み込んでいることに注意してください。 次のブログ記事では、値の計算方法についての洞察を提供します。[Analysis Services 2012 の構成設定 (Wordpress ブログ)](http://go.microsoft.com/fwlink/?LinkId=330387)します。 これらの設定と動作が、それ以降のリリースで調整される可能性があることに注意してください。  
  
 次の一覧に、プロセッサのさまざまな組み合わせを対象にした、他の関係マスクの設定例を示します。  
  
-   8 コアのシステムでプロセッサ 3-2-1-0 のようにすると、このビットマスクが得られます。00001111、および 16 進数値の場合:0xF  
  
-   8 コアのシステムでプロセッサ 7-6-5-4 は、このビットマスクが得られます。11110000、および 16 進数値の場合:0xF0  
  
-   8 コアのシステムでプロセッサ 5-4-3-2 は、このビットマスクが得られます。00111100、および 16 進数値の場合:0x3C  
  
-   8 コアのシステムでプロセッサ 7-6-1-0 は、このビットマスクが得られます。11000011、および 16 進数値の場合:0xC3  
  
 システムに複数のプロセッサ グループが存在する場合は、グループごとに個別の関係マスクが生成され、コンマ区切りリストの形式で表現されることに注意してください。  
  
##  <a name="bkmk_msmdrsrvini"></a> MSMDSRV.INI の概要  
 msmdsrv.ini ファイルには、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] インスタンスの構成設定が含まれます。これらの構成設定は、そのインスタンスで実行されるすべてのデータベースに影響します。 サーバー構成プロパティを使用して、他のすべてのデータベースを除外し、1 つのデータベースのパフォーマンスだけを最適化することはできません。 ただし、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] の複数のインスタンスをインストールし、類似する特性やワークロードを共有するデータベースにメリットをもたらすプロパティを使用するように各インスタンスを構成できます。  
  
 すべてのサーバー構成プロパティが、msmdsrv.ini ファイル内に含まれています。 変更される可能性の高い一部のプロパティは、SSMS のような管理ツール内でも表示されます。  
  
 msmdsrv.ini の内容は、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]のテーブル インスタンスと多次元インスタンスの両方で同一です。 ただし、一部の設定はどちらか 1 つのモードのみに適用されます。 サーバー モードに基づく動作の違いは、プロパティ リファレンスのドキュメントに記載されています。  
  
> [!NOTE]  
>  プロパティの設定方法については、「 [Analysis Services のサーバー プロパティ](../../analysis-services/server-properties/server-properties-in-analysis-services.md)」をご覧ください。  
  
## <a name="see-also"></a>参照  
 [プロセスとスレッドの概要](/windows/desktop/ProcThread/about-processes-and-threads)   
 [複数のプロセッサ](/windows/desktop/ProcThread/multiple-processors)   
 [プロセッサ グループ](/windows/desktop/ProcThread/processor-groups)   
 [SQL Server 2012 での Analysis Services のスレッド プールの変更](http://blogs.msdn.com/b/psssql/archive/2012/01/31/analysis-services-thread-pool-changes-in-sql-server-2012.aspx)   
 [Analysis Services 2012 の構成設定 (Wordpress ブログ)](http://go.microsoft.com/fwlink/?LinkId=330387)   
 [64 を超えるプロセッサを搭載したシステムのサポート](http://msdn.microsoft.com/library/windows/hardware/gg463349.aspx)   
 [SQL Server Analysis Services 操作ガイド](http://go.microsoft.com/fwlink/?LinkID=225539)  
  
  
