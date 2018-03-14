How to accept Bitcoin on a website using Electrum
=================================================
Electrumを使ったWebサイト上でのMonacoinの受け取り方
================================================

This tutorial will show you how to accept Bitcoin on a website with
SSL signed payment requests. It is updated for Electrum 2.6.

このチュートリアルではSSL化された支払いリクエストを使ってWebサイト上でMonacoinを受け取る方法を説明しています。Electrum2.6.で更新されています。

Requirements
------------
要件
----

- A webserver serving static HTML
- A SSL certificate (signed by a CA)
- Electrum version >= 2.6

- static HTMLを提供するWebサーバ
- SSL証明書（CA(認証局)による署名）
- バージョン2.6以上のElectrum


Create and use your merchant wallet
商業用のウォレットを作成、使用する
-----------------------------


Create a wallet on your protected machine, as you want to keep your
cryptocurrency safe. If anybody compromise your merchant server, s/he will be able
to access read-only version of your wallet only and won't be able to spent currency.

暗号通貨を安全に保存しておきたい場合は、保護されたマシン上にウォレットを作成してください。もしあなたの商業用サーバに誰かが侵入した場合、彼又は彼女は読み取り専用のウォレットにアクセスすることができるだけでコインを使用することはできません。

Please notice that the potential intruder still will be able to see your
addresses, transactions and balance, though. It's also recommended to use a
separate wallet for your merchant purposes (and not your main wallet).

しかし内部に隠れた侵入者は依然としてあなたのアドレス、取引、残高を監視することができます。商業目的には（あなたのメインウォレットではなく）分けたウォレットを使用することが推奨されています。

.. code-block:: bash

   electrum create

Still being on a protected machine, export your Master Public Key (xpub):

保護されたマシン上でマスター公開鍵(xpub)をエクスポートする：

.. code-block:: bash

   electrum getmpk -w .electrum/wallets/your-wallet

Now you are able to set up your electrum merchant daemon.

これで商業用Electrumデーモンを設定することができるようになりました。

On the server machine restore your wallet from previously exported Master
Public Key (xpub):

サーバマシン上で先ほどエクスポートしたマスター公開鍵(xpub)を復旧します。

.. code-block:: bash

   electrum restore xpub...............................................

Once your read-only wallet is (re-)created, start Electrum as a daemon:

あなたの読み取り専用のウォレットを（再）復旧したらElectrumをデーモンとして起動します：

.. code-block:: bash

   electrum daemon start
   electrum daemon load_wallet

Add your SSL certificate to your configuration
----------------------------------------------
設定にSSL証明書を追加する
-----------------------

You should have a private key and a public certificate for
your domain.

Create a file that contains only the private key:

あなたのドメイン用の秘密鍵と公開証明書を持っている必要があります。

秘密鍵だけを含んだファイルを作成してください。

.. code-block:: none

   -----BEGIN PRIVATE KEY-----
   your private key
   -----END PRIVATE KEY-----


Set the path to your the private key file with setconfig:

setconfigコマンドで秘密鍵のファイルへのパスを指定してください。

.. code-block:: bash

   electrum setconfig ssl_privkey /path/to/ssl.key

Create another file, file that contains your certificate,
and the list of certificates it depends on, up to the root
CA. Your certificate must be at the top of the list, and
the root CA at the end.

別のファイルを作成し、あなたの証明書とルートの認証局まで依存する証明書のリストを含めてください。あなたの証明書はリストの最初、ルートの認証局はリスト最後にある必要があります。

.. code-block:: none

   -----BEGIN CERTIFICATE-----
   your cert
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   intermediate cert
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   root cert
   -----END CERTIFICATE-----


Set the ssl_chain path with setconfig:

setconfigコマンドでssl_chainまでのパスを指定してください。

.. code-block:: bash

   electrum setconfig ssl_chain /path/to/ssl.chain


Configure a requests directory
------------------------------
レクエストディレクトリを設定する
-----------------------------

This directory must be served by your webserver (eg Apache)

このディレクトリはあなたのWebサーバに設置されなければなりません。（Apacheなど）

.. code-block:: bash

   electrum setconfig requests_dir /var/www/r/

By default, electrum will display local URLs, starting with 'file://'
In order to display public URLs, we need to set another configuration
variable, url_rewrite. For example:

デフォルトではElectrumは'file://'で始まるローカルのURLを表示します。公開されたURLを表示するためには、url_rewriteを設定する必要があります。例えば、

.. code-block:: bash

   electrum setconfig url_rewrite "['file:///var/www/','https://electrum.org/']"

Create a signed payment request
-------------------------------
署名された支払いリクエストを作成する
---------------------------------

.. code-block:: bash

   electrum addrequest 3.14 -m "this is a test"
   {
      "URI": "bitcoin:1MP49h5fbfLXiFpomsXeqJHGHUfNf3mCo4?amount=3.14&r=https://electrum.org/r/7c2888541a", 
      "address": "1MP49h5fbfLXiFpomsXeqJHGHUfNf3mCo4", 
      "amount": 314000000, 
      "amount (BTC)": "3.14", 
      "exp": 3600, 
      "id": "7c2888541a", 
      "index_url": "https://electrum.org/r/index.html?id=7c2888541a", 
      "memo": "this is a test", 
      "request_url": "https://electrum.org/r/7c2888541a", 
      "status": "Pending", 
      "time": 1450175741
   }

This command returns a json object with two URLs:

このコマンドは二つのURLとともにjsonオブジェクトを返します：

 - request_url is the URL of the signed BIP70 request.
 - index_url is the URL of a webpage displaying the request.

 - request_urlは署名されたBIP70リクエストのURL
 - index_urlはリクエストを表示するWebページのURL
 
Note that request_url and index_url use the domain name we defined in
url_rewrite.

You can view the current list of requests using the 'listrequests'
command.

request_urlとindex_urlはurl_rewriteに定義したドメイン名を使用することに気を付けてください。

'listrequests'コマンドを使用することで現在のリクエストの一覧を閲覧することができます。


Open the payment request page in your browser
---------------------------------------------
ブラウザで支払いリクエストのページを開く
------------------------------------

Let us open index_url in a web browser.

index_urlをWebブラウザで開いてみましょう。

.. image:: png/payrequest.png


The page shows the payment request. You can open the
bitcoin: URI with a wallet, or scan the QR code. The bottom
line displays the time remaining until the request expires.

このページには支払いリクエストが載っています。ウォレットでMonaocin: URI、またはQRコードをスキャン出来ます。最後の行

.. image:: png/payreq_window.png
          

This page can already used to receive payments. However,
it will not detect that a request has been paid; for that
we need to configure websockets

既にこのページは支払いを受け取るために使用できます。ただしリクエストが支払われたかどうかは検出しません。そのためにはWebソケットを設定する必要があります。

Add web sockets support
-----------------------
Webソケットのサポートを追加する
----------------------------

Get SimpleWebSocketServer from here:

SimpleWebSocketServerをここから入手してください：

.. code-block:: bash

   git clone https://github.com/ecdsa/simple-websocket-server.git


Set ``websocket_server`` and ``websocket_port`` in your config:

設定に``websocket_server`` と ``websocket_port``を指定してください：

.. code-block:: bash

    electrum setconfig websocket_server <FQDN of your server>

    electrum setconfig websocket_port 9999


And restart the daemon:

デーモンを再起動します：

.. code-block:: bash

   electrum daemon stop

   electrum daemon start
   
Now, the page is fully interactive: it will update itself
when the payment is received. Please notice that higher ports might 
be blocked on some client's firewalls, so it is more safe for 
example to reverse proxy websockets transmission using standard 
``443`` port on an additional subdomain.

これでこのページは完全にインタラクティブになり、支払いを受け取ると自らアップデートするようになりました。higher portは一部のクライアントのファイアウォールにブロックされる可能性があるので、たとえば追加のサブドメイン上の標準ポートである ``443`` を使用してWebソケットの転送をリバースプロキシで行う方が安全です。

JSONRPC interface
-----------------
JSONRPCインターフェイス
---------------------

Commands to the Electrum daemon can be sent using JSONRPC. This is
useful if you want to use electrum in a PHP script.

ElectrumデーモンへのコマンドはJSONRPCを使用して送ることができます。PHPスクリプトでElecrumを使用したいときに役に立つでしょう。

Note that the daemon uses a random port number by default. In order to
use a stable port number, you need to set the 'rpcport' configuration
variable (and to restart the daemon):

デーモンはデフォルトではランダムなポート番号を使うことに気を付けてください。確実なポート番号を使うには、'rpcport'設定値を指定（してデーモンを再起動）する必要があります。：

.. code-block:: bash

   electrum setconfig rpcport 7777

Further, starting with Electrum 3.0.5, the JSON-RPC interface is
authenticated using `HTTP basic auth`_.

さらにElectrum 3.0.5以降、JSON-RPCインターフェースはHTTP Basic認証を使用して認証されます。

.. _`HTTP basic auth`: https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Basic_authentication_scheme

The username and the password are config variables.
When first started, Electrum will initialise both;
the password will be set to a random string. You can of course
change them afterwards (the same way as the port, and then restart
the daemon). To simply look up their value:

ユーザー名とパスワードは設定変数です。最初に起動するとき、Electrumは両方を初期化し、パスワードはランダムな文字列に設定されます。後からそれらを変更することもできます（ポートと同じ方法、完了したらデーモンを再起動してください）。それらの値を簡単に見るには、

.. code-block:: bash

   electrum getconfig rpcuser
   electrum getconfig rpcpassword

Note that HTTP basic auth sends the username and the password unencrypted as
part of the request. While using it on localhost is fine in our opinion,
using it across an untrusted LAN or the Internet is not secure.
Hence, you should take further measures in such cases, such as wrapping the
connection in a secure tunnel. For further details, `read this`_.

HTTP Basic認証はリクエストの一部として、暗号化されていないユーザー名とパスワードを送信することに注意してください。我々の見解としてlocalhost上での使用は結構ですが、信頼できないLANやインターネット上での使用は安全ではありません。そのためセキュアなトンネルで接続をラップするなど、そういった場合にはさらなる対策を講じる必要があります。詳細については、こちらをお読みください。

.. _`read this`: https://bitcoin.org/en/release/v0.12.0#rpc-ssl-support-dropped

After setting a static port, and configuring authentication,
we can perform queries using curl or PHP. Example:

静的ポートを設定し、認証を設定したら、curlまたはPHPを使用してクエリを実行できます。例：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"getbalance","params":[]}' http://username:password@127.0.0.1:7777

Query with named parameters:

名前付きパラメータを使用したクエリ：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"listaddresses","params":{"funded":true}}' http://username:password@127.0.0.1:7777

Create a payment request:

支払いリクエストを作成する：

.. code-block:: bash

   curl --data-binary '{"id":"curltext","method":"addrequest","params":{"amount":"3.14","memo":"test"}}' http://username:password@127.0.0.1:7777

