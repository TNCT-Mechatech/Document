# Ticker

定期的に処理を行いたい場合(**割り込み処理**)に強力な API が Ticker です。

## Mbed 公式リファレンス

- [https://os.mbed.com/docs/mbed-os/v6.16/apis/ticker.html](https://os.mbed.com/docs/mbed-os/v6.16/apis/ticker.html)

## 注意点

Ticker を使った割り込み処理には `printf()` などといった重たい処理を行うことができません。

## メンバー関数

### attach

`Ticker` に呼び出したい関数を登録するための関数です。

```cpp
attach(Callback<void()> func, std::chrono::microseconds t)
```

#### func

呼び出したい関数(**コールバック関数**)を指定するための引数です。  
`func` には以下のように `callback()` を使って指定します。

```cpp
callback(&関数名)
```

#### t

どの頻度でコールバック関数を呼び出すか指定するための引数です。

0.1 秒なら `100ms` と指定します。

```cpp title="attach 例"
#include "mbed.h"

//  Ticker変数
Ticker ticker;

//  toggleLed関数用のLED
DigitalOut led(A0);
//  今回呼び出したいfunc関数
void func()
{
    //  ON/OFF切り替え
    led = !led;
};

int main()
{
    //  toggleLed関数を0.1秒おきに呼び出す
    ticker.attach(callback(&toggleLed), 100ms);

    while (1)
    {
        //  ...
    }

    return 0;
}
```

### detach

`attach` した関数の呼び出しを以後呼び出さないようにする関数です。

```cpp
detach()
```

## 活用例

### 7 セグメント LED

一般的な 7 セグメント LED を光らすプログラムがあります。  
もちろんこのままでも動きますが、ループ内で複雑な処理をしたい時にこれらのコードが邪魔になる場合があります。

```cpp title="一般的な7セグメントLEDのプログラム"
#include "mbed.h"

DigitalOut hundreds_digit(D5), tens_digit(D6), one_digit(D7);
DigitalOut led_a(D4), led_b(D2), led_c(A4), led_d(A6), led_e(A7), led_f(D3), led_g(A3), led_dp(A5);

//  7セグメントLED配列
static const int number_style[11][7] = {
        {1,1,1,1,1,1,0},
        {0,1,1,0,0,0,0},
        {1,1,0,1,1,0,1},
        {1,1,1,1,0,0,1},
        {0,1,1,0,0,1,1},
        {1,0,1,1,0,1,1},
        {1,0,1,1,1,1,1},
        {1,1,1,0,0,0,0},
        {1,1,1,1,1,1,1},
        {1,1,1,1,0,1,1},
        {0,0,0,0,0,0,0}
    };

//  指定された数字(１桁)を表示する関数
void display_num(int num)
{
    //  0 - 9 range
    if(0 <= num && num < 10) {
        led_a = number_style[num][0];
        led_b = number_style[num][1];
        led_c = number_style[num][2];
        led_d = number_style[num][3];
        led_e = number_style[num][4];
        led_f = number_style[num][5];
        led_g = number_style[num][6];
    }
}

//  各桁の数字
int values[3];
//  表示する桁
int digit;

int main() {
    //  0 - 999 の範囲を1秒ずつカウントアップ
    for(int i = 0; i < 1000; i++) {
        //  各桁の数字を計算
        values[0] = i / 100;
        values[1] = i % 100 / 10;
        values[2] = i % 10;

        //  40回繰り返す
        for(int j = 0; j < 40; j++)
        {
            //  表示する桁を計算
            digit = j % 3;

            //  表示する桁を変更
            hundreds_digit = digit == 0;
            tens_digit = digit == 1;
            one_digit = digit == 2;

            //  表示する数字を変更
            display_num(values[digit]);

            //  25ms待つ
            wait_us(25*1000);
        }
        //  結果として1秒ずつカウントアップできる。
    }

    return 0;
}
```

そこで、Ticker を使って定期的に光らせるように変更してみましょう。  
`main`関数のループ内で 7 セグメント LED の処理を行うことなく、表示することができます。

```cpp title="Tickerを用いた7セグメントLEDのプログラム"
#include "mbed.h"

DigitalOut hundreds_digit(D5), tens_digit(D6), one_digit(D7);
DigitalOut led_a(D4), led_b(D2), led_c(A4), led_d(A6), led_e(A7), led_f(D3), led_g(A3), led_dp(A5);

//  可変抵抗の値を7セグメントLEDに光らすため
AnalogIn voltage(A1);

//  Ticker
Ticker ticker;

//  7セグメントLED配列
static const int number_style[11][7] = {
        {1,1,1,1,1,1,0},
        {0,1,1,0,0,0,0},
        {1,1,0,1,1,0,1},
        {1,1,1,1,0,0,1},
        {0,1,1,0,0,1,1},
        {1,0,1,1,0,1,1},
        {1,0,1,1,1,1,1},
        {1,1,1,0,0,0,0},
        {1,1,1,1,1,1,1},
        {1,1,1,1,0,1,1},
        {0,0,0,0,0,0,0}
    };

//  指定された数字(１桁)を表示する関数
void display_num(int num)
{
    //  0 - 9 range
    if(0 <= num && num < 10) {
        led_a = number_style[num][0];
        led_b = number_style[num][1];
        led_c = number_style[num][2];
        led_d = number_style[num][3];
        led_e = number_style[num][4];
        led_f = number_style[num][5];
        led_g = number_style[num][6];
    }
}

//  表示する数字
int number;
//  各桁の数字
int values[3];
//  表示する桁
int digit;
//  現在の表示している桁
int current_digit = 0;

//  attach関数に渡す7セグメントLEDを光らすコールバック関数
void segment_led_callback()
{
    //  各桁の数字を計算
    values[0] = number / 100;
    values[1] = number % 100 / 10;
    values[2] = number % 10;

    //  表示する桁を計算
    digit = current_digit % 3;

    //  表示する桁を変更
    hundreds_digit = digit == 0;
    tens_digit = digit == 1;
    one_digit = digit == 2;

    //  表示する数字を変更
    display_num(values[digit]);

    //  次に表示する桁を変えるためにカウントアップ
    current_digit++;
}

int main() {
    //  25msおきに`segment_led_callback`を呼び出すようにattachする
    ticker.attach(callback(&segment_led_callback), 25ms);

    while (1)
    {
        //  0~1.0の範囲で取得できるので、0-999の範囲に変更
        number = voltage.read() * 999;
    }
    return 0;
}
```
