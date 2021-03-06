---
title: sp_xml_preparedocument (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_xml_preparedocument_TSQL
- sp_xml_preparedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_preparedocument
ms.assetid: 95f41cff-c52a-4182-8ac6-bf49369d214c
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e89ebe73f7bee6f44df353afca515aca4cbbdcf2
ms.sourcegitcommit: f62f70298651d6223fa5d215b6a7a0d2ffecbd0d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2018
ms.locfileid: "51947623"
---
# <a name="spxmlpreparedocument-transact-sql"></a>sp_xml_preparedocument (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  入力値として提供された XML テキストを読み取り、MSXML パーサー (Msxmlsql.dll) を使用して解析し、使用できる状態の解析済みドキュメントを提供します。 この解析済みのドキュメントでは、XML ドキュメント内の各種ノード (要素、属性、テキスト、コメントなど) がツリー形式で表示されます。  
  
 **sp_xml_preparedocument** XML ドキュメントの新しく作成された内部表現へのアクセスに使用できるハンドルを返します。 このハンドルは、セッションまたは実行することによって、ハンドルが無効になるまでの期間の有効な**sp_xml_removedocument**します。  
  
> [!NOTE]  
>  解析済みのドキュメントは、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] の内部キャッシュに格納されます。 MSXML パーサーでは、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] に割り当てられている総メモリの 8 分の 1 が使用されます。 メモリ不足を回避するには、実行**sp_xml_removedocument**メモリを解放します。  
  
> [!NOTE]  
>  旧バージョンと互換性のため、 **sp_xml_preparedocument** (char(13)) と LF (属性の場合、これらの文字はエンティティ化でも char(10)) 文字 CR を折りたたみます。  
  
> [!NOTE]  
>  によって呼び出された XML パーサー **sp_xml_preparedocument**内部 Dtd およびエンティティの宣言を解析できます。 悪意を持って作成された Dtd およびエンティティ宣言を使用するには、サービス拒否攻撃を実行する、ユーザーは、信頼されていないソースから XML ドキュメントを直接渡すを強くお勧め**sp_xml_preparedocument**します。  
>   
>  再帰的なエンティティ拡張攻撃を軽減するために**sp_xml_preparedocument**ドキュメントの最上位レベルで 1 つのエンティティの下で展開できるエンティティの数を 10,000 に制限します。 この制限は、文字エンティティまたは数値エンティティには適用されません。 この制限により、多数のエンティティ参照があるドキュメントを格納できるようになりますが、エンティティを再帰的に展開して、単一のエンティティで 10,000 個の展開より長いチェーンにすることはできなくなります。  
  
> [!NOTE]  
>  **sp_xml_preparedocument** 256 に同時に開くことができる要素の数を制限します。  

 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```  
  
sp_xml_preparedocument  
hdoc   
OUTPUT  
[ , xmltext ]  
[ , xpath_namespaces ]   
```  
  
## <a name="arguments"></a>引数  
 *hdoc*  
 新しく作成されたドキュメントへのハンドルを指定します。 *hdoc*は整数です。  
  
 [ *xmltext* ]  
 元の XML ドキュメントを指定します。 MSXML パーサーではこの XML ドキュメントが解析されます。 *xmltext*テキスト パラメーターです: **char**、 **nchar**、 **varchar**、 **nvarchar**、**テキスト**、 **ntext**または**xml**します。 既定値は NULL です。NULL の場合、空の XML ドキュメントの内部表現が作成されます。  
  
> [!NOTE]  
>  **sp_xml_preparedocument**テキストまたは型指定されていない XML だけを処理できます。 入力値として使用するインスタンス値が既に型指定された XML である場合は、まずその値を、型指定されない新しい XML インスタンスまたは文字列にキャストし、その後入力値として渡します。 詳細については、「 [型指定された XML と型指定されていない XML の比較](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)」を参照してください。  
  
 [ *xpath_namespaces* ]  
 OPENXML の XPath 式の行および列で使用される名前空間宣言を指定します。 *xpath_namespaces*テキスト パラメーターです: **char**、 **nchar**、 **varchar**、 **nvarchar**、**テキスト**、 **ntext**または**xml**します。  
  
 既定値は **\<root xmlns:mp="urn:schemas-microsoft-com:xml-metaprop">** します。 *xpath_namespaces*ウェルフォームド XML ドキュメントを使用して、OPENXML の XPath 式で使用されるプレフィックスの名前空間 Uri を提供します。 *xpath_namespaces*名前空間を参照するために使用する必要がありますプレフィックスを宣言します**urn:schemas-microsoft-com:xml-metaprop**; この解析された XML 要素に関するメタデータを提供します。 この方法を使用して、メタプロパティ名前空間に対する名前空間プレフィックスを再定義できますが、再定義してもこの名前空間が失われることはありません。 プレフィックス**mp**に対して引き続き有効**urn:schemas-microsoft-com:xml-metaprop**場合でも*xpath_namespaces*このような宣言が含まれていません。  
  
## <a name="return-code-values"></a>リターン コードの値  
 0 (成功) または >0 (失敗)  
  
## <a name="permissions"></a>アクセス許可  
 ロール **public** のメンバーシップが必要です。  
  
## <a name="examples"></a>使用例  
  
### <a name="a-preparing-an-internal-representation-for-a-well-formed-xml-document"></a>A. ウェルフォームド XML ドキュメントの内部表現を準備する  
 次の例では、入力値として提供されている、新しく作成された XML ドキュメントの内部表現へのハンドルを返します。 `sp_xml_preparedocument` の呼び出しでは、既定の名前空間プレフィックス マッピングを使用します。  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
-- Remove the internal representation.  
exec sp_xml_removedocument @hdoc;  
```  
  
### <a name="b-preparing-an-internal-representation-for-a-well-formed-xml-document-with-a-dtd"></a>B. ウェルフォームド XML ドキュメントの内部表現を DTD を使用して準備する  
 次の例では、入力値として提供されている、新しく作成された XML ドキュメントの内部表現へのハンドルを返します。 このストアド プロシージャでは、読み込まれたドキュメントが、ドキュメント内に含まれる DTD に対して検証されます。 `sp_xml_preparedocument` の呼び出しでは、既定の名前空間プレフィックス マッピングを使用します。  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(2000);  
SET @doc = '  
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE root   
[<!ELEMENT root (Customers)*>  
<!ELEMENT Customers EMPTY>  
<!ATTLIST Customers CustomerID CDATA #IMPLIED ContactName CDATA #IMPLIED>]>  
<root>  
<Customers CustomerID="ALFKI" ContactName="Maria Anders"/>  
</root>';  
  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
```  
  
### <a name="c-specifying-a-namespace-uri"></a>C. 名前空間 URI を指定する  
 次の例では、入力値として提供されている、新しく作成された XML ドキュメントの内部表現へのハンドルを返します。 呼び出し`sp_xml_preparedocument`保持、`mp`メタプロパティ名前空間マッピング プレフィックスし、追加、`xyz`名前空間マッピング プレフィックス`urn:MyNamespace`します。  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc, '<ROOT xmlns:xyz="urn:MyNamespace"/>';  
```  
  
## <a name="see-also"></a>参照  
 <br>[XML ストアド Procedures(Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[システム ストアド Procedures(Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[OPENXML(Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
 <br>[sys.dm_exec_xml_handles (TRANSACT-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_removedocument (TRANSACT-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)
  
  
