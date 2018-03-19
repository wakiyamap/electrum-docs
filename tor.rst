Using Electrum Through Tor
==========================================================
Torを通してElectrumを使用する
==========================

Please note that when using electrum through tor there are two main ways.
The first way has the most Privacy but also requires the most trust in the server you are connecting too. This is because normally
Electrum connects to a few different servers and downloads block headers and checks that they match. This prevents / makes it more difficult for 
Rogue servers to send you bad information. However this can also present a privacy issue because you could be connecting to none .onion servers for these headers.

Torを通してElectrumを使用するには二つの主な方法があります。一つ目の方法は最も秘匿されますが接続するサーバを最も信用する必要もあります。なぜなら通常、Electrumはわずかな数のサーバーに接続してブロックヘッダをダウンロート、検証しているからです。これが悪意あるサーバがあなたに偽物の情報を送信することを難しくしています。しかしながら、これらのヘッダを求めて.onionサーバに接続することができるゆえにプライバシーの問題を発生させています。

Thus the two different options are, Connect to 1 server ONLY and get block headers and transaction info from that server.
Or
Connect to 8 block header servers and connect to 1 .onion server for the general use.

このように二つの異なるオプションは、1つのサーバだけに接続しブロックヘッダとトランザクション情報をサーバから取得する方法と、八つのブロックヘッダサーバに接続し一つの.onionサーバに一般用途のために接続する方法です。

List of Active .onion servers
-----------------------------
Check the list here;

アクティブな.onionサーバのリスト
-----------------------------
ここのリストをチェックしてください

http://electrumserv.noip.me/onionservers.txt

If you wish to be added to this list email me at:

このリストに追加したい場合次にメールを送ってください：

danielcryptos@gmail.com


LINUX
-----

Option 1: Single Server
-----------------------
オプション 1: 単一サーバ
----------------------

Note: Please understand you are sacrificing some security here for extra privacy.

注意：追加プライバシーのためにはいくつかのセキュリティを犠牲にすることを理解してください。



Check out https://electrum.org/#download

https://electrum.org/#downloadをチェックします

Grab the download from python source

Pythonのソースからdownloadをつかみます

Make sure you have dependencies installed

依存関係をインストールしていることを確認します

sudo apt-get install python-qt4 python-pip

Extract the electrum download;

ダウンロードしたElectrumを展開します

tar -xvzf Electrum-2.*.*.tar.gz


Go into the extracted electrum folder and then run;

展開したElecturmフォルダに移動して起動します；

./electrum -1 -s electrums3lojbuj.onion:50001:t -p socks5:localhost:9050


Quick explanation,

クイック説明

-1 means connect to 1 server only.

-1は一つのサーバだけに接続することを意味します

-s is defining which server. You can change this to any .onion server you want. (Check the list at the top)

-sはサーバを定義します。あなたの望む.onionサーバに変更することができます（トップのリストをチェックしてください）

-p Is saying what proxy server to use to get into the tor network. Generally this will be localhost but the port bit after : could be different.

-pはTorネットワークに参加するために使用するプロキシサーバを決めます。一般的にこれはlocalhostですが後のポートビットは異なる可能性があります。

You might need to change the port bit depending on what system you are running;

実行しているシステムによってはポートビットを変更する必要があります。

Currently The port is;

Tor Browser Bundle: 9150

General Tor (Installed): 9050

現在のポートは；

Torブラウザバンドル： 9150

一般的なTor(インストールされたもの)： 9050



Option 2: Multiple servers but Tor Main
---------------------------------------
Same as above until the command to launch electrum, Remove the -1 making it
オプション 2：複数のサーバー。ただしTorがメイン
------------------------------------------
Electrumをスタートするコマンドまでは同じで、-1を削除

./electrum -s electrums3lojbuj.onion:50001:t -p socks5:localhost:9050

For this one you can also just launch electrum and click on the Green or Red icon on the bottom right to bring up server information

これでElectrumを起動し右下にある緑または赤のアイコンをクリックすることでサーバ情報を持ってくることができます。

Untick the box for Auto and enter;

「Auto」のチェックを外しボックスの中に入力してください；

electrums3lojbuj.onion

50001

Into the boxes.

At the bottom select SOCKS5 for proxy and then

下部のSOCKS5をプロキシに選択し、次は

localhost

9150 or 9050


WINDOWS
-------

Option 1: Connecting to a single Server
---------------------------------------
Install electrum from the main download page;
https://electrum.org/#download

オプション1：単一サーバに接続
--------------------------
Electrumをメインダウンロードページからインストール
https://electrum.org/#download

Note: Please understand you are sacrificing some security here for extra privacy.

注意：追加プライバシーのためにはいくつかのセキュリティを犠牲にすることを理解してください。

In windows, On your desktop you will have a electrum icon. Copy and paste this to make a copy. If not you can find the electrum folder in C:\Program Files (x86)\Electrum\

Windowsでは、デスクトップにElectrumアイコンがあるしょう。C:\Program Files (x86)\Electrum\
にElectrumフォルダを見つけられない場合、コピーを作成するためにコピー＆ペーストしてください。

Right click on electrum.exe and create shortcut. It will say cannot make a shortcut here make one on the desktop instead? Ok this.

electrum.exeを右クリックしてショートカットを作成してください。ここにはショートカットを作成できないと言われるのでデスクトップに代わりを作成するにOKしてください。

With your new shortcut or a copy of your old one Right click it and go properties, click shortcut at the top bar, in the box named target:

新しいショートカットまたは古いもののコピーを右クリックしてプロパティに進みます。トップバーのショートカットをクリック、target（リンク先）という名前のボックス内をクリックします。

It should already say something similar to what’s in between the speech bubbles. If yours is different don’t change that bit to match.

それはすでにふきだし(speech bubble)のものと似た何かが書かれているはずです。もし違ったとしても一致させようとして変更しないでください。

What we want to do is add on the bit after the last speech bubble. Make a space and then enter / copy and paste.

我々の目的は最後のふきだしの後にビットを追加することです。スペースを入れて / を入力したらコピー＆ペーストします。

"C:\Program Files (x86)\Electrum\electrum.exe" -1 -s electrums3lojbuj.onion:50001:t -p socks5:localhost:9050

Apply and Ok the change... You can go back to the General Tab if you want and Where it says "electrum.exe - Shortcut" you could change that to Electrum - Tor or something

変更を適用しOKしたら、貴方が望むなら一般タブに戻って"electrum.exe - Shortcut"と書かれているものをElectrum - Tor等に変更してよいでしょう。

Click apply and ok again.

もう一度適用、OKをクリックしてください。

Now when you launch Electrum with this shortcut it will use 1 tor server only.

これでこのショートカットからELectrumを起動した場合、一つのTorサーバだけを使用するようになります。

Quick explanation,

クイック説明

-1 means connect to 1 server only.

-1は一つのサーバのみに接続することを意味します。

-s is defining which server. You can change this to any .onion server you want.

-sはサーバを定義します。あなたの望む.onionサーバに変更することができます（トップのリストをチェックしてください）

-p Is saying what proxy server to use to get into the tor network. Generally this will be localhost but the port bit after : could be different.

-pはTorネットワークに参加するために使用するプロキシサーバを決めます。一般的にこれはlocalhostですが後のポートビットは異なる可能性があります。

You might need to change the port bit depending on what system you are running;

実行しているシステムによってはポートビットを変更する必要があります。

Currently The port is;


Tor Browser Bundle: 9150

General Tor (Installed): 9050

現在のポートは；

Torブラウザバンドル： 9150

一般的なTor(インストールされたもの)： 9050

Option 2
----------
In windows, On your desktop you will have a electrum icon. Copy and paste this to make a copy. If not you can find the electrum folder in C:\Program Files (x86)\Electrum\

オプション 2
-----------
Windowsでは、デスクトップにElectrumアイコンがあるしょう。C:\Program Files (x86)\Electrum\
にElectrumフォルダを見つけられない場合、コピーを作成するためにコピー＆ペーストしてください。

Right click on electrum.exe and create shortcut. It will say cannot make a shortcut here make one on the desktop instead? Ok this.

electrum.exeを右クリックしてショートカットを作成してください。ここにはショートカットを作成できないと言われるのでデスクトップに代わりを作成するにOKしてください。

With your new shortcut or a copy of your old one Right click it and go properties, click shortcut at the top bar, in the box named target:

新しいショートカットまたは古いもののコピーを右クリックしてプロパティに進みます。トップバーのショートカットをクリック、target（リンク先）という名前のボックス内をクリックします。

It should already say something similar to what’s in between the speech bubbles. If yours is different don’t change that bit to match.

それはすでにふきだし(speech bubble)のものと似た何かが書かれているはずです。もし違ったとしても一致させようとして変更しないでください。

What we want to do is add on the bit after the last speech bubble. Make a space and then enter / copy and paste.

我々の目的は最後のふきだしの後にビットを追加することです。スペースを入れて / を入力したらコピー＆ペーストします。

"C:\Program Files (x86)\Electrum\electrum.exe" -s electrums3lojbuj.onion:50001:t -p socks5:localhost:9050

Apply and Ok the change... You can go back to the General Tab if you want and Where it says "electrum.exe - Shortcut" you could change that to Electrum - Tor or something

変更を適用しOKしたら、貴方が望むなら一般タブに戻って"electrum.exe - Shortcut"と書かれているものをElectrum - Tor等に変更してよいでしょう。

Click apply and ok again.

もう一度適用、OKをクリックしてください。

Now when you launch Electrum with this shortcut it will use 1 tor server only.

これでこのショートカットからELectrumを起動した場合、一つのTorサーバだけを使用するようになります。

You might need to change the port bit depending on what system you are running;

実行しているシステムによってはポートビットを変更する必要があります。

Currently The port is;

Tor Browser Bundle: 9150

General Tor (Installed): 9050

現在のポートは；

Torブラウザバンドル： 9150

一般的なTor(インストールされたもの)： 9050


For this one you can also just launch electrum and click on the Green or Red icon on the bottom right to bring up server information
Untick the box for Auto and enter;

これでElectrumを起動し右下にある緑または赤のアイコンをクリックすることでサーバ情報を持ってくることができます。「Auto」のチェックを外しボックスの中に入力してください；

electrums3lojbuj.onion

50001

Into the boxes.

At the bottom select SOCKS5 for proxy and then


下部のSOCKS5をプロキシに選択し、次は

localhost

9150 or 9050
