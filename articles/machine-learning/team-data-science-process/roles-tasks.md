---
title: Team Data Science Process の役割とタスク - Azure | Microsoft Docs
description: データ サイエンス チーム プロジェクトの主要コンポーネント、担当者の役割、および関連するタスクの概要。
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
services: machine-learning
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 8cec2c2b72b88a27c4a6c15b197e859b879bef43
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308526"
---
# <a name="team-data-science-process-roles-and-tasks"></a>Team Data Science Process の役割とタスク

Team Data Science Process は、Microsoft が開発したフレームワークです。予測分析ソリューションと高度なアプリケーションを効率的に構築することができる構造化された手法を提供します。 この記事では、このプロセスを標準とするデータ サイエンス チームの人員の役割とそれに関連したタスクの概要について説明します。 

この概要は、データ サイエンス グループ、データ サイエンス チーム、およびプロジェクト全体の TDSP 環境を設定する手順を説明するチュートリアルにリンクされています。 チュートリアルでは、Visual Studio Team Services (VSTS) の使用に関する詳細なガイダンスが提供されます。  VSTS は、チームのタスク、アクセスの制御、およびリポジトリの管理を行うためのコード ホスティング プラットフォームとアジャイル計画ツールを提供します。 

この情報を使用して、独自のコード ホスティングおよびアジャイル計画ツールで TDSP を実装できます。 

## <a name="structures-of-data-science-groups-and-teams"></a>データ サイエンス グループおよびチームの構造

多くの場合、企業のデータ サイエンスの職務は次のような階層で組織されます。

1. ***データ サイエンス グループ***

2. ***グループ内のデータ サイエンス チーム***

このような構造には、グループとチーム リーダーがあります。 通常、データ サイエンス プロジェクトはデータ サイエンス チームが実行します。このチームは、プロジェクト リーダー (プロジェクトの管理およびガバナンス タスク用) と、プロジェクトのデータ サイエンスおよびデータ エンジニアリングの部分を担うデータ サイエンティストまたはエンジニア (個々の共同作成者/技術担当者) で構成される可能性があります。 プロジェクトの実行前に、グループ、チーム、またはプロジェクトのリーダーが設定とガバナンスを実行します。

## <a name="definition-of-four-tdsp-roles"></a>TDSP の 4 つの役割の定義
上記の前提を踏まえ、チームの担当者には、4 つの個別の役割があります。

1. ***グループ マネージャー***。 グループ マネージャーとは、企業のデータ サイエンス ユニット全体のマネージャーのことです。 データ サイエンス ユニット内には複数のチームがあり、それぞれのチームが、異なるビジネスの複数のデータ サイエンス プロジェクトに携わっている場合があります。 グループ マネージャーは、そのタスクを代理人に委任する場合もありますが、役割に関連付けられているタスクは変わりません。

2. ***チーム リーダー***。 チーム リーダーは、企業のデータ サイエンス部のチームを管理します。 チームは複数のデータ サイエンティストで構成されます。 データ サイエンティストがわずかしかいないデータ サイエンス部では、グループ マネージャーとチーム リーダーを 1 人が兼任している場合があります。

3. ***プロジェクト リーダー***。 プロジェクト リーダーは、特定のデータ サイエンス プロジェクトに関する個々 のデータ サイエンティストの毎日のアクティビティを管理します。

4. ***プロジェクトの個々の共同作成者***。 データ サイエンティスト、ビジネス アナリスト、データ エンジニア、設計者など。プロジェクトの個々の共同作成者は、データ サイエンス プロジェクトを実行します。 


> [!NOTE]
> 企業の構造によっては、1 人が複数の役割を担ったり、1 つの役割に複数の担当者が割り当てられたりすることがあります。 このような例は、小規模な企業や、データ サイエンス組織の担当者数が少ない企業でよく見られます。

## <a name="tasks-to-be-completed-by-four-personnel"></a>4 人の担当者が完了するタスク

次の図は、Microsoft が概念化している Team Data Science Process を採用し、実装する際の役割ごとに、担当者の最上位レベルのタスクを示しています。 

![ロールとタスクの概要](./media/roles-tasks/overview-tdsp-top-level.png)

このスキーマと、TDSP の各役割に割り当てられているタスクの詳細なアウトラインを参照し、組織内の責任に応じて適切なチュートリアルを選択してください。

> [!NOTE]
> 以下の手順では、TDSP 環境の設定方法と Visual Studio Team Services (VSTS) で他のデータ サイエンス タスクを完了する方法について説明します。 VSTS を使用してこれらのタスクを達成する方法を示しているのは、Microsoft がこの方法で TDSP を実装しているためです。 VSTS では、ユーティリティの共有、バージョンの整理、役割ベースのセキュリティの提供に使用されるタスクとコード ホスティング サービスを追跡する作業項目の管理を統合することで、共同作業を容易にしています。 必要に応じて、他のプラットフォームを選択して TDSP で示されるタスクを実装することができます。 ただし、ここで VSTS から利用する一部の機能は、プラットフォームによっては使用できない場合があります。 
>
>ここに示す手順では、分析デスクトップとして Azure クラウド上の[データ サイエンス仮想マシン (DSVM)](http://aka.ms/dsvm) と、構成済みで多様な Microsoft ソフトウェアおよび Azure サービスと統合されている、人気のあるデータ サイエンス ツールもいくつか使用します。 DSVM または他の開発環境を使用して TDSP を実装できます。 


## <a name="group-manager-tasks"></a>グループ マネージャーのタスク

次のタスクは、グループ マネージャー (または指定された TDSP システム管理者) が TDSP を採用するために実行します。

- コード ホスティング プラットフォーム (GitHub、Git、VSTS など) で**グループ アカウント**を作成します
- グループ アカウントで**プロジェクト テンプレート リポジトリ**を作成し、Microsoft TDSP チームが開発したプロジェクト テンプレート リポジトリからシード処理を実行します。 Microsoft の TDSP プロジェクト テンプレート リポジトリは、 
    - データ、コード、およびドキュメントを含む**標準化されたディレクトリ構造**を提供します。 
    - 効率的なデータ サイエンス プロセスをガイドするための一連の**標準化されたドキュメント テンプレート**を提供します。 
- **ユーティリティ リポジトリ**を作成し、Microsoft TDSP チームが開発したユーティリティ リポジトリからシード処理を実行します。 Microsoft の TDSP ユーティリティ リポジトリは、 
    - データ サイエンティストの作業をより効率的にする一連の便利なユーティリティを提供します。たとえば、対話型のデータ探索、分析、およびレポート用のユーティリティや、ベースライン モデリングとレポート用のユーティリティなどです。
- グループ アカウントでこれら 2 つのリポジトリの**セキュリティ制御ポリシー**を設定します。  

詳細な手順については、「[Group Manager tasks for a data science team](group-manager-tasks.md)」(データ サイエンス チームのグループ マネージャーのタスク) を参照してください。 


## <a name="team-lead-tasks"></a>チーム リーダーのタスク

次のタスクは、チーム リーダー (または指定されたチーム プロジェクト管理者) が TDSP を採用するために実行します。

- バージョン管理とコラボレーションのためのコード ホスティング プラットフォームとして VSTS を選択する場合は、グループの VSTS サーバーに**チーム プロジェクト**を作成します。 それ以外の場合、このタスクはスキップできます。
- チーム プロジェクト以下に**チーム プロジェクト テンプレート リポジトリ**を作成し、グループ マネージャーまたはマネージャーの代理人が設定したグループ プロジェクト テンプレート リポジトリからシード処理します。 
- **チーム ユーティリティ リポジトリ**を作成し、チーム固有のユーティリティをリポジトリに追加します。 
- (省略可能) チーム全体にとって役立つデータ資産の保存に使用する **[Azure File Storage](https://azure.microsoft.com/services/storage/files/)** を作成します。 他のチーム メンバーは、分析デスクトップにこの共有クラウド ファイル ストアをマウントできます。
- (省略可能) Azure File Storage をチーム リーダーの**データ サイエンス仮想マシン** (DSVM) にマウントし、データ資産を追加します。
- チーム メンバーを追加し、アクセス許可を構成して、**セキュリティ制御**を設定します。 

詳細な手順については、[データ サイエンス チームのチーム リーダーのタスク](team-lead-tasks.md)に関するページを参照してください。  


## <a name="project-lead-tasks"></a>プロジェクト リーダーのタスク

次のタスクは、プロジェクト リーダーが TDSP を採用するために実行します。

- チーム プロジェクト以下に**プロジェクト リポジトリ**を作成し、チーム プロジェクト テンプレート リポジトリからシード処理します。 
- (省略可能) プロジェクトのデータ資産の保存に使用する **Azure File Storage** を作成します。 
- (省略可能) Azure File Storage をプロジェクト リーダーの**データ サイエンス仮想マシン** (DSVM) にマウントし、プロジェクトのデータ資産を追加します。
- プロジェクト メンバーを追加し、アクセス許可を構成して、**セキュリティ制御**を設定します。 

詳細な手順については、[データ サイエンス チームのプロジェクト リーダーのタスク](project-lead-tasks.md)に関するページを参照してください。 

## <a name="project-individual-contributor-tasks"></a>プロジェクトの個々の共同作成者のタスク

次のタスクは、プロジェクトの個々の共同作成者 (通常はデータ サイエンティスト) が、TDSP を使用してデータ サイエンス プロジェクトを進めるために実行します。

- プロジェクト リーダーが設定した**プロジェクト リポジトリ**を複製します。 
- (省略可能) **データ サイエンス仮想マシン** (DSVM) にチームとプロジェクトの共有 **Azure File Storage** をマウントします。
- プロジェクトを実行します。 

 
プロジェクトのオンボーディングに関する詳細な手順については、[データ サイエンス チームのプロジェクトの個々の共同作成者](project-ic-tasks.md)に関するページを参照してください。 


## <a name="data-science-project-execution"></a>データ サイエンス プロジェクトの実行
 
データ サイエンティスト、プロジェクト リーダー、およびチーム リーダーは、関連する一連の手順に従って、プロジェクトの最初から最後までに必要なすべてのタスクと段階を追跡する作業項目を作成できます。 また、Git を使用すると、データ サイエンティスト間の共同作業が容易になり、プロジェクトの実行中に生成されるアーティファクトをプロジェクト メンバー全員でバージョン管理し、共有することができます。

プロジェクトの実行用に用意されている手順は、作業項目とプロジェクトの Git リポジトリの両方が VSTS 上にあるという前提に基づいて作成されています。 両方に VSTS を使用することで、作業項目をプロジェクト リポジトリの Git ブランチとリンクできるようになります。 この方法を利用すると、作業項目に対する処理内容を簡単に追跡できます。 

次の図は、この TDSP を使用したプロジェクト実行のワークフローの概要です。

![一般的なデータ サイエンス プロジェクトの実行](./media/roles-tasks/overview-project-execute.png)

ワークフローには、次の 3 つのアクティビティにグループ化できる手順が含まれています。

- スプリント計画 (プロジェクト リーダー)
- 作業項目に対処する Git ブランチ上のアーティファクト開発 (データ サイエンティスト)
- コード レビューと、ブランチとマスター ブランチの統合 (プロジェクト リーダーまたは他のチーム メンバー)

プロジェクト実行ワークフローの詳細な手順については、「[Execution of data science projects](project-execution.md)」(データ サイエンス プロジェクトの実行) を参照してください。

## <a name="project-structure"></a>プロジェクト構造

この[プロジェクト テンプレート リポジトリ](https://github.com/Azure/Azure-TDSP-ProjectTemplate)を使用して、効率的なプロジェクトの実行とコラボレーションをサポートします。 このリポジトリには、独自の TDSP プロジェクトで使用できる標準化されたディレクトリ構造とドキュメント テンプレートが用意されています。

## <a name="next-steps"></a>次の手順

Team Data Science Process で定義されている役割とタスクの詳細な説明を確認します。

- [データ サイエンス チームのグループ マネージャーのタスク](group-manager-tasks.md)
- [データ サイエンス チームのチーム リーダーのタスク](team-lead-tasks.md)
- [データ サイエンス チームのプロジェクト リーダーのタスク](project-lead-tasks.md)
- [データ サイエンス チームの個々の共同作成者](project-ic-tasks.md)