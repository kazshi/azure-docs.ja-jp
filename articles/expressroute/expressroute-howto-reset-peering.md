---
title: Azure ExpressRoute ピアリングをリセットする | Microsoft Docs
description: ExpressRoute 回線のピアリングを無効および有効にする方法。
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180555"
---
# <a name="reset-expressroute-peerings"></a>ExpressRoute ピアリングをリセットする

この記事では、PowerShell を使用して ExpressRoute 回線のピアリングを無効および有効にする方法について説明します。 ピアリングを無効にすると、ExpressRoute 回線のプライマリ接続とセカンダリ接続の両方の BGP セッションがシャットダウンされます。 Microsoft へのこのピアリング経由の接続が失われます。 ピアリングを有効にすると、ExpressRoute 回線のプライマリ接続とセカンダリ接続の両方の BGP セッションが起動されます。 Microsoft へのこのピアリング経由の接続が回復します。 ExpressRoute 回線上の Microsoft ピアリングと Azure プライベート ピアリングを独立に有効および無効にすることができます。 ExpressRoute 回線上のピアリングを初めて構成すると、そのピアリングは既定で有効になります。 

ExpressRoute ピアリングをリセットすると役立つ可能性のあるシナリオがいくつかの存在します。
* ディザスター リカバリーの設計と実装をテストします。 たとえば、2 つの ExpressRoute 回線があるとします。 1 つの回線のピアリングを無効にして、ネットワーク トラフィックをもう一方の回線に強制的にフェールオーバーするようにできます。
* ExpressRoute 回線の Azure プライベート ピアリングで BFD (Bidirectional Forwarding Detection) を有効にします。 ExpressRoute 回線が 2018 年 8 月 1 日の後に作成されている場合、BFD は既定で有効になります。 回線がその前に作成された場合、BFD は有効になっていません。 ピアリングを無効にし、再度有効にすることによって、BFD を有効にすることができます。 BFD が Azure プライベート ピアリングでのみサポートされることに注意する必要があります。


## <a name="reset-a-peering"></a>ピアリングをリセットする

1. Azure Resource Manager PowerShell コマンドレットの最新版をインストールしてください。 詳細については、[Azure PowerShell のインストールおよび構成](/powershell/azure/install-azurerm-ps)をご覧ください。

2. 昇格された特権で PowerShell コンソールを開き、アカウントに接続します。 接続については、次の例を参考にしてください。

  ```powershell
  Connect-AzureRmAccount
  ```
3. 複数の Azure サブスクリプションを所有している場合には、アカウントのサブスクリプションをすべて確認します。

  ```powershell
  Get-AzureRmSubscription
  ```
4. 使用するサブスクリプションを指定します。

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. 次のコマンドを実行して、ExpressRoute 回線を取得します。

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. 無効または有効にするピアリングを識別します。 *Peerings* は配列です。 次の例では、Peerings[0] は Azure プライベート ピアリングであり、Peerings[1] は Microsoft ピアリングです。

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. 次のコマンドを実行して、ピアリングの状態を変更します。

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
ピアリングが、設定された状態になります。 

## <a name="next-steps"></a>次の手順
ExpressRoute の問題をトラブルシューティングするためにサポートが必要な場合は、次の記事をチェックしてください。
* [ExpressRoute 接続の確認](expressroute-troubleshooting-expressroute-overview.md)
* [ネットワーク パフォーマンスのトラブルシューティング](expressroute-troubleshooting-network-performance.md)
