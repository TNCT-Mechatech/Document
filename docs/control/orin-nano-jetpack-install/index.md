# Jetpack を導入する方法について

##### 環境：Jetson Orin Nano Developer Kit

- ### 概要

NVMe(SSD)にいきなりJetpackを導入することはできない(と思われる)ので、MicroSDカードにjetpackを書き込み、そこからNVMeに書き込みをする方向になります。
~~JetPack SDKはクソ~~

- ### SDカードにJetPackを書き込む。

適当なFlashをするソフト(自分は[balenaEtcher](https://etcher.balena.io/))を使ってファイルを焼きます。

まず、[JetPack Archive](https://developer.nvidia.com/embedded/jetpack-archive) にアクセスし、適当なバージョンのJetPackをクリックし、`SD Card Image Method` → `JETSON Orin Nano DEVELOPER KIT`をクリックして、ダウンロードします。

MicroSDカードをカードリーダー等を用いてPCに接続し、Flashします。

そしてMicroSDカードをJetsonに挿して、通常通り起動します。

- ### NVMe(SSD)にJetPackを書き込む。

以下は試行錯誤途中なので省略。また書きます


