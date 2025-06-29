---
description: Wio Terminalを使用して騒音レベルを測定する方法
title:  Wio Terminalを使用して騒音レベルを測定する方法
keywords:
- Wio_terminal チュートリアル
image: https://files.seeedstudio.com/wiki/wiki-platform/S-tempor.png
slug: /ja/Wio-Terminal-Sound-Meter
last_update:
  date: 05/15/2025
  author: jianjing Huang
---
:::note
この文書は AI によって翻訳されています。内容に不正確な点や改善すべき点がございましたら、文書下部のコメント欄または以下の Issue ページにてご報告ください。  
https://github.com/Seeed-Studio/wiki-documents/issues
:::

# Wio Terminalを使用して騒音レベルを測定する方法

## 概要

この例では、Wio Terminalでデシベルメーターを完璧に表示する方法を示します。

## 部品リスト

- [**Wio Terminal**](https://www.seeedstudio.com/Wio-Terminal-p-4509.html)
- [**ReSpeaker 2-Mics Pi HAT**](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT.html)

:::note
**ハードウェア接続**については、[**Audio Overview**](https://wiki.seeedstudio.com/ja/Wio-Terminal-Audio-Overview/)をご覧ください。
:::

## 特徴

- デシベル（dB）は、ダイヤルプレート、読み取り値、折れ線グラフでそれぞれ表示できます。
- dBの数値が何を意味するのかを紹介するガイドがあります。

## はじめに

Wio Terminal用のオーディオライブラリを使用するには、以下の手順に従ってください。

## ライブラリのインストール

1. オーディオライブラリ `Seeed_Arduino_Audio` をインストールします。詳細については、[**Audio Overview**](https://wiki.seeedstudio.com/ja/Wio-Terminal-Audio-Overview/)をご覧ください。

2. LCDスクリーンライブラリ `Seeed_Arduino_LCD` をインストールします。詳細については、[Wio Terminal LCD](https://wiki.seeedstudio.com/ja/Wio-Terminal-LCD-Overview/)をご覧ください。

3. 次に、ライブラリをArduino IDEにインストールします。Arduino IDEを開き、`スケッチ` -> `ライブラリをインクルード` -> `ZIP形式のライブラリを追加` をクリックし、ダウンロードした `Seeed_Arduino_Audio` ファイルを選択します。

![InstallLibrary](https://files.seeedstudio.com/wiki/Wio-Terminal/img/Xnip2019-11-21_15-50-13.jpg)

## ArduinoモニターにdB値を表示する

この例では、ArduinoモニターにdB値を表示します。そのため、dBをテストするコードを用意しました。

:::note
コードは非常に長いため、「クリップボードにコピー」をクリックしてArduino IDEに貼り付けてください。
:::

```cpp
#include <Audio.h>
#include <Wire.h> 
#include <SD.h> 

int i;
float v[512] = {0};
float magnitude = 0;
float dB_holder;
float a_weight[512] = {0.000491601, 0.007439144, 0.026456683, 0.057237193, 0.097648235, 0.145530499, 0.19895028, 0.256192702, 0.315753433, 0.376341431, 0.436880336, 0.496502258, 0.554533348, 0.610473237, 0.663971116, 0.714800959, 0.762837753, 0.808035908, 0.85041046, 0.890021233, 0.926959868, 0.961339445, 0.993286373, 1.022934171, 1.050418819, 1.075875354, 1.099435459, 1.12122582, 1.141367062, 1.159973138, 1.177151032, 1.193000704, 1.207615207, 1.221080906, 1.233477786, 1.244879798, 1.255355228, 1.264967081, 1.273773452, 1.281827897, 1.289179783, 1.29587462, 1.301954369, 1.307457742, 1.312420463, 1.316875521, 1.320853401, 1.324382291, 1.327488281, 1.330195534, 1.332526449, 1.334501812, 1.336140927, 1.337461742, 1.338480956, 1.339214128, 1.339675764, 1.339879407, 1.33983771, 1.33956251, 1.339064892, 1.338355246, 1.337443324, 1.336338287, 1.33504875, 1.333582824, 1.331948154, 1.330151954, 1.328201037, 1.326101843, 1.323860472, 1.3214827, 1.318974007, 1.316339599, 1.313584419, 1.310713175, 1.307730349, 1.304640214, 1.301446848, 1.298154147, 1.294765835, 1.291285478, 1.28771649, 1.284062146, 1.280325589, 1.276509835, 1.272617788, 1.268652238, 1.264615874, 1.260511287, 1.256340974, 1.252107348, 1.247812737, 1.243459393, 1.239049492, 1.234585143, 1.230068386, 1.2255012, 1.220885501, 1.216223152, 1.211515958, 1.206765674, 1.201974005, 1.19714261, 1.1922731, 1.187367045, 1.182425973, 1.177451372, 1.172444693, 1.167407348, 1.162340715, 1.157246139, 1.152124932, 1.146978373, 1.141807712, 1.136614169, 1.131398936, 1.126163178, 1.120908033, 1.115634612, 1.110344003, 1.105037268, 1.099715447, 1.094379556, 1.08903059, 1.083669521, 1.078297301, 1.072914859, 1.067523108, 1.062122937, 1.05671522, 1.05130081, 1.045880542, 1.040455233, 1.035025683, 1.029592675, 1.024156975, 1.018719333, 1.013280482, 1.00784114, 1.00240201, 0.996963778, 0.991527118, 0.986092686, 0.980661126, 0.975233068, 0.969809127, 0.964389904, 0.958975988, 0.953567954, 0.948166364, 0.942771769, 0.937384705, 0.932005696, 0.926635255, 0.921273883, 0.915922069, 0.91058029, 0.905249011, 0.899928687, 0.894619763, 0.88932267, 0.88403783, 0.878765656, 0.873506548, 0.868260896, 0.863029082, 0.857811477, 0.852608441, 0.847420326, 0.842247473, 0.837090216, 0.831948877, 0.826823771, 0.821715204, 0.816623471, 0.811548862, 0.806491654, 0.80145212, 0.796430522, 0.791427115, 0.786442146, 0.781475853, 0.776528467, 0.771600212, 0.766691304, 0.761801951, 0.756932355, 0.752082709, 0.747253202, 0.742444013, 0.737655316, 0.732887277, 0.728140056, 0.723413808, 0.71870868, 0.714024813, 0.709362343, 0.704721397, 0.700102099, 0.695504567, 0.690928912, 0.686375241, 0.681843653, 0.677334244, 0.672847104, 0.668382317, 0.663939963, 0.659520116, 0.655122846, 0.650748219, 0.646396293, 0.642067124, 0.637760764, 0.633477258, 0.629216649, 0.624978975, 0.620764269, 0.616572561, 0.612403877, 0.608258238, 0.604135661, 0.600036161, 0.595959749, 0.59190643, 0.587876209, 0.583869084, 0.579885053, 0.575924108, 0.571986239, 0.568071433, 0.564179674, 0.560310942, 0.556465216, 0.552642469, 0.548842674, 0.5450658, 0.541311815, 0.537580682, 0.533872363, 0.530186816, 0.526524, 0.522883867, 0.519266371, 0.51567146, 0.512099084, 0.508549187, 0.505021714, 0.501516605, 0.498033801, 0.49457324, 0.491134857, 0.487718587, 0.484324362, 0.480952113, 0.47760177, 0.47427326, 0.47096651, 0.467681443, 0.464417985, 0.461176056, 0.457955577, 0.454756467, 0.451578645, 0.448422028, 0.445286531, 0.442172069, 0.439078556, 0.436005904, 0.432954025, 0.429922829, 0.426912226, 0.423922125, 0.420952434, 0.41800306, 0.41507391, 0.412164889, 0.409275902, 0.406406853, 0.403557647, 0.400728185, 0.397918372, 0.395128108, 0.392357295, 0.389605834, 0.386873625, 0.384160569, 0.381466565, 0.378791512, 0.37613531, 0.373497856, 0.37087905, 0.368278789, 0.36569697, 0.363133492, 0.360588252, 0.358061147, 0.355552074, 0.353060929, 0.35058761, 0.348132012, 0.345694033, 0.343273569, 0.340870517, 0.338484772, 0.336116231, 0.333764791, 0.331430348, 0.329112798, 0.326812038, 0.324527964, 0.322260474, 0.320009464, 0.31777483, 0.315556471, 0.313354282, 0.311168162, 0.308998008, 0.306843717, 0.304705188, 0.302582319, 0.300475007, 0.298383152, 0.296306653, 0.294245408, 0.292199316, 0.290168278, 0.288152193, 0.286150961, 0.284164484, 0.28219266, 0.280235393, 0.278292582, 0.276364131, 0.27444994, 0.272549913, 0.270663953, 0.268791961, 0.266933843, 0.265089501, 0.263258841, 0.261441767, 0.259638183, 0.257847996, 0.256071111, 0.254307435, 0.252556875, 0.250819337, 0.249094729, 0.247382959, 0.245683935, 0.243997567, 0.242323763, 0.240662434, 0.239013489, 0.23737684, 0.235752397, 0.234140071, 0.232539776, 0.230951422, 0.229374924, 0.227810195, 0.226257148, 0.224715697, 0.223185759, 0.221667247, 0.220160077, 0.218664167, 0.217179432, 0.215705789, 0.214243158, 0.212791454, 0.211350598, 0.209920507, 0.208501103, 0.207092304, 0.205694032, 0.204306207, 0.202928752, 0.201561587, 0.200204635, 0.19885782, 0.197521065, 0.196194293, 0.19487743, 0.193570399, 0.192273127, 0.19098554, 0.189707563, 0.188439124, 0.187180149, 0.185930567, 0.184690306, 0.183459294, 0.182237461, 0.181024737, 0.179821051, 0.178626335, 0.177440519, 0.176263535, 0.175095315, 0.173935792, 0.172784898, 0.171642567, 0.170508733, 0.16938333, 0.168266293, 0.167157558, 0.166057059, 0.164964734, 0.163880519, 0.162804351, 0.161736168, 0.160675908, 0.159623508, 0.158578909, 0.157542048, 0.156512867, 0.155491306, 0.154477304, 0.153470803, 0.152471745, 0.151480071, 0.150495724, 0.149518647, 0.148548783, 0.147586076, 0.146630469, 0.145681907, 0.144740336, 0.143805699, 0.142877944, 0.141957016, 0.141042862, 0.140135428, 0.139234663, 0.138340513, 0.137452928, 0.136571854, 0.135697243, 0.134829041, 0.133967201, 0.133111671, 0.132262402, 0.131419345, 0.130582451, 0.129751671, 0.128926959, 0.128108265, 0.127295544, 0.126488747, 0.125687829, 0.124892743, 0.124103444, 0.123319885, 0.122542022, 0.121769811, 0.121003206, 0.120242164, 0.119486641, 0.118736594, 0.117991979, 0.117252754, 0.116518877, 0.115790306, 0.115066999, 0.114348914, 0.113636011, 0.112928249, 0.112225587, 0.111527986, 0.110835406, 0.110147807, 0.109465151, 0.108787398, 0.108114511, 0.10744645, 0.106783179, 0.106124659, 0.105470853, 0.104821725, 0.104177238, 0.103537355, 0.10290204, 0.102271258, 0.101644973, 0.10102315, 0.100405754, 0.09979275, 0.099184104, 0.098579782, 0.09797975, 0.097383975, 0.096792423, 0.096205061, 0.095621857, 0.095042778, 0.094467792, 0.093896867, 0.093329972, 0.092767075, 0.092208144, 0.091653149, 0.09110206, 0.090554845, 0.090011475, 0.08947192, 0.088936151, 0.088404136, 0.087875849, 0.087351259, 0.086830338};

AudioInputI2S            i2s1;           //xy=204.00000381469727,247.00000381469727
AudioMixer4              mixer1;         //xy=402,248
AudioAnalyzeFFT1024      fft1024_1;      //xy=635.0000076293945,249.00000381469727
AudioConnection          patchCord1(i2s1, 0, mixer1, 0);
AudioConnection          patchCord2(i2s1, 1, mixer1, 1);
AudioConnection          patchCord3(mixer1, fft1024_1);
// GUItool: end automatically generated code

AudioControlWM8960 wm8960;

const int myInput = AUDIO_INPUT_LINEIN;

void setup() {
  analogReadResolution(16);
  AudioMemory(60);
  Serial.begin(9600);
  fft1024_1.windowFunction(AudioWindowHanning1024);

  wm8960.enable();
  wm8960.inputSelect(myInput);
  wm8960.volume(0.5);
}

void loop() { 
  if (fft1024_1.available()) {
      magnitude = 0;
      dB_holder = 0;
      float v[512] = {0};
      
      for (i=0; i<512; i++) {
        v[i] = fft1024_1.read(i) * a_weight[i];
        magnitude = magnitude + sq(v[i]);
      }
      magnitude = sqrt(magnitude);
      
      dB_holder = log10f(magnitude) * 20  + 97.05;
      Serial.println(dB_holder,2); // f[23] = 1kHz, f[82] = 3.5kHz, f[252] = 12kHz
  }
}
```

デシベル値が表示されます。
<div align="center"><img src="https://files.seeedstudio.com/wiki/Wio-Terminal-Sound-Meter/sound-Meter-value_gGIF.gif" /></div>

## デシベルメーターデモ

この例では、ReSpeaker 2-Mic Hat のマイクを使用してデシベル (dB) を検出します。周囲の環境が検出され、ディスプレイに表示されます。

<div align="center"><img src="https://files.seeedstudio.com/wiki/Wio-Terminal-Sound-Meter/sound-Meter_gGIF.gif" /></div>

## 完全なコード

```cpp

#include <Audio.h>
#include <Wire.h> 
#include <SD.h> 
#include <TFT_eSPI.h> // ハードウェア固有のライブラリ
#include <SPI.h>
#include <Bounce.h>
#include"seeed_line_chart.h" // ライブラリをインクルード
TFT_eSPI tft = TFT_eSPI();       // カスタムライブラリを呼び出し
#define TFT_GREY 0x5AEB
#define LOOP_PERIOD 35 // ディスプレイは35msごとに更新

#define max_size 20 // データの最大サイズ
doubles data; // データを格納するための doubles 型を初期化
TFT_eSprite spr = TFT_eSprite(&tft);  // スプライト 
TFT_eSprite spr2 = TFT_eSprite(&tft);

float ltx = 0;    // 針の下部の保存された x 座標
uint16_t osx = 120, osy = 120; // 保存された x & y 座標
uint32_t updateTime = 0;       // 次の更新時間

int old_analog =  -999; // 最後に表示された値

int mode = 0;  // 1=情報表示

int i;
float v[512] = {0};
float magnitude = 0;
float dB_holder;
float a_weight[512] = {0.000491601, 0.007439144, 0.026456683, 0.057237193, 0.097648235, 0.145530499, 0.19895028, 0.256192702, 0.315753433, 0.376341431, 0.436880336, 0.496502258, 0.554533348, 0.610473237, 0.663971116, 0.714800959, 0.762837753, 0.808035908, 0.85041046, 0.890021233, 0.926959868, 0.961339445, 0.993286373, 1.022934171, 1.050418819, 1.075875354, 1.099435459, 1.12122582, 1.141367062, 1.159973138, 1.177151032, 1.193000704, 1.207615207, 1.221080906, 1.233477786, 1.244879798, 1.255355228, 1.264967081, 1.273773452, 1.281827897, 1.289179783, 1.29587462, 1.301954369, 1.307457742, 1.312420463, 1.316875521, 1.320853401, 1.324382291, 1.327488281, 1.330195534, 1.332526449, 1.334501812, 1.336140927, 1.337461742, 1.338480956, 1.339214128, 1.339675764, 1.339879407, 1.33983771, 1.33956251, 1.339064892, 1.338355246, 1.337443324, 1.336338287, 1.33504875, 1.333582824, 1.331948154, 1.330151954, 1.328201037, 1.326101843, 1.323860472, 1.3214827, 1.318974007, 1.316339599, 1.313584419, 1.310713175, 1.307730349, 1.304640214, 1.301446848, 1.298154147, 1.294765835, 1.291285478, 1.28771649, 1.284062146, 1.280325589, 1.276509835, 1.272617788, 1.268652238, 1.264615874, 1.260511287, 1.256340974, 1.252107348, 1.247812737, 1.243459393, 1.239049492, 1.234585143, 1.230068386, 1.2255012, 1.220885501, 1.216223152, 1.211515958, 1.206765674, 1.201974005, 1.19714261, 1.1922731, 1.187367045, 1.182425973, 1.177451372, 1.172444693, 1.167407348, 1.162340715, 1.157246139, 1.152124932, 1.146978373, 1.141807712, 1.136614169, 1.131398936, 1.126163178, 1.120908033, 1.115634612, 1.110344003, 1.105037268, 1.099715447, 1.094379556, 1.08903059, 1.083669521, 1.078297301, 1.072914859, 1.067523108, 1.062122937, 1.05671522, 1.05130081, 1.045880542, 1.040455233, 1.035025683, 1.029592675, 1.024156975, 1.018719333, 1.013280482, 1.00784114, 1.00240201, 0.996963778, 0.991527118, 0.986092686, 0.980661126, 0.975233068, 0.969809127, 0.964389904, 0.958975988, 0.953567954, 0.948166364, 0.942771769, 0.937384705, 0.932005696, 0.926635255, 0.921273883, 0.915922069, 0.91058029, 0.905249011, 0.899928687, 0.894619763, 0.88932267, 0.88403783, 0.878765656, 0.873506548, 0.868260896, 0.863029082, 0.857811477, 0.852608441, 0.847420326, 0.842247473, 0.837090216, 0.831948877, 0.826823771, 0.821715204, 0.816623471, 0.811548862, 0.806491654, 0.80145212, 0.796430522, 0.791427115, 0.786442146, 0.781475853, 0.776528467, 0.771600212, 0.766691304, 0.761801951, 0.756932355, 0.752082709, 0.747253202, 0.742444013, 0.737655316, 0.732887277, 0.728140056, 0.723413808, 0.71870868, 0.714024813, 0.709362343, 0.704721397, 0.700102099, 0.695504567, 0.690928912, 0.686375241, 0.681843653, 0.677334244, 0.672847104, 0.668382317, 0.663939963, 0.659520116, 0.655122846, 0.650748219, 0.646396293, 0.642067124, 0.637760764, 0.633477258, 0.629216649, 0.624978975, 0.620764269, 0.616572561, 0.612403877, 0.608258238, 0.604135661, 0.600036161, 0.595959749, 0.59190643, 0.587876209, 0.583869084, 0.579885053, 0.575924108, 0.571986239, 0.568071433, 0.564179674, 0.560310942, 0.556465216, 0.552642469, 0.548842674, 0.5450658, 0.541311815, 0.537580682, 0.533872363, 0.530186816, 0.526524, 0.522883867, 0.519266371, 0.51567146, 0.512099084, 0.508549187, 0.505021714, 0.501516605, 0.498033801, 0.49457324, 0.491134857, 0.487718587, 0.484324362, 0.480952113, 0.47760177, 0.47427326, 0.47096651, 0.467681443, 0.464417985, 0.461176056, 0.457955577, 0.454756467, 0.451578645, 0.448422028, 0.445286531, 0.442172069, 0.439078556, 0.436005904, 0.432954025, 0.429922829, 0.426912226, 0.423922125, 0.420952434, 0.41800306, 0.41507391, 0.412164889, 0.409275902, 0.406406853, 0.403557647, 0.400728185, 0.397918372, 0.395128108, 0.392357295, 0.389605834, 0.386873625, 0.384160569, 0.381466565, 0.378791512, 0.37613531, 0.373497856, 0.37087905, 0.368278789, 0.36569697, 0.363133492, 0.360588252, 0.358061147, 0.355552074, 0.353060929, 0.35058761, 0.348132012, 0.345694033, 0.343273569, 0.340870517, 0.338484772, 0.336116231, 0.333764791, 0.331430348, 0.329112798, 0.326812038, 0.324527964, 0.322260474, 0.320009464, 0.31777483, 0.315556471, 0.313354282, 0.311168162, 0.308998008, 0.306843717, 0.304705188, 0.302582319, 0.300475007, 0.298383152, 0.296306653, 0.294245408, 0.292199316, 0.290168278, 0.288152193, 0.286150961, 0.284164484, 0.28219266, 0.280235393, 0.278292582, 0.276364131, 0.27444994, 0.272549913, 0.270663953, 0.268791961, 0.266933843, 0.265089501, 0.263258841, 0.261441767, 0.259638183, 0.257847996, 0.256071111, 0.254307435, 0.252556875, 0.250819337, 0.249094729, 0.247382959, 0.245683935, 0.243997567, 0.242323763, 0.240662434, 0.239013489, 0.23737684, 0.235752397, 0.234140071, 0.232539776, 0.230951422, 0.229374924, 0.227810195, 0.226257148, 0.224715697, 0.223185759, 0.221667247, 0.220160077, 0.218664167, 0.217179432, 0.215705789, 0.214243158, 0.212791454, 0.211350598, 0.209920507, 0.208501103, 0.207092304, 0.205694032, 0.204306207, 0.202928752, 0.201561587, 0.200204635, 0.19885782, 0.197521065, 0.196194293, 0.19487743, 0.193570399, 0.192273127, 0.19098554, 0.189707563, 0.188439124, 0.187180149, 0.185930567, 0.184690306, 0.183459294, 0.182237461, 0.181024737, 0.179821051, 0.178626335, 0.177440519, 0.176263535, 0.175095315, 0.173935792, 0.172784898, 0.171642567, 0.170508733, 0.16938333, 0.168266293, 0.167157558, 0.166057059, 0.164964734, 0.163880519, 0.162804351, 0.161736168, 0.160675908, 0.159623508, 0.158578909, 0.157542048, 0.156512867, 0.155491306, 0.154477304, 0.153470803, 0.152471745, 0.151480071, 0.150495724, 0.149518647, 0.148548783, 0.147586076, 0.146630469, 0.145681907, 0.144740336, 0.143805699, 0.142877944, 0.141957016, 0.141042862, 0.140135428, 0.139234663, 0.138340513, 0.137452928, 0.136571854, 0.135697243, 0.134829041, 0.133967201, 0.133111671, 0.132262402, 0.131419345, 0.130582451, 0.129751671, 0.128926959, 0.128108265, 0.127295544, 0.126488747, 0.125687829, 0.124892743, 0.124103444, 0.123319885, 0.122542022, 0.121769811, 0.121003206, 0.120242164, 0.119486641, 0.118736594, 0.117991979, 0.117252754, 0.116518877, 0.115790306, 0.115066999, 0.114348914, 0.113636011, 0.112928249, 0.112225587, 0.111527986, 0.110835406, 0.110147807, 0.109465151, 0.108787398, 0.108114511, 0.10744645, 0.106783179, 0.106124659, 0.105470853, 0.104821725, 0.104177238, 0.103537355, 0.10290204, 0.102271258, 0.101644973, 0.10102315, 0.100405754, 0.09979275, 0.099184104, 0.098579782, 0.09797975, 0.097383975, 0.096792423, 0.096205061, 0.095621857, 0.095042778, 0.094467792, 0.093896867, 0.093329972, 0.092767075, 0.092208144, 0.091653149, 0.09110206, 0.090554845, 0.090011475, 0.08947192, 0.088936151, 0.088404136, 0.087875849, 0.087351259, 0.086830338};
float init_value = 95.05;

AudioInputI2S            i2s1;           //xy=204.00000381469727,247.00000381469727
AudioMixer4              mixer1;         //xy=402,248
AudioAnalyzeFFT1024      fft1024_1;      //xy=635.0000076293945,249.00000381469727
AudioConnection          patchCord1(i2s1, 0, mixer1, 0);
AudioConnection          patchCord2(i2s1, 1, mixer1, 1);
AudioConnection          patchCord3(mixer1, fft1024_1);
// GUItool: end automatically generated code

AudioControlWM8960 wm8960;

// オーディオシールドで使用する入力はどれですか？
#ifndef SEEED_WIO_TERMINAL 
const int myInput = AUDIO_INPUT_LINEIN;
#else
const int myInput = AUDIO_INPUT_MIC;
#endif

void setup(void) {
    tft.init();
    tft.setRotation(0);
    Serial.begin(57600); // デバッグ用
    spr.createSprite(240,160);
    tft.fillScreen(TFT_WHITE);
    tft.setTextColor(TFT_BLACK, TFT_RED);
    tft.setFreeFont(&FreeSansBoldOblique9pt7b);
    tft.drawString("dB value guidance", 40, 40); 
    tft.setTextColor(TFT_BLACK, TFT_YELLOW);
    tft.setFreeFont(&FreeSansBoldOblique9pt7b);
    tft.drawString("0 - 25 dB Whisper", 20, 80); 
    tft.drawString("25 - 50 dB Quite library", 20, 110); 
    tft.drawString("50 - 75 dB Loud Music", 20, 140); 
    tft.drawString("75 - 100 dB Motorcycle", 20, 170); 
    delay(5000);
    tft.fillScreen(TFT_WHITE);

    analogMeter(); // アナログメーターを描画

    updateTime = millis(); // 次の更新時間

    analogReadResolution(16);
  
    AudioMemory(60);
   
    fft1024_1.windowFunction(AudioWindowHanning1024);

    wm8960.enable();
    wm8960.inputSelect(myInput);


}

void loop() {

  if (fft1024_1.available()) {
      magnitude = 0;
      dB_holder = 0;
      float v[512] = {0};
      
      for (i=0; i<512; i++) {
        v[i] = fft1024_1.read(i) * a_weight[i];
        magnitude = magnitude + sq(v[i]);
      }
      magnitude = sqrt(magnitude);
      dB_holder = log10f(magnitude) * 20  + init_value;
      
  }
Serial.println(dB_holder); // f[23] = 1kHz, f[82] = 3.5kHz, f[252] = 12kHz

// #########################################################################
// 線グラフを表示
// #########################################################################
    spr.fillSprite(TFT_WHITE);
    if (data.size() == max_size) {
        data.pop();// 最初の読み取り変数を削除
    }
    data.push(dB_holder); // 変数を読み取り、データに格納

    spr2.createSprite(40, 40);
    spr2.fillSprite(TFT_WHITE);
    spr2.setTextColor(TFT_BLACK, TFT_WHITE);
    spr2.setFreeFont(&FreeSansBoldOblique18pt7b);
    //char buf_2[0]; dtostrf(dB_holder, 2, 1, buf_2);
    spr2.drawNumber(dB_holder, 0, 0); 
    spr2.pushSprite(80, 140); 
    spr2.deleteSprite();
    
    // 線グラフタイトルの設定
    auto header =  text(7, 0)
                .value("dB")
                .valign(vcenter)
                .width(tft.width())
                .thickness(2);
 
    header.height(header.font_height() * 1.1);
    header.draw(); // ヘッダーの高さはフォントの高さの2倍
 
  // 線グラフの設定
    auto content = line_chart(8, header.height()); //(x,y) 線グラフの開始位置
         content
                .height(tft.height() - header.height() * 12) // 線グラフの実際の高さ
                .width(tft.width() - content.x() * 2) // 線グラフの実際の幅
                .based_on(0.0) // y軸の開始点、floatで指定
                .show_circle(true) // 各ポイントに円を描画、デフォルトはオン
                .value(data) // データを線グラフに渡す
                .color(TFT_PURPLE) // 線の色を設定
                .draw();
                 
    spr.pushSprite(0, 190);
    delay(50);

// #########################################################################

    if (updateTime <= millis()) {
        updateTime = millis() + LOOP_PERIOD;
        int d;
        d += 4;
        if (d >= 360) {
            d = 0;
        }
        plotNeedle(dB_holder, 0);
    }
}

// #########################################################################
// 画面にアナログメーターを描画
// #########################################################################
void analogMeter() {
    // メーターのアウトライン
    tft.fillRect(0, 0, 239, 126, TFT_GREY);
    tft.fillRect(5, 3, 230, 119, TFT_WHITE);
    tft.setTextColor(TFT_BLACK);  // テキストの色

    // -50度から+50度まで5度ごとに目盛りを描画 (100度のフルスケールスイング)
    for (int i = -50; i < 51; i += 5) {
        // 長い目盛りの長さ
        int tl = 15;

        // 描画する目盛りの座標
        float sx = cos((i - 90) * 0.0174532925);
        float sy = sin((i - 90) * 0.0174532925);
        uint16_t x0 = sx * (100 + tl) + 120;
        uint16_t y0 = sy * (100 + tl) + 140;
        uint16_t x1 = sx * 100 + 120;
        uint16_t y1 = sy * 100 + 140;

        // 次の目盛りの座標をゾーン塗りつぶし用に計算
        float sx2 = cos((i + 5 - 90) * 0.0174532925);
        float sy2 = sin((i + 5 - 90) * 0.0174532925);
        int x2 = sx2 * (100 + tl) + 120;
        int y2 = sy2 * (100 + tl) + 140;
        int x3 = sx2 * 100 + 120;
        int y3 = sy2 * 100 + 140;

        // 黄色ゾーンの制限
        if (i >= -50 && i < 0) {
          tft.fillTriangle(x0, y0, x1, y1, x2, y2, TFT_GREEN);
          tft.fillTriangle(x1, y1, x2, y2, x3, y3, TFT_GREEN);
        }

        // 緑色ゾーンの制限
        if (i >= 0 && i < 25) {
            tft.fillTriangle(x0, y0, x1, y1, x2, y2, TFT_YELLOW);
            tft.fillTriangle(x1, y1, x2, y2, x3, y3, TFT_YELLOW);
        }

        // オレンジゾーンの制限
        if (i >= 25 && i < 50) {
            tft.fillTriangle(x0, y0, x1, y1, x2, y2, TFT_RED);
            tft.fillTriangle(x1, y1, x2, y2, x3, y3, TFT_RED);
        }

        // 短い目盛りの長さ
        if (i % 25 != 0) {
            tl = 8;
        }

        // 目盛りの長さが変更された場合に座標を再計算
        x0 = sx * (100 + tl) + 120;
        y0 = sy * (100 + tl) + 140;
        x1 = sx * 100 + 120;
        y1 = sy * 100 + 140;

        // 目盛りを描画
        tft.drawLine(x0, y0, x1, y1, TFT_BLACK);

        // ラベルを描画するかどうかをチェックし、位置を調整
        if (i % 25 == 0) {
            // ラベルの位置を計算
            x0 = sx * (100 + tl + 10) + 120;
            y0 = sy * (100 + tl + 10) + 140;
            switch (i / 25) {
                case -2: tft.drawCentreString("0", x0, y0 - 12, 2); break;
                case -1: tft.drawCentreString("25", x0, y0 - 9, 2); break;
                case 0: tft.drawCentreString("50", x0, y0 - 6, 2); break;
                case 1: tft.drawCentreString("75", x0, y0 - 9, 2); break;
                case 2: tft.drawCentreString("100", x0, y0 - 12, 2); break;
            }
        }

        // スケールの弧を描画、最後の部分は描画しない
        sx = cos((i + 5 - 90) * 0.0174532925);
        sy = sin((i + 5 - 90) * 0.0174532925);
        x0 = sx * 100 + 120;
        y0 = sy * 100 + 140;
        if (i < 50) {
            tft.drawLine(x0, y0, x1, y1, TFT_BLACK);
        }
    }

    tft.drawCentreString("dB", 120, 80, 4); // フォント4を避けるためコメントアウト
    tft.drawRect(5, 3, 230, 119, TFT_BLACK); // ベゼルラインを描画

    tft.setTextColor(TFT_BLACK, TFT_WHITE);
    tft.setFreeFont(&FreeSansBoldOblique18pt7b);
    tft.drawString(" dB",120,140);
    
    plotNeedle(0, 0); // メーターの針を0に配置
}
  


// #########################################################################
// 針の位置を更新
// この関数は針が動く間ブロックされます。時間は ms_delay に依存します。
// 10ms は、針のスイープエリア内にテキストが描画される場合、針のちらつきを最小限に抑えます。
// テキストがスイープエリア内にない場合は小さい値でも問題ありません。ゼロは即時更新ですが、
// リアルな見た目にはなりません...(注: フルスケール偏向で100インクリメント)
// #########################################################################
void plotNeedle(int value, byte ms_delay) {
    if (value < -10) {
        value = -10;    // 値を針のエンドストップに制限
    }
    if (value > 110) {
        value = 110;
    }

    // 新しい値に達するまで針を動かす
    while (!(value == old_analog)) {
        if (old_analog < value) {
            old_analog++;
        } else {
            old_analog--;
        }

        if (ms_delay == 0) {
            old_analog = value;    // 遅延が0の場合は即時更新
        }

        float sdeg = map(old_analog, -10, 110, -150, -30); // 値を角度にマップ
        // 針の先端の座標を計算
        float sx = cos(sdeg * 0.0174532925);
        float sy = sin(sdeg * 0.0174532925);

        // 針の開始位置の x のデルタを計算 (ピボットポイントから開始しない)
        float tx = tan((sdeg + 90) * 0.0174532925);

        // 古い針の画像を消去
        tft.drawLine(120 + 20 * ltx - 1, 140 - 20, osx - 1, osy, TFT_WHITE);
        tft.drawLine(120 + 20 * ltx, 140 - 20, osx, osy, TFT_WHITE);
        tft.drawLine(120 + 20 * ltx + 1, 140 - 20, osx + 1, osy, TFT_WHITE);

        // 針の下にあるテキストを再描画
        tft.setTextColor(TFT_BLACK);
        tft.drawCentreString("dB", 120, 80, 4); // フォント4を避けるためコメントアウト

        // 次の消去用に新しい針の端座標を保存
        ltx = tx;
        osx = sx * 98 + 120;
        osy = sy * 98 + 140;

        // 新しい位置に針を描画、マゼンタで針を少し太くする
        // 針を太くするために3本の線を描画
        tft.drawLine(120 + 20 * ltx - 1, 140 - 20, osx - 1, osy, TFT_RED);
        tft.drawLine(120 + 20 * ltx, 140 - 20, osx, osy, TFT_MAGENTA);
        tft.drawLine(120 + 20 * ltx + 1, 140 - 20, osx + 1, osy, TFT_RED);

        // 新しい位置に近づくにつれて針を少し遅くする
        if (abs(old_analog - value) < 10) {
            ms_delay += ms_delay / 5;
        }
        // 次の更新まで待機
        delay(ms_delay);
    }
}
```
