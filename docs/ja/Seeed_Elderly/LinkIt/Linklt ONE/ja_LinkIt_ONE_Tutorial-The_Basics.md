---
description: LinkIt ONE チュートリアル - 基本
title: LinkIt ONE チュートリアル - 基本
keywords:
- Seeed_Elderly
image: https://files.seeedstudio.com/wiki/wiki-platform/S-tempor.png
slug: /ja/LinkIt_ONE_Tutorial-The_Basics
last_update:
  date: 05/15/2025
  author: shuxu hu
---
:::note
この文書は AI によって翻訳されています。内容に不正確な点や改善すべき点がございましたら、文書下部のコメント欄または以下の Issue ページにてご報告ください。  
https://github.com/Seeed-Studio/wiki-documents/issues
:::

**何をするのか**

トランジスタ、LED、トグルスイッチを使用して簡単な回路を作りましょう。発光ダイオード（LED）は電気エネルギーを可視光に変換します。トランジスタは電子工学の基本的な部品で、スイッチや電流増幅器として機能します。ここでは、トランジスタを介してLEDを間接的にオン・オフするためにスイッチを使用します。

**必要なもの**

*   LinkIt One x 1
*   ブレッドボード x 1
*   抵抗 330Ω、1kΩ x 各1個
*   5mm LED x 1
*   スイッチ x 1
*   トランジスタ (2N3904) x 1

**回路図**

![](https://files.seeedstudio.com/wiki/LinkIt_ONE_Tutorial-The_Basics/img/LinkItONE_Kit_0_1.jpg)

**接続**

![](https://files.seeedstudio.com/wiki/LinkIt_ONE_Tutorial-The_Basics/img/LinkItONE_Kit_0_2.jpg)

**トラブルシューティング**

*   LEDの極性を確認してください（長いリードが正極、短いリードが負極です）。
*   スイッチの2つのピンがショートしていないか確認してください。
*   回路内でトランジスタの接続を忘れていないか確認してください（LEDを駆動するにはトランジスタが必要です）。
*   LEDが焼けてしまった場合はどうする？
    *   これは、LEDに電流制限器として機能する入力抵抗を接続し忘れた場合に発生します。

**改善する方法**

これで簡単な制御回路が完成しました。次に、コンデンサという部品を追加することで、さらに魅力的な回路を作りましょう。コンデンサは一時的に電荷を蓄えるデバイスとして機能します。コンデンサをLEDと直列に接続された抵抗と並列に接続すると、LEDが徐々に暗くなります。スイッチがオンになるとコンデンサが充電され、スイッチがオフになるとコンデンサに蓄えられた電荷がゆっくりと放電され、LEDが徐々に消える効果を生み出します。

**さらなるアイデア**

LEDがオンになったときに徐々に明るさを増すために使用される部品は何でしょうか？

**参考**

<!-- *   [基本](/LinkIt_ONE_Tutorial-The_Basics)

*   [Hello World](/LinkIt_ONE_Tutorial-Hello_World)

*   [プッシュボタン](/LinkIt_ONE_Tutorial-Push_Button)

*   [マーキー](/LinkIt_ONE_Tutorial-Marquee)

*   [カラフルな世界](/LinkIt_ONE_Tutorial-Colorful_World)

*   [アナログインターフェース](/LinkIt_ONE_Tutorial-Analog_Interface)

*   [ミニサーボ](/LinkIt-ONE-Tutorial---Mini-Servo)

*   [光センサー](/LinkIt_ONE_Tutorial-Light-Sensor)

*   [SMSでLEDを制御する](/LinkIt_ONE_Tutorial-SMS_control_the_LED)

*   [ウェブページで温度を取得する](/LinkIt_ONE_Tutorial-Get_temperature_with_Webpage) -->

## 技術サポートと製品ディスカッション

弊社製品をお選びいただきありがとうございます！製品の使用体験がスムーズになるよう、さまざまなサポートを提供しています。異なる好みやニーズに対応するため、いくつかのコミュニケーションチャネルを用意しています。

<div class="button_tech_support_container">
<a href="https://forum.seeedstudio.com/" class="button_forum"></a> 
<a href="https://www.seeedstudio.com/contacts" class="button_email"></a>
</div>

<div class="button_tech_support_container">
<a href="https://discord.gg/eWkprNDMU7" class="button_discord"></a> 
<a href="https://github.com/Seeed-Studio/wiki-documents/discussions/69" class="button_discussion"></a>
</div>