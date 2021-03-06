---
title: Azure Data Studio で notebook を管理します。
titleSuffix: SQL Server 2019 big data clusters
description: Azure Data Studio でノートブックを管理する方法について説明します。 これは、保存、およびビッグ データ クラスター接続を変更するノートブックを開いてが含まれます。
author: rothja
ms.author: jroth
manager: craigg
ms.date: 12/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.custom: seodec18
ms.openlocfilehash: 107a567da4727fa5786b0b913f1c75706a23a9b7
ms.sourcegitcommit: 202ef5b24ed6765c7aaada9c2f4443372064bd60
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2019
ms.locfileid: "54241927"
---
# <a name="how-to-manage-notebooks-in-azure-data-studio"></a>Azure Data Studio でノートブックを管理する方法

この記事を開き、SQL Server 2019 プレビューで、Azure Data Studio でノートブック ファイルを保存する方法を示します。 SQL Server、ビッグ データ クラスターへの接続を変更する方法も示します。

## <a name="prerequisites"></a>前提条件

この記事で Azure Data Studio を使用するノートブック済みであることを前提としています。 ノートブックを作成する場合は、「 [SQL Server 2019 プレビューで notebook を使用する方法](notebooks-guidance.md)します。 Notebook を使用して、Azure Data Studio で、次の前提条件を満たす必要があります。

- [ビッグ データ クラスター デプロイ](quickstart-big-data-cluster-deploy.md)します。
- [SQL Server 2019 ビッグ データ ツール](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **SQL Server 2019 の拡張機能**
   - **kubectl**

## <a name="open-a-notebook"></a>ノートブックを開く

いくつかの方法を開く、 **Notebook を開く**ダイアログ。 [ファイル] メニューのダッシュ ボード、およびコマンド パレットを使用することができます。 次のセクションでは、各メソッドについて説明します。

### <a name="file-menu"></a>[ファイル] メニュー

選択**ファイルを開く**ファイル メニュー (Windows) では、Ctrl + O と Cmd + O (Mac) にします。

![ファイルを開くを選択してファイルを開く ダイアログを開きます](./media/notebooks-how-to-manage/open-file-1.png) 

### <a name="dashboard"></a>ダッシュボード

クリックして**Notebook を開く**ダッシュ ボード ファイルを開く ダイアログを開きます。

![Notebook を開いて、ダッシュ ボードを選択してファイルを開く ダイアログを開きます](./media/notebooks-how-to-manage/open-file-2.png) 

### <a name="command-palette"></a>コマンド パレット

コマンドを使用して**ファイル。開いている**(Windows) で Ctrl + Shift + P と Cmd + Shift + P (Mac) で入力してコマンド パレットから。

![コマンド パレットで File:Open を入力してファイルを開く ダイアログを開きます](./media/notebooks-how-to-manage/open-file-3.png)

## <a name="save-a-notebook"></a>ノートブックを保存します。

現在は、notebook を保存する方法の 1 つ。 選択する必要があります**保存**notebook ツールバーから。

![Notebook のツールバーで保存 をクリックしてファイルを保存します。](./media/notebooks-how-to-manage/save-file-1.png)

> [!NOTE]
> 次のメソッドは、現在変更を保存しないのノートブックに。
>
> - **ファイルを保存**、**ファイルの名前を付けて保存.** と**ファイルすべて保存**ファイル メニューからコマンド。
> - **ファイル:保存**コマンド パレットで入力したコマンド。

## <a name="change-the-big-data-cluster"></a>ビッグ データ クラスターを変更します。

ノートブックの SQL Server のビッグ データ クラスターを変更するには。

1. をクリックして、**にアタッチ**notebook ツールバーからメニュー。

   ![Notebook のツールバーのメニューにアタッチ をクリックします。](./media/notebooks-how-to-manage/select-attach-to-1.png)

2. サーバーをクリックして、**にアタッチ**メニュー。

   ![メニューにアタッチからサーバーを選択します。](./media/notebooks-how-to-manage/select-attach-to-2.png)

## <a name="next-steps"></a>次の手順

Azure Data Studio で notebook の詳細については、次を参照してください。 [SQL Server 2019 プレビューで notebook を使用する方法](notebooks-guidance.md)します。