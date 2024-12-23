# 10×7ドットマトリックスLEDモジュール（TM1628A)  

https://massa4649.booth.pm/items/6223956 の製品説明となります。  

## 概要
TM1626AをLEDドライバとして実装した汎用的に使用できる10×7ドットマトリックスLEDディスプレイモジュールです。   
筐体への固定用ネジ穴（Φ３ｍｍ）を４つ設けています。  

【LED実装面】  
![omote1](https://github.com/user-attachments/assets/8b613744-a243-4c68-8b66-2ef13d9506a6)  
【IC実装面】  
![ura](https://github.com/user-attachments/assets/dba3b079-3ac5-4c19-b819-b0250c426493)  

## 仕様  
・サイズ：59 × 28 × 11 ㎜ typ(端子部除く)  
・動作電圧：5V(Typ)  
・外部端子：DIO、CLOCK、STB、VCC、GND  
・LED色   ：赤色(STANLEY製）  
・搭載LEDドライバーIC：TM1628A(TITAN MICRO ELECTRONICS)  
・ピンヘッダー（L型 1x5)：１個同梱（未実装）   
・ピンヘッダー（ストレート型 1x5)：１個同梱（未実装）   

※ピンヘッダーはどちら一方（またはご自身で用意した物）を、お好きな取り付け方向で半田付けして下さい。  
※ロットにより、部品名称や色あい、細かいデザインや形が微妙に変わることがあります。  

【LED未実装品】をご購入した場合  
  MATRIX　LEDはご自身で半田付けする必要があります。  
  LED D1およびD2の半田付け方向は、次の写真の通りです。  
  LEDの印字方向をよく確認して間違えないように半田付けをお願いします。  
![MATRIX-direction-30per](https://github.com/user-attachments/assets/5489bcb1-e060-4da9-89ee-9b04e39be6ca)

## 参考回路図  
![circuit](https://github.com/user-attachments/assets/abdf0add-3d86-4d39-b122-b65f4f6e194d)  

・2024年10月現在において、TM1628AのデータシートはArduino.CCで紹介されているライブラリの添付資料として提供されていますのでご参照下さい。  
  参照サイト：https://docs.arduino.cc/libraries/tm16xx-leds-and-buttons/  

## 動作確認  
・上記Arduino.CCのサイトからリンク先repositoryで提供されているライブラリに含まれるサンプルスケッチを使用しました。  

```
#include <Adafruit_GFX.h>
#include <TM1628.h>
#include <TM16xxMatrixGFX.h>

TM1628 module(8, 9, 7);   // DIO=8, CLK=9, STB=7
#define MODULE_SIZECOLUMNS 7
#define MODULE_SIZEROWS 10
TM16xxMatrixGFX matrix(&module, MODULE_SIZECOLUMNS, MODULE_SIZEROWS);    // TM16xx object, columns, rows

String tape = "Arduino";
int wait = 100; // In milliseconds

int spacer = 1;
int width = 5 + spacer; // The font width is 5 pixels

void setup()
{
  matrix.setIntensity(1); // Use a value between 0 and 7 for brightness
  matrix.setRotation(0); 
  matrix.setMirror(false);   // set X-mirror true when using the WeMOS D1 mini Matrix LED Shield (X0=Seg1/R1, Y0=GRD1/C1)
}

void loop()
{
  for ( int i = 0 ; i < width * tape.length() + matrix.width() - 1 - spacer; i++ ) {
    matrix.fillScreen(LOW);

    int letter = i / width;
    int x = (matrix.width() - 1) - i % width;
    int y = (matrix.height() - 8) / 2; // center the text vertically

    while ( x + width - spacer >= 0 && letter >= 0 ) {
      if ( letter < tape.length() ) {
        matrix.drawChar(x, y, tape[letter], HIGH, LOW, 1);
      }

      letter--;
      x -= width;
    }
    matrix.write(); // Send bitmap to display
    delay(wait);
  }
}
```

・Tikerを動作させた場合の動画をご紹介します。  
　　
[![紹介動画]()](https://youtu.be/ylSofqe5cog)  

## 関連ブログ
https://massa4649.com/tm1628a_1/

以上です。
