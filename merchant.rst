Electrumを使ったWebサイト上でのMonacoinの受け取り方
================================================

このチュートリアルではSSL化された支払いリクエストを使ってWebサイト上で BIP-70_ に基づいたMonacoinを受け取る方法を説明しています。Electrum2.6.で更新されています。
このドキュメントは3.1.2で更新されています。

.. _BIP-70:
    https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki

要件
----

* static HTMLを提供するWebサーバ
* SSL証明書（CA(認証局)による署名 例えば Letsencrypt_）
* バージョン3.1以上のElectrum
* Electrum-Merchant_

.. _Letsencrypt:
    https://letsencrypt.org/
.. _Electrum-Merchant:
    https://pypi.org/project/electrum-merchant/

商業用のウォレットを作成、使用する
-----------------------------

暗号通貨を安全に保存しておきたい場合は、保護されたマシン上にウォレットを作成してください。もしあなたの商業用サーバに誰かが侵入した場合、彼又は彼女は読み取り専用のウォレットにアクセスすることができるだけでコインを使用することはできません。
しかし内部に隠れた侵入者は依然としてあなたのアドレス、取引、残高を監視することができます。商業目的には（あなたのメインウォレットではなく）分けたウォレットを使用することが推奨されています。

.. code-block:: bash

   electrum create

保護されたマシン上でマスター公開鍵(xpub)をエクスポートする：

.. code-block:: bash

   electrum getmpk -w .electrum/wallets/your-wallet


これで商業用Electrumデーモンを設定することができるようになりました。

サーバマシン上で先ほどエクスポートしたマスター公開鍵(xpub)を復旧します。

.. code-block:: bash

   electrum restore xpub...............................................

あなたの読み取り専用のウォレットを（再）復旧したらElectrumをデーモンとして起動します：

.. code-block:: bash

   electrum daemon start
   electrum daemon load_wallet

設定にSSL証明書を追加する
-----------------------

あなたのドメイン用の秘密鍵と公開証明書を持っている必要があります。
これはウォレット・キーではなく、一致するTLS/SSL証明書の秘密キーであることに注意してください。


秘密鍵だけを含んだファイルを作成してください。

.. code-block:: openssl

   -----BEGIN PRIVATE KEY-----
   your private key
   -----END PRIVATE KEY-----


setconfigコマンドで秘密鍵のファイルへのパスを指定してください。

.. code-block:: bash

   electrum setconfig ssl_privkey /path/to/ssl.key


別のファイルを作成し、あなたの証明書とルートの認証局まで依存する証明書のリストを含めてください。あなたの証明書はリストの最初、ルートの認証局はリスト最後にある必要があります。

.. code-block:: openssl

   -----BEGIN CERTIFICATE-----
   your cert
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   intermediate cert
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   root cert
   -----END CERTIFICATE-----


setconfigコマンドでssl_chainまでのパスを指定してください。

.. code-block:: bash

   electrum setconfig ssl_chain /path/to/ssl.chain


リクエストディレクトリを設定する
-----------------------------

このディレクトリはあなたのWebサーバに設置されなければなりません。（Apache や Nginx 等）

.. code-block:: bash

   electrum setconfig requests_dir /srv/www/payment/

デフォルトではElectrumは'file://'で始まるローカルのURLを表示します。公開されたURLを表示するためには、url_rewriteを設定する必要があります。例えば、

.. code-block:: bash

   electrum setconfig url_rewrite "[ 'file:///srv/www/', 'https://example.com/' ]"

Webサーバーは、支払要求を処理できるように準備しておく必要があります。
configuration for Nginx:

.. code-block:: nginx

    location /payment/ {
        default_type "application/bitcoin-paymentrequest";
        alias /srv/www/payment/;
    }
    
Or for Apache 2:

.. code-block:: apache

    <Directory /srv/www/payment/requests/>
        ForceType application/bitcoin-paymentrequest
    </Directory>

その他のWebサーバーについては、ディレクトリに基づいてデフォルトのMIMEタイプを変更する方法について、マニュアルを参照してください。

Electrum-Merchant をインストールする
-------------------------

Electrum-Merchant_ をインストールして走らせるプログラム。 デフォルトでは、単純なインタフェースがインストールされます。
他のインタフェースは準備中であり、将来利用可能になる予定です。

.. code-block:: bash

    pip3 install electrum-merchant
    python3 -m electrum-merchant

Electrum-Merchant_ を正常に実行するには、前述の手順に従う必要があることに注意してください。

署名された支払いリクエストを作成する
---------------------------------

.. code-block:: bash

   electrum addrequest 3.14 -m "this is a test"
   {
      "URI": "bitcoin:1MP49h5fbfLXiFpomsXeqJHGHUfNf3mCo4?amount=3.14&r=https://example.com/payment/7c2888541a",
      "address": "1MP49h5fbfLXiFpomsXeqJHGHUfNf3mCo4",
      "amount": 314000000,
      "amount (BTC)": "3.14",
      "exp": 3600,
      "id": "7c2888541a",
      "index_url": "https://example.com/payment/index.html?id=7c2888541a",
      "memo": "this is a test",
      "request_url": "https://example.com/payment/7c2888541a",
      "status": "Pending",
      "time": 1450175741
   }


このコマンドは二つのURLとともにjsonオブジェクトを返します：

 - request_urlは署名されたBIP70リクエストのURL
 - index_urlはリクエストを表示するWebページのURL
 
request_urlとindex_urlはurl_rewriteに定義したドメイン名を使用することに気を付けてください。

'listrequests'コマンドを使用することで現在のリクエストの一覧を閲覧することができます。

ブラウザで支払いリクエストのページを開く
------------------------------------

index_urlをWebブラウザで開いてみましょう。

.. image:: png/payrequest.png


このページには支払いリクエストが載っています。ウォレットでMonaocin: URIを開くか、QRコードをスキャンすることが出来ます。最後の行はリクエストの期限が切れるまでに残された時間を表しています。

.. image:: png/payreq_window.png

既にこのページは支払いを受け取るために使用できます。ただしリクエストが支払われたかどうかは検出しません。そのためにはWebソケットを設定する必要があります。

Webソケットのサポートを追加する
----------------------------

SimpleWebSocketServerをここから入手してください：

.. code-block:: bash

    git clone https://github.com/dpallot/simple-websocket-server

設定に``websocket_server`` と ``websocket_port``を指定してください：

.. code-block:: bash

    electrum setconfig websocket_server example.com

    electrum setconfig websocket_port 9999


デーモンを再起動します：

.. code-block:: bash

    electrum daemon stop

    electrum daemon start


これでページは完全に対話的になり、支払いを受け取るとページが更新されます。

クライアントのファイアウォールで上位のポートがブロックされる可能性があることに注意してください。
そのため、たとえば追加の外部IPアドレスで標準の ``443`` ポートを使用してプロキシWebソケットの送信を逆にする方が安全です。
ローカルにインストールされているWebsocketサーバまたはサーバのポートElectrumサーバが、
カスタマに通知するポートと異なる場合は、次の2つの追加設定パラメータを設定できます。

* websocket_server_announce
* websocket_port_announce

これでこのページは完全にインタラクティブになり、支払いを受け取ると自らアップデートするようになりました。higher portは一部のクライアントのファイアウォールにブロックされる可能性があるので、たとえば追加のサブドメイン上の標準ポートである ``443`` を使用してWebソケットの転送をリバースプロキシで行う方が安全です。

JSONRPCインターフェイス
---------------------

ElectrumデーモンへのコマンドはJSONRPCを使用して送ることができます。PHPスクリプトでElecrumを使用したいときに役に立つでしょう。

デーモンはデフォルトではランダムなポート番号を使うことに気を付けてください。確実なポート番号を使うには、'rpcport'設定値を指定（してデーモンを再起動）する必要があります。：

.. code-block:: bash

   electrum setconfig rpcport 7777


さらにElectrum 3.0.5以降、JSON-RPCインターフェースはHTTP Basic認証を使用して認証されます。

.. _`HTTP basic auth`: https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Basic_authentication_scheme


ユーザー名とパスワードは設定変数です。最初に起動するとき、Electrumは両方を初期化し、パスワードはランダムな文字列に設定されます。後からそれらを変更することもできます（ポートと同じ方法、完了したらデーモンを再起動してください）。それらの値を簡単に見るには、

.. code-block:: bash

   electrum getconfig rpcuser
   electrum getconfig rpcpassword


HTTP Basic認証はリクエストの一部として、暗号化されていないユーザー名とパスワードを送信することに注意してください。我々の見解としてlocalhost上での使用は結構ですが、信頼できないLANやインターネット上での使用は安全ではありません。そのためセキュアなトンネルで接続をラップするなど、そういった場合にはさらなる対策を講じる必要があります。詳細については、こちらをお読みください。

.. _`read this`: https://bitcoin.org/en/release/v0.12.0#rpc-ssl-support-dropped


静的ポートを設定し、認証を設定したら、curlまたはPHPを使用してクエリを実行できます。例：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"getbalance","params":[]}' http://username:password@127.0.0.1:7777

名前付きパラメータを使用したクエリ：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"listaddresses","params":{"funded":true}}' http://username:password@127.0.0.1:7777

支払いリクエストを作成する：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"addrequest","params":{"amount":"3.14","memo":"test"}}' http://username:password@127.0.0.1:7777
