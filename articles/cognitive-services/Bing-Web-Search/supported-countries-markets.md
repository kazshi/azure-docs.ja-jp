---
title: Bing Web Search API でサポートされている国、地域、言語
titleSuffix: Azure Cognitive Services
description: Bing Web Search API でサポートされている国/地域と言語を確認します。
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: b954814d1c26e8afd24e0bb13e74c493f0fd2c55
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125121"
---
# <a name="countries-regions-and-languages-supported-by-the-bing-web-search-api"></a>Bing Web Search API でサポートされている国、地域、言語

Bing Web Search API では 30 を超える国または地域がサポートされ、その多くで複数の言語が使用されています。 クエリでの国または地域の指定は、国または地域の関心に基づいた検索結果の絞り込みに役立ちます。 結果には Bing へのリンクを含めることができ、これらのリンクにより、指定された国/地域または言語に従って Bing ユーザー エクスペリエンスをローカライズできます。

`cc` クエリ パラメーターを使用して国または地域を指定できます。 国または地域を指定する場合、[`Accept-Language` ヘッダー](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers)を使用して 1 つ以上の言語コードも指定する必要があります。 各市場でサポートされている言語の一覧を確認するには、「[市場](#Markets)」の表をご利用ください。

または、`mkt` クエリ パラメーターと「**市場**」の表のコードを使用して市場を指定できます。 市場を指定すると、同時に国または地域と優先言語が指定されます。 `setLang` クエリ パラメーターを使用すれば、言語を明示的に設定できます。

## <a name="countriesregions"></a>国/地域

|国/リージョン|コード|
|-------|----|
|アルゼンチン|AR|
|オーストラリア|AU|
|オーストリア|AT|
|ベルギー|BE|
|ブラジル|BR|
|カナダ|CA|
|チリ|CL|
|デンマーク|DK|
|フィンランド|FI|
|フランス|FR|
|ドイツ|DE|
|香港特別行政区|HK|
|インド|IN|
|インドネシア|ID|
|イタリア|IT|
|日本|JP|
|韓国|KR|
|マレーシア|MY|
|メキシコ|MX|
|オランダ|NL|
|ニュージーランド|NZ|
|ノルウェー|NO|
|中国|CN|
|ポーランド|PL|
|ポルトガル|PT|
|フィリピン|PH|
|ロシア|RU|
|サウジアラビア|SA|
|南アフリカ|ZA|
|スペイン|ES|
|スウェーデン|SE|
|スイス|CH|
|台湾|TW|
|トルコ|TR|
|イギリス|GB|
|米国|US|

## <a name="markets"></a>市場

|国/リージョン|Language|市場コード|
|-------|--------|-----------|
|アルゼンチン|スペイン語|es-AR|
|オーストラリア|英語|en-AU|
|オーストリア|ドイツ語|de-AT|
|ベルギー|オランダ語|nl-BE|
|ベルギー|フランス語|fr-BE|
|ブラジル|ポルトガル語|pt-BR|
|カナダ|英語|en-CA|
|カナダ|フランス語|fr-CA|
|チリ|スペイン語|es-CL|
|デンマーク|デンマーク語|da-DK|
|フィンランド|フィンランド語|fi-FI|
|フランス|フランス語|fr-FR|
|ドイツ|ドイツ語|de-DE|
|香港特別行政区|繁体字中国語|zh-HK|
|インド|英語|en-IN|
|インドネシア|英語|en-ID|
|イタリア|イタリア語|it-IT|
|日本|日本語|ja-JP|
|韓国|韓国語|ko-KR|
|マレーシア|英語|en-MY|
|メキシコ|スペイン語|es-MX|
|オランダ|オランダ語|nl-NL|
|ニュージーランド|英語|en-NZ|
|ノルウェー|ノルウェー語|no-NO|
|中国|中国語|zh-CN|
|ポーランド|ポーランド語|pl-PL|
|ポルトガル|ポルトガル語|pt-PT|
|フィリピン|英語|en-PH|
|ロシア|ロシア語|ru-RU|
|サウジアラビア|アラビア語|ar-SA|
|南アフリカ|英語|en-ZA|
|スペイン|スペイン語|es-ES|
|スウェーデン|スウェーデン語|sv-SE|
|スイス|フランス語|fr-CH|
|スイス|ドイツ語|de-CH|
|台湾|繁体字中国語|zh-TW|
|トルコ|トルコ語|tr-TR|
|イギリス|英語|en-GB|
|米国|英語|en-US|
|米国|スペイン語|es-US|
