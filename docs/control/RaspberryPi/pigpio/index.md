# GPIOを用いた制御

- ## GPIO とは

GPIOは、General Purpose Input/Output の略である。Raspberry Piを含むコンピュータに各GPIO端子には特定の機能が備わっており、ユーザーの意思で自由に電気信号の入出力を操作できる。

# 本題

## pigpio

読み方は*パイ・ジーピーアイオー*である。決して*ぴぐぴお*なんてへなちょこな名前ではない。

- ### 概要

pigpio とは Raspberry Pi のGPIOを用いた制御に用いるライブラリである。調べた限りでは、C++, Pythonに対応している。

- ### 導入

導入は[pigpio公式サイト](https://abyz.me.uk/rpi/pigpio/)を参考に行う。

1. **本体のインストール**

ターミナルで以下のコマンドを実行する。
```
wget https://github.com/joan2937/pigpio/archive/master.zip #githubからpigpio本体をダウンロード
unzip master.zip   #ダウンロードした pigpio本体を解凍
cd pigpio-master   #新しくディレクトリに移動する。(ない場合は作成すること。->mkdir)
make
sudo make install  #インストール
```

すると pigpio が Raspberry Pi にインストールされる。

(Python で制御をしたいときはまた別にインストールが必要なものがあるとかないとか。)

2. **環境変数の継承と追加(ros2を使う場合のみ)**

次に、sudoユーザーがros2 humble等を認識できるように環境変数を設定する必要がある。

ターミナルで以下のコマンドを実行する。
```
nano /etc/environment
```

すると、以下のような画面になるはずである。
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

ここはシステム全体のデフォルトの環境変数を定義する場所である。初期設定では PATH 環境変数のみが記されている。

ここをこう書き換える。

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
LD_LIBRARY_PATH="/opt/ros/humble/lib:/usr/local/lib:/usr/lib"
PYTHONPATH="/opt/ros/humble/lib/python3.10/site-packages:/opt/ros/humble/local/lib/python3.10/dist-packages"
```

こうすることによって、root含め、すべてのユーザーが、書き足した`LD_LIBRARY_PATH`と`PYTHONPATH`を使うことができるようになる。

最後に、rootユーザーとしてのノードの実行に必要な機能を使うために環境変数を追加していく。

ターミナルで以下のコマンドを実行する。

```
sudo visudo
```

するとこのような画面になる。

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty

# This preserves proxy settings from user environments of root
# equivalent users (group sudo)
#Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"

# This allows running arbitrary commands, but so does ALL, and it means
# different sudoers have their choice of editor respected.
#Defaults:%sudo env_keep += "EDITOR"

#続く...
```

ここはsudoコマンドの動作や権限を管理する重要な設定ファイルであり、ユーザーやグループがsudoを使ってコマンドを実行する際のルールが記述されている。

これをこう書き換える。

```
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty
Defaults        env_keep += "PYTHONPATH"
Defaults        env_keep += "LD_LIBRARY_PATH"
Defaults        env_keep += "PATH"
Defaults        env_keep += "AMENT_PREFIX_PATH"
```

追加する部分は`Defaults env_pty`の下の四つのみであり、そのほかの`#`コメントアウト等は書き換えなくてよい。

以上で導入は終わりである。たぶん。