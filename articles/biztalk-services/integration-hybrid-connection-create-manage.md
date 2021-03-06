---
title: ハイブリッド接続の作成と管理 | Microsoft Docs
description: ハイブリッド接続の作成、接続の管理、Hybrid Connection Manager のインストールの方法について説明します。 MABS、WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 1751d33b5f6f6a506654daedd15bbd75ae271483
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2017
ms.locfileid: "26628847"
---
# <a name="create-and-manage-hybrid-connections"></a>ハイブリッド接続の作成と管理

> [!IMPORTANT]
> BizTalk ハイブリッド接続は廃止され、App Service ハイブリッド接続に置き換えられています。 既存の BizTalk ハイブリッド接続の管理方法など詳しくは、「[Azure App Services からのハイブリッド接続](../app-service/app-service-hybrid-connections.md)」をご覧ください。

>[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>手順の概要
1. プライベート ネットワークのオンプレミス リソースの **host name** いずれか **FQDN** of the on-premises resource in your private netwいずれかk.
2. ハイブリッド接続に Azure Web Apps または Azure Mobile Apps を関連付ける。
3. オンプレミス リソースに Hybrid Connection Manager をインストールし、特定のハイブリッド接続に接続する。 Azure ポータルでは 1 回のクリックでインストールと接続を実行できます。
4. ハイブリッド接続を管理し、その接続キーを管理します。

このトピックではこれらの手順について説明します。 

> [!IMPORTANT]
> ハイブリッド接続のエンドポイントを IP アドレスに設定することができます。 IP アドレスを使用する場合、クライアントに応じて、オンプレミス リソースにアクセスできる場合とできない場合があります。 ハイブリッド接続は、DNS 参照を行うクライアントに依存します。 ほとんどの場合、 **クライアント** はアプリケーション コードです。 クライアントが DNS 参照を実行しない場合 (IP アドレスをドメイン名 (x.x.x.x) であるかのように解決しようとしない場合)、トラフィックはハイブリッド接続を介して送信されません。
> 
> たとえば (擬似コードの場合)、オンプレミス ホストとして **10.4.5.6** を定義します。
> 
> **次のシナリオは動作します。**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **次のシナリオは動作しません。**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>ハイブリッド接続の作成
ハイブリッド接続は、「[Azure App Services からのハイブリッド接続](../app-service/app-service-hybrid-connections.md)」の説明に従って、**または**[BizTalk Services REST API](https://msdn.microsoft.com/library/azure/dn232347.aspx)を使って、作成できます。 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>その他
* 複数のハイブリッド接続を作成することができます。 許可されている接続数については「 [BizTalk サービス: 開発者、基本、標準、およびプレミアム エディションのチャート](biztalk-editions-feature-chart.md) 」を参照してください。 
* それぞれのハイブリッド接続は、送信用のアプリケーション キーとリッスン用のオンプレミス キーで構成される接続文字列のペアと共に作成されます。 各ペアにはプライマリ キーとセカンダリ キーが含まれています。 

## <a name="LinkWebSite"></a>Azure App Service の Web アプリまたはモバイル アプリを関連付ける
既存のハイブリッド接続に Azure App Service の Web アプリまたはモバイル アプリを関連付けるには、ハイブリッド接続ブレードで **[既存のハイブリッド接続を使用する]** を選択します。 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>オンプレミスへの Hybrid Connection Manager のインストール
ハイブリッド接続の作成後、オンプレミス リソースに Hybrid Connection Manager をインストールします。 Hybrid Connection Manager は Azure Web Apps または BizTalk サービスからダウンロードできます。 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
「[Azure App Services からのハイブリッド接続](../app-service/app-service-hybrid-connections.md)」もよいリソースです。

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>その他
* ハイブリッド接続マネージャーは次のオペレーティング システムにインストールできます。
  
  * Windows Server 2008 R2 (.NET Framework 4.5 以降および Windows Management Framework 4.0 以上が必要)
  * Windows Server 2012 (Windows Management Framework 4.0 以上が必要)
  * Windows Server 2012 R2
* Hybrid Connection Manager をインストールすると、以下のことが行われます。 
  
  * プライマリ アプリケーション接続文字列が使用されるように、Azure がホストするハイブリッド接続が自動的に構成されます。 
  * プライマリ オンプレミス接続文字列が使用されるように、オンプレミス リソースが自動的に構成されます。
* Hybrid Connection Manager は、承認のために有効なオンプレミスの接続文字列を使用する必要があります。 Azure Web Apps または Mobile Apps は、承認のために有効なアプリケーション接続文字列を使用する必要があります。
* Hybrid Connection Manager の別のインスタンスを別のサーバーにインストールすることにより、ハイブリッド接続を拡張できます。 最初のオンプレミス リスナーと同じアドレスを使用するように、オンプレミス リスナーを構成します。 この状況では、トラフィックは複数のアクティブなオンプレミス リスナーにランダムに分散されます (ラウンド ロビン方式)。 

## <a name="ManageHybridConnection"></a>ハイブリッド接続の管理

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

「[Azure App Services からのハイブリッド接続](../app-service/app-service-hybrid-connections.md)」もよいリソースです。

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>ハイブリッド接続文字列をコピー、再生成する

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

「[Azure App Services からのハイブリッド接続](../app-service/app-service-hybrid-connections.md)」もよいリソースです。

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>ハイブリッド接続で使用するオンプレミス リソースを管理するグループ ポリシーの使用
1. [Hybrid Connection Manager の管理用テンプレート](http://www.microsoft.com/download/details.aspx?id=42963)をダウンロードします。
2. ファイルを解凍します。
3. グループ ポリシーを変更するコンピューターで、以下の手順を実行します。  
   
   * .ADMX ファイルを *%WINROOT%\PolicyDefinitions* フォルダーにコピーします。
   * .ADML ファイルを *%WINROOT%\PolicyDefinitions\en-us* フォルダーにコピーします。

コピーが終了すると、グループ ポリシー エディターを使用してポリシーを変更できます。

## <a name="next"></a>次へ
[ハイブリッド接続の概要](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>関連項目
[Microsoft Azure での BizTalk Services 管理の REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk サービス: 開発者、基本、標準、およびプレミアム エディションのチャート](biztalk-editions-feature-chart.md)  
[BizTalk サービスを作成する](biztalk-provision-services.md)  
[BizTalk Services: [ダッシュボード]、[監視]、および [スケール] タブ](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
