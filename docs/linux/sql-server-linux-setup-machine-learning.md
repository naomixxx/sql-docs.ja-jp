---
title: Learning サービス (R、Python、Java) を Linux 上の SQL Server マシンのインストール |Microsoft Docs
description: この記事では、Red Hat、Ubuntu 上の SQL Server Machine Learning サービス (R、Python、Java) をインストールする方法を説明します。
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.date: 12/07/2018
ms.topic: conceptual
ms.prod: sql
ms.custom: sql-linux
ms.technology: machine-learning
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 15a1a411672303fc8556927bcaf218052758744d
ms.sourcegitcommit: 2f5773f4bc02bfff4f2924226ac5651eb0c00924
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2018
ms.locfileid: "53553254"
---
# <a name="install-sql-server-2019-machine-learning-services-r-python-java-on-linux"></a>SQL Server 2019 の Machine Learning サービス (R、Python、Java) Linux 上のインストールします。

[SQL Server Machine Learning Services](../advanced-analytics/what-is-sql-server-machine-learning.md)以降の SQL Server 2019 このプレビュー リリースでは Linux オペレーティング システム上で動作します。 Java プログラミング拡張機能、または R と Python の拡張機能を学習するコンピューターにインストールするには、この記事の手順に従います。 

機械学習やプログラミング拡張機能は、データベース エンジンへのアドオンです。 できますが、[データベース エンジンと Machine Learning サービスを同時にインストール](#install-all)、インストールして詳細を追加する前に、問題を解決できるように、まず、SQL Server データベース エンジンを構成することをお勧めコンポーネント。 

R、Python、および Java 拡張機能パッケージの場所は、SQL Server Linux ソース リポジトリでいます。 実行することができます、データベース エンジンのインストールのソース リポジトリが既に構成されている場合、 **mssql mlservices**同じリポジトリの登録を使用して、インストール コマンドをパッケージ化します。

## <a name="uninstall-previous-ctp"></a>以前の CTP をアンインストールします。

パッケージ一覧は、最近 CTP のリリース、結果としてパッケージ数が少ない経由で変更されました。 CTP 2.2 以降をインストールする前に、前のすべてのパッケージを削除するには、CTP 2.0 または 2.1 をアンインストールすることをお勧めします。 複数のバージョンのサイド バイ サイドでインストールがサポートされていません。

### <a name="1-confirm-package-installation"></a>1.パッケージのインストールを確認します。

最初の手順として以前のインストールの有無を確認することがあります。 次のファイルは、既存のインストールを示す: checkinstallextensibility.sh、exthost、スタート パッド。

```bash
ls /opt/microsoft/mssql/bin
```

### <a name="2-uninstall-ctp-20-or-21-packages"></a>2.CTP 2.0 または 2.1 パッケージをアンインストールします。

最小のパッケージ レベルでアンインストールします。 下位レベルのパッケージに依存するすべてのアップ ストリーム パッケージが自動的にアンインストールされます。

  + R の統合、削除**microsoft オープン r***
  + Python の統合、削除**mssql-mlservices-python**
  + Java 統合の場合は、削除**mssql server extensibility java**

パッケージを削除するためのコマンドは、次の表に表示されます。

| プラットフォーム  | パッケージの削除コマンド | 
|-----------|----------------------------|
| RHEL  | `sudo yum remove microsoft-r-open-mro-3.4.4`<br/>`sudo yum remove msssql-mlservices-python`<br/>`sudo yum remove msssql-server-extensibility-java` |
| SLES  | `sudo zypper remove microsoft-r-open-mro-3.4.4`<br/>`sudo zypper remove msssql-mlservices-python`<br/>`sudo zypper remove msssql-server-extensibility-java` |
| Ubuntu    | `sudo apt-get remove microsoft-r-open-mro-3.4.4`<br/>`sudo apt-get remove msssql-mlservices-python`<br/>`sudo apt-get remove msssql-server-extensibility-java`|

> [!Note]
> Microsoft R Open は、3 つのパッケージで構成されています。 Microsoft の r のオープン-mro-3.4.4 を削除した後のこれらのパッケージが残っている場合は、個別に削除してください。
> ```
> microsoft-r-open-foreachiterators-3.4.4
> microsoft-r-open-mkl-3.4.4
> microsoft-r-open-mro-3.4.4
> ```

### <a name="3-proceed-with-ctp-22-install"></a>3.CTP 2.2 のインストールを続行します。

この記事の手順を使用して、オペレーティング システムの最上位のパッケージ レベルでインストールします。

各 OS 固有のインストール手順については、一連の*最上位のパッケージ レベル*か**例 1 - フル インストール**、パッケージの完全なセットまたは**例 2 - 最小限のインストール**最小限の数の実行可能なインストールに必要なパッケージです。

1. R の統合、まず[MRO](#mro)の前提条件があるためです。 R 統合しなくてもインストールされません。

2. パッケージ マネージャーと構文を使用して、オペレーティング システムのインストール コマンドを実行します。 

   + [Red Hat](#RHEL)
   + [Ubuntu](#ubuntu)
   + [SUSE](#SUSE)

## <a name="prerequisites"></a>前提条件

+ Linux バージョンである必要があります[SQL Server でサポートされている](sql-server-linux-release-notes-2019.md#supported-platforms)、オンプレミス、または Docker コンテナーで実行中です。 サポートされているバージョンは次のとおりです。

   + [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)

   + [SUSE Enterprise Linux Server](quickstart-install-connect-suse.md)

   + [Ubuntu](quickstart-install-connect-ubuntu.md)

+ (R の場合のみ)[Microsoft R Open](#mro)基本の R ディストリビューションの SQL Server で R の機能を提供します。

+ T-SQL コマンドを実行するためのツールが必要です。 クエリ エディターは、インストール後の構成と検証の必要があります。 お勧め[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download?view=sql-server-2017#get-azure-data-studio-for-linux)Linux で実行されている無料でダウンロードします。

<a name="mro"></a>

### <a name="microsoft-r-open-mro-installation"></a>Microsoft R Open (MRO) のインストール

Microsoft の基本ディストリビューションの R は、RevoScaleR、MicrosoftML、および Machine Learning サービスでインストールされているその他の R パッケージを使用するための前提条件です。

必要なバージョンは、MRO 3.4.4 です。

MRO にインストールする次の 2 つの方法から選択します。

+ MRAN から MRO tarball をダウンロード、展開、および、install.sh スクリプトを実行します。 利用できる、[インストール手順については、MRAN](https://mran.microsoft.com/releases/3.4.4)このアプローチの場合。

+ または、登録、 **packages.microsoft.com** MRO 配布を構成する 3 つのパッケージをインストールする以下のようにリポジトリ: microsoft r オープン mro、microsoft r のオープン mkl とmicrosoft r のオープン foreachiterators します。 

次のコマンドは、MRO を提供するリポジトリを登録します。 登録後、mssql mlservices-mml r などの他の R パッケージをインストールするためのコマンドにより、パッケージの依存関係として MRO は自動的に含めます。

#### <a name="mro-on-ubuntu"></a>Ubuntu で MRO

```bash
# Install as root
sudo su

# Optionally, if your system does not have the https apt transport option
apt-get install apt-transport-https

# Add the **azure-cli** repo to your apt sources list
AZ_REPO=$(lsb_release -cs)

echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | sudo tee /etc/apt/sources.list.d/azure-cli.list

# Set the location of the package repo the "prod" directory containing the distribution.
# This example specifies 16.04. Replace with 14.04 if you want that version
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

# Register the repo
dpkg -i packages-microsoft-prod.deb

# Update packages on your system (required), including MRO installation
sudo apt-get update
```

#### <a name="mro-on-rhel"></a>RHEL で MRO

```bash
# Import the Microsoft repository key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Create local `azure-cli` repository
sudo sh -c 'echo -e "[azure-cli]\nname=Azure CLI\nbaseurl=https://packages.microsoft.com/yumrepos/azure-cli\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'

# Set the location of the package repo at the "prod" directory
# The following command is for version 7.x
# For 6.x, replace 7 with 6 to get that version
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Update packages on your system (optional)
yum update
```
#### <a name="mro-on-suse"></a>SUSE に MRO

```bash
# Install as root
sudo su

# Set the location of the package repo at the "prod" directory containing the distribution
# This example is for SLES12, the only supported version of SUSE in Machine Learning Server
zypper ar -f https://packages.microsoft.com/sles/12/prod packages-microsoft-com

# Update packages on your system (optional)
zypper update
```

## <a name="package-list"></a>パッケージの一覧

インターネットに接続されたデバイスで、パッケージがダウンロードされ、各オペレーティング システム、パッケージ インストーラーを使用して、データベース エンジンとは別にインストールされています。 次の表に、すべての利用可能なパッケージが、R と Python の場合は、完全な機能のインストールまたは最小機能のインストールのいずれかを提供するパッケージを指定します。

| パッケージ名 | 適用先 | 説明 |
|--------------|----------|-------------|
|mssql-サーバーの機能拡張  | All | 機能拡張フレームワークが R、Python、または Java コードを実行するために使用します。 |
|mssql server extensibility java | Java | Java の実行環境を読み込むための Java 拡張機能。 その他のライブラリや Java のパッケージはありません。 |
| microsoft openmpi  | Python、R | メッセージは Linux での並列処理の Revo のライブラリによって使用されるインターフェイスを渡します。 |
| mssql-mlservices-python | Python | Anaconda と Python のオープン ソース ディストリビューション。 |
|mssql mlservices mlm py  | Python | *完全なインストール*します。 Revoscalepy、microsoftml、事前トレーニング済みの画像の特徴の生成とテキストのセンチメント分析のモデルを提供します。| 
|mssql mlservices パッケージ py  | Python | *最小インストールによって*します。 Microsoftml revoscalepy を提供します。 <br/>事前トレーニング済みモデルを除外します。 | 
| [microsoft の r のオープン *](#mro) | R | R のオープン ソース ディストリビューションは、3 つのパッケージで構成されます。 |
|mssql mlservices mlm r  | R | *完全なインストール*します。 第 sqlRUtils RevoScaleR、MicrosoftML、olapR、事前トレーニング済みの画像の特徴の生成とテキストのセンチメント分析のモデルを提供します。| 
|mssql mlservices パッケージ r  | R | *最小インストールによって*します。 RevoScaleR、sqlRUtils、MicrosoftML、olapR を提供します。 <br/>事前トレーニング済みモデルを除外します。 | 
|mssql mlservices mml py  | CTP 2.0 2.1 のみ | Mssql-mslservices-python を Python パッケージの統合により、CTP 2.2 で廃止します。 Revoscalepy を提供します。 事前トレーニング済みモデルと microsoftml を除外します。| 
|mssql mlservices mml r  | CTP 2.0 2.1 のみ | Mssql-mslservices python に R パッケージの統合により、CTP 2.2 で廃止します。 RevoScaleR、sqlRUtils、olapR を提供します。 事前トレーニング済みモデルと MicrosoftML を除外します。  |

<a name="RHEL"></a>

## <a name="rhel-commands"></a>RHEL コマンド

言語サポートをインストールすることができます (1 つまたは複数の言語) に必要なすべての組み合わせでします。 R と Python の場合は、選択できる 2 つのパッケージがあります。 特徴として、すべての使用可能な機能は、1 つ、*完全インストール*します。 代替の選択肢は、事前トレーニング済みの機械学習モデルを除外しては見なされません、*最小インストール*します。

> [!Tip]
> 実行可能であれば、`yum clean all`をインストールする前に、システム上のパッケージを更新します。

### <a name="example-1----full-installation"></a>例 1: フル インストール 

R と Python のオープン ソース R および Python に extensibility framework、microsoft openmpi、拡張機能 (R、Python、Java) を machine learning ライブラリと事前トレーニング済みモデルが含まれています。 

```bash
# Install as root or sudo
# Add everything (all R, Python, Java)
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-mlm-py-9.4.6*
sudo yum install mssql-mlservices-mlm-r-9.4.6* 
sudo yum install mssql-server-extensibility-java
```

### <a name="example-2---minimum-installation"></a>例 2: 最小インストール 

R および Python と Java 拡張機能のオープン ソース R および Python に extensibility framework、microsoft openmpi、core Revo * ライブラリと機械学習ライブラリが含まれています。 事前トレーニング済みモデルを除外します。

```bash
# Install as root or sudo
# Minimum install of R, Python, Java extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-packages-py-9.4.6*
sudo yum install mssql-mlservices-packages-r-9.4.6*
sudo yum install mssql-server-extensibility-java
```

<a name="ubuntu"></a>

## <a name="ubuntu-commands"></a>Ubuntu のコマンド

言語サポートをインストールすることができます (1 つまたは複数の言語) に必要なすべての組み合わせでします。 R と Python の場合は、選択できる 2 つのパッケージがあります。 特徴として、すべての使用可能な機能は、1 つ、*完全インストール*します。 代替の選択肢は、事前トレーニング済みの機械学習モデルを除外しては見なされません、*最小インストール*します。

> [!Tip]
> 実行可能であれば、`apt-get update`をインストールする前に、システム上のパッケージを更新します。 さらに、Ubuntu のいくつかの docker イメージでは、https の apt トランスポート オプションがあります。 これをインストールするには使用`apt-get install apt-transport-https`します。

<!---
### Prerequisite for 18.04

Running mssql-mlservices R libraries on Ubuntu 18.04 requires **libpng12** from the Linux Kernel archives. This package is no longer included in the standard distribution and must be installed manually. To get this library, run the following commands:

```bash
wget https://mirrors.kernel.org/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1_amd64.deb
dpkg -i libpng12-0_1.2.54-1ubuntu1_amd64.deb
```--->

### <a name="example-1----full-installation"></a>例 1: フル インストール 

R と Python のオープン ソース R および Python に extensibility framework、microsoft openmpi、拡張機能 (R、Python、Java) を machine learning ライブラリと事前トレーニング済みモデルが含まれています。 

```bash
# Install as root or sudo
# Add everything (all R, Python, Java)
# There is no asterisk in this full install
sudo apt-get install mssql-mlservices-mlm-py 
sudo apt-get install mssql-mlservices-mlm-r 
sudo apt-get install mssql-server-extensibility-java
```

### <a name="example-2---minimum-installation"></a>例 2: 最小インストール 

R および Python と Java 拡張機能のオープン ソース R および Python に extensibility framework、microsoft openmpi、core Revo * ライブラリと機械学習ライブラリが含まれています。 事前トレーニング済みモデルを除外します。 

```bash
# Install as root or sudo
# Minimum install of R, Python, Java
# No aasterisk
sudo apt-get install mssql-mlservices-packages-py
sudo apt-get install mssql-mlservices-packages-r
sudo apt-get install mssql-server-extensibility-java
```

<a name="suse"></a>

## <a name="suse-commands"></a>SUSE コマンド

言語サポートをインストールすることができます (1 つまたは複数の言語) に必要なすべての組み合わせでします。 R と Python の場合は、選択できる 2 つのパッケージがあります。 特徴として、すべての使用可能な機能は、1 つ、*完全インストール*します。 代替の選択肢は、事前トレーニング済みの機械学習モデルを除外しては見なされません、*最小インストール*します。

### <a name="example-1----full-installation"></a>例 1: フル インストール 

R と Python のオープン ソース R および Python に extensibility framework、microsoft openmpi、拡張機能 (R、Python、Java) を machine learning ライブラリと事前トレーニング済みモデルが含まれています。 

```bash
# Install as root or sudo
# Add everything (all R, Python, Java)
# Be sure to include -9.4.6* in mlsservices package names
sudo zypper install mssql-mlservices-mlm-py-9.4.6*
sudo zypper install mssql-mlservices-mlm-r-9.4.6* 
sudo zypper install mssql-server-extensibility-java
```

### <a name="example-2---minimum-installation"></a>例 2: 最小インストール 

R および Python と Java 拡張機能のオープン ソース R および Python に extensibility framework、microsoft openmpi、core Revo * ライブラリと機械学習ライブラリが含まれています。 事前トレーニング済みモデルを除外します。 

```bash
# Install as root or sudo
# Minimum install of R, Python, Java extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo zypper install mssql-mlservices-packages-py-9.4.6*
sudo zypper install mssql-mlservices-packages-r-9.4.6*
sudo zypper install mssql-server-extensibility-java
```

## <a name="post-install-config-required"></a>インストール後の構成 (必須)

追加の構成は、主に、 [mssql-conf ツール](sql-server-linux-configure-mssql-conf.md)します。


1. SQL Server サービスを実行するために使用する mssql ユーザー アカウントを追加します。 以前のセットアップを実行していない場合に必要です。

  ```bash
  sudo /opt/mssql/bin/mssql-conf setup
  ```

2. オープン ソース R と Python のライセンス契約に同意します。 これを行ういくつかの方法はあります。 以前 SQL Server ライセンスを受け入れ、R または Python の拡張機能を追加するようになりましたすると、次のコマンドは、その条項に同意は。

  ```bash
  # Run as SUDO or root
  # Use set + EULA 
    sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y
  ```

  別のワークフローは、SQL Server データベース エンジンの使用許諾契約書を許可していない場合は、セットアップによって検出されたこと、mssql mlservices パッケージおよび EULA 同意のプロンプト時に`mssql-conf setup`を実行します。 使用許諾契約書パラメーターに関する詳細については、次を参照してください。 [mssql-conf ツールで SQL Server の構成](sql-server-linux-configure-mssql-conf.md#mlservices-eula)します。

3. R の機能の統合のみ、設定、 **MKL_CBWR**環境変数を[出力を一貫性のある](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)Intel 数値演算ライブラリ (MKL) 計算から。

  + という名前のファイルを編集または **.bash_profile** 、ユーザーのホーム ディレクトリ内の行を追加する`export MKL_CBWR="AUTO"`ファイルにします。

  + このファイルを入力して実行`source .bash_profile`bash コマンド プロンプトでします。

4. SQL Server スタート パッド サービスとデータベース エンジンのインスタンスを再起動します。 

  ```bash
  systemctl restart mssql-launchpadd

  systemctl restart mssql-server.service
  ```

5. Azure Data Studio または SQL Server Management Studio (Windows のみ) などの別のツールを使用して外部スクリプトの実行を有効にする Transact SQL を実行します。 

  ```bash
  EXEC sp_configure 'external scripts enabled', 1 
  RECONFIGURE WITH OVERRIDE 
  ```

6. スタート パッド サービスを再起動します。

## <a name="verify-installation"></a>インストールの確認

R ライブラリ (MicrosoftML、RevoScaleR、およびその他のユーザー) をご覧`/opt/mssql/mlservices/libraries/RServer`します。

Python ライブラリ (microsoftml および revoscalepy) をご覧`/opt/mssql/mlservices/libraries/PythonServer`します。

Java と機能の統合は、ライブラリは含まれませんが、実行することができます`grep -r JAVA_HOME /etc`JAVA_HOME 環境変数の作成を確定します。

インストールを確認するには、R または Python の呼び出しのシステム ストアド プロシージャを実行する T-SQL スクリプトを実行します。 このタスクは、クエリ ツールを必要があります。 Azure Data Studio をお勧めします。 その他のよく使用されるツールは、SQL Server Management Studio や PowerShell など Windows 専用です。 これらのツールを Windows コンピューターがある場合は、データベース エンジンの Linux インストールへの接続に使用します。

SQL Server で R の実行をテストする次の SQL コマンドを実行します。 スクリプトが実行されない場合は、サービスの再起動をお試しください`sudo systemctl restart mssql-server.service`します。

```r
EXEC sp_execute_external_script   
@language =N'R', 
@script=N' 
OutputDataSet <- InputDataSet', 
@input_data_1 =N'SELECT 1 AS hello' 
WITH RESULT SETS (([hello] int not null)); 
GO 
```
 
SQL Server での Python の実行をテストする次の SQL コマンドを実行します。 
 
```python
EXEC sp_execute_external_script  
@language =N'Python', 
@script=N' 
OutputDataSet = InputDataSet; 
', 
@input_data_1 =N'SELECT 1 AS hello' 
WITH RESULT SETS (([hello] int not null)); 
GO 
```

<a name="install-all"></a>

## <a name="chained-combo-install"></a>連鎖「コンボ」のインストールします。

インストールして、1 つのプロシージャで R、Python、または Java のパッケージとデータベース エンジンをインストールするコマンドのパラメーターを追加して、データベース エンジンと Machine Learning サービスを構成することができます。 

1. R の統合、インストール[Microsoft R Open](#mro)前提条件として。 R の機能をインストールしない場合は、この手順をスキップします。

2. データベース エンジン、および言語拡張機能が含まれるコマンドラインを提供します。

  データベース エンジンへの統合をインストールする Java などの 1 つの機能を追加することができます。

  ```bash
  sudo yum install -y mssql-server mssql-server-extensibility-java 
  ```

  または、すべての拡張機能 (Java、R、Python) を追加します。

  ```bash
  sudo yum install -y mssql-server mssql-server-extensibility-java mssql-mlservices-packages-r-9.4.6* mssql-mlservices-packages-py-9.4.6*
  ```

3. ライセンス契約に同意し、インストール後の構成を完了します。 使用して、 **mssql conf**このタスクのためのツール。

  ```bash
  sudo /opt/mssql/bin/mssql-conf setup
  ```

  データベース エンジンのライセンス契約に同意し、エディションを選択して、管理者パスワードを設定するメッセージが表示されます。 Machine Learning Services のライセンス契約に同意することも求められます。

4. サービスを再起動するように求められた場合。

  ```bash
  sudo systemctl restart mssql-server.service
  ```

## <a name="unattended-installation"></a>無人インストール

使用して、[無人インストール](https://docs.microsoft.com/sql/linux/sql-server-linux-setup?view=sql-server-2017#unattended)データベース エンジン、mssql mlservices と Eula 用のパッケージを追加します。

セットアップや mssql-conf ツールをライセンス契約書への同意を求めることを思い出してください。 既に SQL Server データベース エンジンを構成し、その使用許諾契約書を受け入れる場合、は、オープン ソース R および Python ディストリビューション mlservices 固有の使用許諾契約書パラメーターのいずれかを使用します。

```bash
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml
```

使用許諾契約書への同意の可能なすべての順列が記載されて[mssql-conf ツールを使った Linux 上の SQL Server の構成](sql-server-linux-configure-mssql-conf.md#mlservices-eula)します。

## <a name="offline-installation"></a>オフライン インストール

に従って、[オフライン インストール](sql-server-linux-setup.md#offline)パッケージをインストールする方法の手順を実行します。 ダウンロード サイトを見つけて、以下のパッケージの一覧を使用して特定のパッケージをダウンロードします。

> [!Tip]
> パッケージの管理ツールのいくつか提供するのに役立つコマンドは、パッケージの依存関係を判断します。 Yum を使用して`sudo yum deplist [package]`します。 使用して、ubuntu、`sudo apt-get install --reinstall --download-only [package name]`続けて`dpkg -I [package name].deb`します。


#### <a name="download-site"></a>ダウンロード サイト

パッケージをダウンロードする[ https://packages.microsoft.com/](https://packages.microsoft.com/)します。 すべての R、Python、および Java の mlservices パッケージは、データベース エンジンのパッケージと同じ場所にします。 Mlservices パッケージの基本バージョンが 9.4. 5. (CTP 2.0) の 9.4.6 (CTP 2.1 以降)。 Microsoft オープン r パッケージが含まれる再現率、[別のリポジトリ](#mro)します。

#### <a name="rhel7-paths"></a>RHEL 7/パス

|||
|--|----|
| mssql/mlservices パッケージ | [https://packages.microsoft.com/rhel/7/mssql-server-preview/](https://packages.microsoft.com/rhel/7/mssql-server-preview/) |
| microsoft オープン r パッケージ | [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) | 


#### <a name="ubuntu1604-paths"></a>Ubuntu 16.04/パス

|||
|--|----|
| mssql/mlservices パッケージ | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/) |
| microsoft オープン r パッケージ | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/) | 

#### <a name="sles12-paths"></a>SLES 12/パス

|||
|--|----|
| mssql/mlservices パッケージ | [ https://packages.microsoft.com/sles/12/mssql-server-preview/](https://packages.microsoft.com/sles/12/mssql-server-preview/) |
| microsoft オープン r パッケージ | [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) | 

#### <a name="package-list"></a>パッケージの一覧

どの拡張機能によってを使用するには、特定の言語のために必要なパッケージをダウンロードします。 正確なファイル名は、サフィックスにプラットフォームの情報を含めるが、次のファイル名を取得するファイルを決定するための十分にする必要があります。

```
# Core packages 
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# Java
mssql-server-extensibility-java-15.0.1000

# R
microsoft-openmpi-3.0.0
microsoft-r-open-foreachiterators-3.4.4
microsoft-r-open-mkl-3.4.4
microsoft-r-open-mro-3.4.4
mssql-mlservices-packages-r-9.4.6.523
mssql-mlservices-mlm-r-9.4.6.523
mssql-mlservices-mml-r-9.4.6.523

# Python
microsoft-openmpi-3.0.0
mssql-mlservices-python-9.4.6.523
mssql-mlservices-packages-py-9.4.6.523
mssql-mlservices-mlm-py-9.4.6.523
mssql-mlservices-mml-py-9.4.6.523
```

#### <a name="package-list-for-original-ctp-20-and-21"></a>CTP 2.0 および 2.1 の元のパッケージ リスト

CTP 2.2 削除**mssql mlservices mlm py**と**mssql mlservices mlm r**にパッケージの統合による**mssql mlservices パッケージ py**と**mssql mlservices パッケージ r**、それぞれします。

元の CTP 2.0 または 2.1 パッケージが必要な具体的には、次のパッケージをダウンロードします。

* CTP 2.0 では、ダウンロード パッケージ バージョン 9.4. 5.

* CTP 2.1 では、ダウンロード パッケージ バージョン 9.4.6.237


## <a name="add-more-rpython-packages"></a>R および Python パッケージを追加します。 
 
その他の R と Python パッケージをインストールして、SQL Server 2019 で実行されるスクリプトで使用できます。

### <a name="r-packages"></a>R パッケージ 
 
1. R セッションを開始します。

   ```r
   # sudo /opt/mssql/mlservices/bin/R/R 
   ```

2. 呼ばれる、R パッケージをインストール[グルー](https://mran.microsoft.com/package/glue)パッケージのインストールをテストします。

   ```r
   # install.packages("glue",lib="/opt/mssql/mlservices/libraries/RServer") 
   ```
   または、コマンドラインから、R パッケージをインストールできます。 

   ```r
   # sudo /opt/mssql/mlservices/bin/R/R CMD INSTALL -l /opt/mssql/mlservices/libraries/RServer glue_1.1.1.tar.gz 
   ```

3. R パッケージをインポート[sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)します。

   ```r
   EXEC sp_execute_external_script  
   @language = N'R', 
   @script = N'library(glue)' 
   ```

### <a name="python-packages"></a>Python パッケージ 
 
1. という名前の Python パッケージをインストール[httpie](https://httpie.org/) pip を使用しています。 

   ```python
   # sudo /opt/mssql/mlservices/bin/python/python -m pip install httpie 
   ``` 

2. Python パッケージをインポート[sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)します。
 
   ```python
   EXEC sp_execute_external_script  
   @language = N'Python',  
   @script = N'import httpie' 
   ```

## <a name="limitations-in-ctp-releases"></a>CTP のリリースでの制限事項

Linux 上の R、Python、および Java の統合では、まだアクティブな開発中です。 次の機能はプレビュー バージョンではまだ使用できません。

+ 暗黙の認証は現在 Linux での Machine Learning Services で使用可能なデータまたはその他のリソースにアクセスする実行中の R または Python スクリプトからサーバーに接続することはできませんが、現時点で。 

+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) (R パッケージを格納する、データベース内) でない現在 Linux で利用可能とは、Python をサポートしていません。  

### <a name="resource-governance"></a>リソース ガバナンス

Linux および Windows の間の類似性がある[リソース ガバナンス](../t-sql/statements/create-external-resource-pool-transact-sql.md)の外部リソース プールの統計情報は[sys.dm_resource_governor_external_resource_pools](../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md)が現在Linux 上のさまざまな単位です。 ユニットは、今後の CTP で揃います。
 
| 列名   | 説明 | Linux 上の値 | 
|---------------|--------------|---------------|
|peak_memory_kb | 最大リソース プールに使用されるメモリ量。 | Linux では、この統計は、値が memory.max_usage_in_bytes CGroups メモリ サブシステムからソースします。 |
|write_io_count | IOs のリソース ガバナー統計がリセットされた後に発行された書き込みの合計。 | Linux では、この統計は、書き込みの行に値が blkio.throttle.io_serviced CGroups blkio サブシステムからソースします。 | 
|read_io_count | 読み取りリソース ガバナー統計がリセットされた後に発行された Io の合計。 | Linux では、この統計情報は読み取り行の値が blkio.throttle.io_serviced は、CGroups blkio サブシステムからソースします。 | 
|total_cpu_kernel_ms | 累積的な CPU ユーザー カーネル時間 (リソース ガバナー統計がリセットされた後のミリ秒単位)。 | Linux では、この統計は、ユーザーの行に値が cpuacct.stat CGroups cpuacct サブシステムからソースします。 |  
|total_cpu_user_ms | リソース ガバナー統計がリセットされた後のミリ秒単位で累積的な CPU ユーザー時間。| Linux では、この統計は、システムの行の値に値が cpuacct.stat は、CGroups cpuacct サブシステムからソースします。 | 
|active_processes_count | 要求の時点で実行されている外部プロセスの数。| Linux では、この統計は、値が pids.current GGroups pid サブシステムからソースします。 | 

## <a name="next-steps"></a>次の手順

R 開発者は、簡単な例で作業を開始し、SQL Server での R の動作の基本を学習します。 次の手順で、次のリンクを参照してください。

+ [チュートリアル:T-SQL での R を実行します。](../advanced-analytics/tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)
+ [チュートリアル:R の開発者向けのデータベース内分析](../advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers.md)

Python の開発者は、これらのチュートリアルに従って、SQL Server で Python を使用する方法を学ぶことができます。

+ [チュートリアル:T-SQL での Python を実行します。](../advanced-analytics/tutorials/run-python-using-t-sql.md)
+ [チュートリアル:Python 開発者向けのデータベース内分析](../advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers.md)

実際のシナリオに基づく機械学習の例を表示するを参照してください。 [Machine learning のチュートリアル](../advanced-analytics/tutorials/machine-learning-services-tutorials.md)します。
