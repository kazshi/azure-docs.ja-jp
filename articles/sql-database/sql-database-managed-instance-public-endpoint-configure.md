---
title: パブリック エンドポイントの構成 - Azure SQL Database マネージド インスタンス | Microsoft Docs
description: マネージド インスタンスのパブリック エンドポイントの構成方法について説明します
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: cebe6b4ca61b835e7c77f51592c20799fe271853
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567402"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Azure SQL Database マネージド インスタンスのパブリック エンドポイントの構成

[マネージド インスタンス](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)のパブリック エンドポイントを使用すると、[仮想ネットワーク](../virtual-network/virtual-networks-overview.md)の外部からマネージド インスタンスにデータ アクセスできます。 マネージド インスタンスには、マルチテナントの Azure サービス (Power BI、Azure App Service など) またはオンプレミス ネットワークからアクセスできます。 マネージド インスタンスでパブリック エンドポイントを使用すると、VPN を使用する必要がなくなるため、VPN のスループットの問題を回避できます。

この記事では、以下の方法について説明します。

> [!div class="checklist"]
> - Azure portal でマネージド インスタンスのパブリック エンドポイントを有効にする
> - PowerShell を使用してマネージド インスタンスのパブリック エンドポイントを有効にする
> - マネージド インスタンスのパブリック エンドポイントへのトラフィックを許可するようマネージド インスタンスのネットワーク セキュリティ グループを構成する
> - マネージド インスタンスのパブリック エンドポイントの接続文字列を取得する

## <a name="permissions"></a>アクセス許可

マネージド インスタンス内にあるデータの機密性のため、マネージド インスタンスのパブリック エンドポイントを有効にするための構成では 2 段階のプロセスが必要になります。 このセキュリティ対策は、職務の分離 (SoD) に準拠します。

- マネージド インスタンスのパブリック エンドポイントの有効化は、マネージド インスタンスの管理者が行う必要があります。マネージド インスタンスの管理者は、SQL マネージド インスタンス リソースの **[概要]** ページで確認できます。
- ネットワーク セキュリティ グループを使用したトラフィックの許可は、ネットワークの管理者が行う必要があります。詳細については、[ネットワーク セキュリティ グループのアクセス許可](../virtual-network/manage-network-security-group.md#permissions)に関するページを参照してください。

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Azure portal でマネージド インスタンスのパブリック エンドポイントを有効にする

1. Azure portal (<https://portal.azure.com/.>) を起動します。
1. マネージド インスタンスのあるリソース グループを開き、パブリック エンドポイントを構成する **SQL マネージド インスタンス**を選択します。
1. **[セキュリティ]** の設定で、 **[仮想ネットワーク]** タブを選択します。
1. 仮想ネットワークの構成ページで **[有効]** を選択し、 **[保存]** アイコンを選択して構成を更新します。

![mi-vnet-config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>PowerShell を使用してマネージド インスタンスのパブリック エンドポイントを有効にする

### <a name="enable-public-endpoint"></a>パブリック エンドポイントの有効化

次の PowerShell コマンドを実行します。 **subscription-id** を実際のサブスクリプション ID に置き換えます。 また、**rg-name** を実際のマネージド インスタンスのリソース グループに置き換え、**mi-name** を実際のマネージド インスタンスの名前に置き換えます。

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>パブリック エンドポイントの無効化

PowerShell を使用してパブリック エンドポイントを無効にするには、次のコマンドを実行します (また、受信ポート 3342 を構成済みの場合はそれに対する NSG を忘れずに閉じてください)。

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>ネットワーク セキュリティ グループでパブリック エンドポイント トラフィックを許可する

1. マネージド インスタンスの構成ページをまだ開いている場合は、 **[概要]** タブに移動します。そうでない場合は、**SQL マネージド インスタンス** リソースに戻ります。 **[仮想ネットワーク/サブネット]** リンクを選択します。そうすると、仮想ネットワーク構成ページが表示されます。

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. 仮想ネットワークの左側の構成ウィンドウで **[サブネット]** タブを選択し、ManagedInstance の **[セキュリティ グループ]** を書き留めます。

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. そのマネージド インスタンスが含まれているリソース グループに戻ります。 前に書き留めておいた**ネットワーク セキュリティ グループ**の名前が表示されるはずです。 名前を選択して、ネットワーク セキュリティ グループの構成ページに移動します。

1. **[受信セキュリティ規則]** タブを選択し、**deny_all_inbound** 規則よりも優先度の高い規則を以下の設定で**追加**します。 </br> </br>

    |Setting  |推奨値  |説明  |
    |---------|---------|---------|
    |**ソース**     |任意の IP アドレスまたはサービス タグ         |<ul><li>Power BI などの Azure サービスの場合は、Azure クラウド サービス タグを選択します</li> <li>コンピューターまたは Azure VM の場合は、NAT IP アドレスを使用します</li></ul> |
    |**ソース ポート範囲**     |*         |ソース ポートは、通常、動的に割り当てられ、予測できないため、* (任意) のままにしておきます |
    |**宛先**     |Any         |マネージド インスタンスのサブネットへのトラフィックを許可するには、宛先は [任意] のままにしておきます |
    |**宛先ポート範囲**     |3342         |宛先ポート野範囲を 3342 に設定します。これが、マネージド インスタンスのパブリック TDS エンドポイントです |
    |**プロトコル**     |TCP         |マネージド インスタンスは、TDS に TCP プロトコルを使用します |
    |**アクション**     |Allow         |パブリック エンドポイントを介したマネージド インスタンスへの受信トラフィックを許可します |
    |**優先順位**     |1300         |この規則が **deny_all_inbound** 規則よりも優先度が高いことを確認してください |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > マネージド インスタンスへのパブリック エンドポイント接続にはポート 3342 が使用され、現時点では変更できません。

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>マネージド インスタンスのパブリック エンドポイントの接続文字列を取得する

1. パブリック エンドポイントに対して有効になっている SQL マネージド インスタンスの構成ページに移動します。 **[設定]** 構成の下にある **[接続文字列]** タブを選択します。
1. パブリック エンドポイントのホスト名が <mi_name>.**public**.<dns_zone>.database.windows.net 形式になっていることと、接続に使用されるポートが 3342 であることに注意してください。

    ![mi-public-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>次の手順

- [パブリック エンドポイントで安全に Azure SQL Database マネージド インスタンスを使用する](sql-database-managed-instance-public-endpoint-securely.md)方法を学習する。
