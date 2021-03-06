---
title: CREATE KPI ステートメント (MDX) |Microsoft ドキュメント
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2a905c223418392ee9d3bd45dffbfe2ab821a298
ms.sourcegitcommit: 97bef3f248abce57422f15530c1685f91392b494
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34741531"
---
# <a name="mdx-data-definition---create-kpi"></a>MDX データ定義の KPI の作成


  主要業績評価指標 (KPI) を作成します。 KPI は、キューブ内のメジャー グループに関連付けられた計算のコレクションであり、ビジネスやシナリオの成功を評価するために使用します。  
  
## <a name="syntax"></a>構文  
  
```  
  
CREATE KPI CURRENTCUBE | <Cube Name>.KPI_Name AS KPI_Value  
   [,Property_Name = Property_Value, ...n]  
```  
  
## <a name="arguments"></a>引数  
 *Kpi 名*  
 KPI 名を指定する有効な文字列です。  
  
 *KPI_Value*  
 数値を返す有効な多次元式 (MDX) 式です。  
  
 *Property_Name*  
 KPI のプロパティの名前を指定する有効な文字列です。  
  
 *Property_Value*  
 KPI のプロパティの値を定義する有効なスカラー式です。  
  
## <a name="remarks"></a>コメント  
 現在接続しているキューブ以外のキューブを指定すると、エラーになります。 したがって、キューブ名の代わりに CURRENTCUBE を使用して、現在のキューブを確実に指定してください。  
  
## <a name="kpi-properties"></a>KPI のプロパティ  
 次の表は、KPI のすべてのプロパティを示しています。 これらのプロパティに既定値はありません。 したがって、KPI のプロパティに特定の値が割り当てられるまで、そのプロパティに対してクエリを実行すると NULL 値が返されます。  
  
|プロパティの識別子|説明|  
|-------------------------|-------------|  
|GOAL|数値を返す有効な MDX 式です。|  
|STATUS|数値を返す有効な MDX 式です。|  
|STATUS_GRAPHIC|クライアント アプリケーションが使用するグラフィック イメージのセットを定義する文字列です。|  
|TREND|数値を返す有効な MDX 式です。|  
|TREND_GRAPHIC|クライアント アプリケーションが使用するグラフィック イメージのセットを定義する文字列です。|  
|WEIGHT|数値を返す有効な MDX 式です。|  
|CURRENT_TIME_MEMBER|時間ディメンションのメンバーを 1 つ返す有効な MDX 式です。 CURRENT_TIME_MEMBER は、すべての相対時間関数の参照ポイントを設定します。|  
|PARENT_KPI|親 KPI の名前を指定する文字列です。|  
|CAPTION|KPI のキャプションとしてクライアント アプリケーションが使用する文字列です。|  
|DISPLAY_FOLDER|クライアント アプリケーションによって KPI が表示される、表示フォルダーのパスを指定する文字列です。 フォルダー レベルの区切り記号は、クライアント アプリケーションによって定義されます。 ツールおよびクライアントによって提供される[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]、円記号 (\\) は、レベルの区切り記号。 定義済みのメンバーの複数の表示フォルダーを提供するのにには、フォルダーを区切りますセミコロン (;) を使用します。|  
|ASSOCIATED_MEASURE_GROUP|すべての MDX 計算で参照される必要がある、メジャー グループの名前を指定する文字列です。|  
  
 GOAL、STATUS、TREND の各プロパティの値は、-1 ～ 1 の間に評価される MDX 式です。 ただし、これらのプロパティで実際に使用される値の範囲は、クライアント アプリケーションによって定義されます。 ツールおよびクライアントによって提供されるを使用すると[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]Kpi を参照するには、-1 未満の値は-1 として処理、および 1 より大きい値は 1 として扱われます。  
  
 STATUS_GRAPHIC と TREND_GRAPHIC は共に文字列値であり、表示する正しいイメージのセットを識別するために、クライアント アプリケーションが使用します。 これらの文字列は表示関数の動作も定義します。 この動作には、表示する状態の数 (通常は奇数) と、これらの各表示でどのイメージを使用するかなどがあります。  
  
### <a name="kpi-graphics-in-sql-server-data-tools"></a>SQL Server データ ツールでの KPI グラフィック  
 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]、KPI グラフィックに 3 つまたは 5 つのいずれかの状態があります。 次の表は、それぞれの状態の値を定義します。  
  
|KPI グラフィックの状態の数|これらの状態の値|  
|--------------------------------------|---------------------------|  
|3|悪い =-1 ～-0.5<br /><br /> [Ok] を 0.4999-0.4999 を =<br /><br /> 0.50 ～ 1 の良い =|  
|5|悪い = -1 ～ -0.75<br /><br /> リスク = -0.7499 ～ -0.25<br /><br /> OK = -0.2499 ～ 0.2499<br /><br /> 上昇 = 0.25 ～ 0.7499<br /><br /> 良い = 0.75 ～ 1|  
  
> [!NOTE]  
>  反転ゲージや反転した状態の矢印などの一部のグラフィックでは、範囲が逆になります。 つまり、-1 が良い評価、1 が悪い評価になります。  
  
 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] では、KPI グラフィックの名前によって、そのグラフィックの状態が 3 つであるか 5 つであるかを判断できます。 次の表に、使用法、名前、および状態の数を[!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]KPI グラフィックに関連付けます。  
  
|グラフィックの使用法|KPI グラフィックの名前|状態の数|  
|--------------------|-------------------------|----------------------|  
|Status|図形|3|  
|Status|信号機|3|  
|Status|道路標識|3|  
|Status|ゲージ|3|  
|Status|反転ゲージ|5|  
|Status|温度計|3|  
|Status|シリンダー|3|  
|Status|外観|3|  
|Status|変位の矢印|3|  
|傾向|標準の矢印|3|  
|傾向|状態の矢印|3|  
|傾向|反転した状態の矢印|5|  
|傾向|外観|3|  
  
## <a name="see-also"></a>参照  
 [DROP KPI ステートメント&#40;MDX&#41;](../mdx/mdx-data-definition-drop-kpi.md)   
 [MDX データ定義ステートメント&#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
  
