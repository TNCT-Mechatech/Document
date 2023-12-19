# 開発環境

## 回路設計

回路班では機械班が利用するSolidworksやFusionといったcadと同様に、回路版のcadとしてEagleやkicad（Fusionを使うこともある）があります。メカテック部内での使い分けとしては、ユニバーサル基板で回路を組むだけの設計ならEagleだけで設計を行い、プリント基板を作る際は最初はEagleで回路を設計し、細かい設定がしやすいKicadの方にデータを移行してPCB(print-circuit-board)を設計を完成させて、JLCPCBという基板製造業者に基板を発注する形式です。
Eagleとkicadのダウンロードリンクは下に参照しておきます。

Eagle：https://www.autodesk.co.jp/products/eagle/free-download
kicad：https://www.kicad.org

[!注意]
Eagleについて**2026年6月7日**をもってオートデスクはEAGLEの販売およびサポートを終了して使えなくなるため早めのkicadへの移行が必然です。


## その他回路で役立つツール

# STM32CubeMX

回路で使用するピンアサインの設定に戸惑うことも多いと思いますが、その際に役立つのがCubeMX-IDです。直接ICに模したLibraryに細かいPin設定を行えるだけでなくピン設定の重複を避けてくれるため、とても有能なツールです。
STM32CubeMXのダウンロードリンクは下に参照しておきます。

STM32CubeMX：https://www.st.com/content/st_com/ja/stm32cubemx.html

# LT-spice

回路をデジタルでシミュレートして波形を見ることができ、あらかじめ回路動作の確認が行え、また解析の仕方にもトランジェント解析やAC解析、DC解析と言ったようにある程度のことはすべてできてしまうツールです。
※あくまでもデジタル上のため、理想での測定であり、実際には発熱やノイズ対策、その他の外的要因等の考慮しないといけないことは多くあります。基本的な部品の性質や静特性、動特性、周波数特性等を学ぶのには最適です。
LT-spiceのダウンロードリンクは下に参照しておきます。

LTspice：https://www.analog.com/jp/design-center/design-tools-and-calculators/ltspice-simulator.html

# Tera-Term

TeraTermは『SSH』などの通信プロトコルを通じてサーバーの遠隔操作するのが主な使い方で、回路班としてはtweliteでシリアル通信するためにシリアルのポート設定でインタラクティブモードにし、周波数チャネル設定等を変更するときに使います。
TeraTermのダウンロードリンクは下に参照しておきます。

TeraTerm；https://github.com/TeraTermProject/teraterm/releases/tag/v5.1


## 回路で利用するサイト

# 秋月電子通商

主に電子部品の注文で利用します。**送料は一律で500円ほどで代引き手数料だと300円の追加料金**となり、商品は早くて３日から遅くても１週間ほどで届くため頻繁に利用します。
サイトのURLは下に参照しておきます。

https://akizukidenshi.com/catalog/

# Digikey

秋月電子同様に電子部品の注文で利用します。違いとしては、秋月よりも電子部品の種類が豊富で必要とする部品を細かいカテゴリで検索して選定できる点です。しかしデメリットとしては、**6000円以上の買い物で送料は無料となり、基本送料は2000円もかかる**ところにあります。また、商品のお届けも最低でも一週間ほどはかかります。
サイトのURLは下に参照しておきます。

https://www.digikey.jp/

# Mouser

同様に電子部品の注文で利用します。MouserもDigikeyと同程度の電子部品の種類があり、違う点ではDigikeyは送料込みで一万円以内であれば消費税免除されるのに対し、Mouserは2020年12月ほどからの変更で値段に限らず地方消費税と内国消費税がかかります。よってサイトの利用の使い分けとしては、**6000~10000円内ではDigikey、10000円以上はMouserがベスト**だと思われます。
サイトのURLは下に参照しておきます。

https://www.mouser.jp/

# DigikeyのTech_Forum

ここでは様々なトピックについて議論していたり、基本的な電子部品についての概要が書かれています。
サイトのURLは下に参照しておきます。

https://forum.digikey.com/c/japanese/43


