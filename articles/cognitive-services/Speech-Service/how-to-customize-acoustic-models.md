---
title: Speech Service を使用して音響モデルを作成する方法 - Microsoft Cognitive Services
description: Microsoft Cognitive Services の Speech Service を使用して音響モデルを作成する方法について説明します。
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 7f7e008e8fb999ce28cf515fe9af549c309316d4
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285178"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>チュートリアル: カスタム音響モデルを作成する

カスタム音響モデルを作成することは、録音デバイスや条件がはっきりしている特定の環境 (自動車など) で、または特定のユーザーによって、使用されるようにアプリケーションが設計されている場合に便利です。 たとえば、アクセント記号付きの音声、特定の背景ノイズ、録音に特定のマイクを使用する場合などです。

このページでは、次の方法について説明します。
> [!div class="checklist"]
> * データを準備する
> * 音響データ セットをインポートする
> * カスタム音響モデルを作成する

Cognitive Services アカウントを持っていない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/try/cognitive-services)を作成してください。

## <a name="prerequisites"></a>前提条件

[[Cognitive Services サブスクリプション]](https://customspeech.ai/Subscriptions) ページを開いて、Cognitive Services アカウントに接続されることを確認します。

**[Connect existing subscription]\(既存のサブスクリプションに接続する\)** ボタンをクリックすると、Azure portal で作成された Speech Service サブスクリプションに接続できます。

Azure portal での Speech Service サブスクリプションの作成については、[概要](get-started.md)ページを参照してください。

## <a name="prepare-the-data"></a>データを準備する

特定のドメインの音響モデルをカスタマイズするには、音声データのコレクションが必要です。 コレクションの範囲は、少数の発話から、数百時間の音声まで広範です。 このコレクションは、一連の音声認識データのオーディオ ファイルと、各オーディオ ファイルの文字起こしのテキスト ファイルで構成されます。 オーディオ データは、認識エンジンを使用する典型的なシナリオのものである必要があります。

例: 

*   ノイズが多い工場環境での音声認識精度を高めるには、オーディオ ファイルがノイズの多い工場での人の会話で構成されている必要があります。
*   1 人の話者に対するパフォーマンスを最適化する場合は (たとえば、FDR の炉辺談話をすべて文字起こしする場合など)、オーディオ ファイルはその話者だけの多くの例で構成される必要があります。

音響モデルをカスタマイズするための音響データ セットは、2 つの部分で構成されます。(1) 音声データを含むオーディオ ファイルと、(2) すべてのオーディオ ファイルの文字起こしを含むファイルです。

### <a name="audio-data-recommendations"></a>オーディオ データの推奨事項

*   データ セット内のすべてのオーディオ ファイルは、WAV (RIFF) オーディオ形式で格納されている必要があります。
*   オーディオのサンプル速度は 8 kHz または 16 kHz でなければならず、サンプル値は圧縮されていない PCM 16 ビット符号付き整数 (short) として格納されている必要があります。
*   単一チャンネル (モノラル) のオーディオ ファイルのみがサポートされます。
*   オーディオ ファイルの長さは、100 ミリ秒から 1 分の範囲である必要があります。 理想的には各オーディオ ファイルの開始時と終了時に 100 ミリ秒以上の無音部分が存在する必要があり、一般的には 500 ミリ秒間から 1 秒の範囲です。
*   データに背景ノイズがある場合は、音声コンテンツの前と後の一方または両方のデータに、さらに長い無音セグメントの例 (たとえば数秒) を入れることをお勧めします。
*   各オーディオ ファイルは、1 つの発話で構成される必要があります (口述の 1 つの文、1 つのクエリ、対話システムの 1 つのターンなど)。
*   データ セット内の各オーディオ ファイルには、一意のファイル名と拡張子 "wav" が必要です。
*   オーディオ ファイルのセットはサブディレクトリのない 1 つのフォルダーに置く必要があり、オーディオ ファイルのセット全体を 1 つの ZIP ファイル アーカイブとしてパッケージ化する必要があります。

> [!NOTE]
> 現在、Web ポータルを使用したデータのインポートは 2 GB に制限されているので、これが音響データ セットの最大サイズです。 これは、16 kHz で録音されたオーディオの約 17 時間分、または 8 kHz で録音されたオーディオの約 34 時間分に相当します。 オーディオ データの主な要件を次の表にまとめます。
>

| プロパティ | 値 |
|---------- |----------|
| ファイル形式 | RIFF (WAV) |
| サンプル速度 | 8000 Hz または 16,000 Hz |
| チャネル | 1 (モノラル) |
| サンプル形式 | PCM、16 ビット整数 |
| ファイルの時間 | 0.1 秒 ～ 60 秒 |
| サイレンス カラー | 0.1 秒以上 |
| アーカイブ形式 | ZIP |
| 最大アーカイブ サイズ | 2 GB |

## <a name="language-support"></a>言語のサポート

次の言語は、カスタムの **Speech to Text** 言語モデルでサポートされています。

サポートされている言語の一覧を表示するには、[こちら](supported-languages.md)をクリックしてください

### <a name="transcriptions-for-the-audio-dataset"></a>オーディオのデータセットの文字起こし

すべての WAV ファイルの文字起こしは、1 つのプレーン テキスト ファイルに格納されている必要があります。 文字起こしファイルの各行には、オーディオ ファイルの 1 つの名前に続けて、対応する文字起こしが記載されている必要があります。 ファイル名と文字起こしは、タブ (\t) で区切る必要があります。

  例: 
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> 文字起こしは、UTF-8 Bom としてエンコードする必要があります

文字起こしは、システムによって処理できるように、テキストが正規化されます。 ただし、Custom Speech Service にデータをアップロードする "_前_" に、ユーザーが行う必要があるいくつかの重要な正規化があります。 文字起こしを準備するときの適切な言語については、[文字起こしガイドライン](prepare-transcription.md)に関するページの該当するセクションをご覧ください。

[Speech Service ポータル](https://customspeech.ai)を使用して、次の手順のようにします。

## <a name="import-the-acoustic-data-set"></a>音響データ セットをインポートする

オーディオ ファイルと文字起こしの準備ができたら、それらをサービス Web ポータルにインポートできます。

そのためには、最初に [Speech Service ポータル](https://customspeech.ai)にサインインします。 次に、上部のリボンの [Custom Speech] ドロップダウン メニューをクリックし、[Adaptation Data]\(適応データ\) を選択します。 Custom Speech Service へのデータのアップロードを初めて行う場合は、[データセット] という名前の空のテーブルが表示されます。 

[音響データ セット] 行の [インポート] ボタンをクリックします。新しいデータ セットをアップロードするためのサイトのページが表示されます。

![試す](media/stt/speech-acoustic-datasets-import.png)

適切なテキスト ボックスに _[名前]_ と _[説明]_ を入力します。 アップロードしたさまざまなデータ セットを追跡するのに役立つわかりやすい説明にします。 次に、[Transcription File]\(文字起こしファイル\) と [WAV files]\(WAV ファイル\) の [Choose File]\(ファイルの選択\) をクリックし、それぞれ、プレーン テキストの文字起こしファイルと、WAV ファイルの zip アーカイブを選択します。 準備が完了したら、[Import]\(インポート\) をクリックしてデータをアップロードします。 データがアップロードされます。 大きなデータ セットでは、数分かかることがあります。

アップロードが完了したら、[Acoustic Datasets]\(音響データセット\) テーブルに戻り、音響データ セットに対応するエントリがあることを確認します。 一意の ID (GUID) が割り当てられることに注意してください。 データには、現在の状態を反映するステータスもあります。 データの状態は、処理を待っている間は [NotStarted]\(未開始\)、検証中は [Running]\(実行中\)、データが使用可能な状態になると [Complete]\(完了\) になります。

データの入力規則には、オーディオ ファイルに関するファイル形式、長さ、サンプル速度の確認、および文字起こしファイルに関するファイル形式の確認とテキスト正規化の実行のための、一連のチェックが含まれます。

状態が [Succeeded]\(成功\) になったら、[Details]\(詳細\) をクリックして音響データ検証レポートを表示できます。 失敗した発話の詳細と共に、検証に成功した発話の数と失敗した発話の数が表示されます。 次の例では、オーディオ形式が不適切なために 2 つの WAV ファイルの検証が失敗しています (このデータ セットでは、1 つのファイルはサンプル速度が正しくなく、もう 1 つのファイルはファイル形式が正しくありません)。

![試す](media/stt/speech-acoustic-datasets-report.png)

データ セットの名前または説明を変更したい場合は、いくつかの時点で、[Edit]\(編集\) リンクをクリックしてこれらのエントリを変更できます。 オーディオ ファイルまたは文字起こしを変更することはできません。

## <a name="create-a-custom-acoustic-model"></a>カスタム音響モデルの作成

音響データ セットの状態が [Complete]\(完了\) になったら、カスタム音響モデルの作成に使用できます。 そのためには、[Custom Speech] ドロップダウン メニューの [Acoustic Models]\(音響モデル\) をクリックします。 [Your models]\(お使いのモデル\) テーブルに、すべてのカスタム音響モデルが一覧表示されます。 初めて使用するときはこのテーブルは空です。 現在のロケールがテーブルのタイトルに表示されます。 現在、音響モデルを作成できるのは英語 (米国) のみです。

新しいモデルを作成するには、テーブル タイトルの下の [Create New]\(新規作成\) をクリックします。 前と同じように、このモデルを識別しやすい名前と説明を入力します。 たとえば、[説明] フィールドを使って、モデルの作成に使用された開始モデルと音響データ セットを記録できます。 次に、ドロップダウン メニューから [Base Acoustic Model]\(基本音響モデル\) を選択します。 基本モデルは、カスタマイズの起点となるモデルです。 2 つの基本音響モデルから選択できます。 _Microsoft Search and Dictation AM_ は、コマンド、検索クエリ、ディクテーションなどのアプリケーションに向けて発せられる音声に適しています。 "_Microsoft Conversational モデル_" は、会話形式の音声の認識に適しています。 この種類の音声は、通常は他の人に向けて発せられるものであり、コール センターや会議で発生します。 Conversational モデルでの部分結果の待機時間は、Search and Dictation モデルより長くなります。

> [!NOTE]
> 現在、すべてのシナリオに対応することを目的とする新しい Universal モデルを展開しています。 前述のモデルも引き続きパブリックに利用できます。

次に、カスタマイズの実行に使用する音響データをドロップダウン メニューで選択します。

![試す](media/stt/speech-acoustic-models-create2.png)

処理が完了したら、必要に応じて、新しいモデルの正確性テストを選択して実行できます。 カスタマイズした音響モデルを使用して指定した音響データ セットについて音声からテキストへの評価が実行されて、結果が報告されます。 このテストを実行するには、[Accuracy Testing]\(正確性テスト\) チェック ボックスをオンにします。 次に、ドロップダウン メニューから言語モデルを選択します。 カスタム言語モデルを作成していない場合は、基本言語モデルのみがドロップダウン リストに表示されます。 ガイドで基本言語モデルの[説明](how-to-customize-language-model.md)を見て、最も適切なものを選択します。

最後に、カスタム モデルの評価に使用する音響データ セットを選択します。 正確性テストを実行する場合は、現実的なモデルのパフォーマンスを把握するため、モデルの作成に使用したものとは異なる音響データを選択することが重要です。 トレーニング データで正確性をテストすると、適応したモデルが実際の条件下でどのように動作するかを評価できません。 結果は、過度に楽観的になります。 また、正確性テストは 1000 個の発話に制限されることにも注意してください。 テスト用音響データセットが大きい場合は、最初の 1000 個の発話のみが評価されます。

カスタマイズ プロセスの実行を開始する準備ができたら、[Create]\(作成\) をクリックします。

この新しいモデルに対応する新しいエントリが音響モデル テーブルに表示されます。 プロセスの状態がテーブルに反映されます。 状態は [Waiting]\(待機中\)、[Processing]\(処理中\)、[Complete]\(完了\) のいずれかです。

![試す](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>次の手順

- [Speech 試用版サブスクリプションを取得する](https://azure.microsoft.com/try/cognitive-services/)
- [C# で音声を認識する方法](quickstart-csharp-dotnet-windows.md)
- [Git サンプル データ](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
