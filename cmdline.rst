

Command Line
============
コマンドライン
============


Electrum has a powerful command line. This page will show you a few basic principles.

Electrumには強力なコマンドラインがあります。このページではいくつかの基本原則を示します。

Using the inline help
---------------------
インラインヘルプの使用
--------------------


To see the list of Electrum commands, type:

 Electrumのコマンドリストを表示するには、次のように入力します。

.. code-block:: bash

   electrum help


To see the documentation for a command, type:

コマンドのドキュメントを表示するには、次のように入力します。

.. code-block:: bash

   electrum help <command>


Magic words
-----------
マジックワード
------------


The arguments passed to commands may be one of the following magic words: ! ? : and -.

コマンドに渡す引数には、次のマジックワードを使うことがあります：! ? - 

- The exclamation mark ! is a shortcut that means 'the maximum amount
  available'.
  
- エクスクラメーションマーク"!"は利用可能な最大の金額を意味するショートカットです。

  Example:
  
  例：

  .. code-block:: bash

     electrum payto 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE !

  Note that the transaction fee will be computed and deducted from the
  amount.

  トランザクション手数料は計算され、総計から差し引かれることに気を付けてください。

- A question mark ? means that you want the parameter to be prompted.

- クエスチョンマーク"?"はそのパラメータを入力してもらいたいことを表すためのものです。

  Example:
  
  例：

  .. code-block:: bash

     electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE ?

- Use a colon : if you want the prompted parameter to be hidden (not
  echoed in your terminal).
  
- コロン":"は入力したパラメータを隠したいとき（ターミナルに表示したくないとき）に使います。

  .. code-block:: bash

     electrum importprivkey :

  Note that you will be prompted twice in this example, first for the
  private key, then for your wallet password.

　この例では、秘密鍵とウォレット・パスワードの2つのプロンプトが表示されます。

- A parameter replaced by a dash - will be read from standard input
  (in a pipe)
  
- ダッシュ"-"で置き換えられたパラメータは、標準入力（パイプ内）から読み込まれます。

  .. code-block:: bash

     cat LICENCE | electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -

Aliases
-------
エイリアス
---------

You can use DNS aliases in place of bitcoin addresses, in most
commands.

ほとんどのコマンドにおいて、bitcoinアドレスの部分にDNSエイリアスを使うことできます。

.. code-block:: bash

   electrum payto ecdsa.net !


Formatting outputs using jq
---------------------------
jqを使って出力を成形する。
-----------------------

Command outputs are either simple strings or json structured data. A
very useful utility is the 'jq' program.  Install it with:

コマンド出力はシンプルな文字列かjson形式のデータのどちらかになっています。jqプログラムというとても便利なユーティリティががあります。次のようにしてインストールできます。

.. code-block:: bash

   sudo apt-get install jq

The following examples use it.

以下の例ではjpプログラムを用いています。

Examples
--------
例
--

Sign and verify message
```````````````````````
メッセージの署名と検証
````````````````````


We may use a variable to store the signature, and verify
it:

変数を使用して署名を格納し、検証することができます。


.. code-block:: bash

   sig=$(cat LICENCE| electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -)
          
And:

そして：

.. code-block:: bash

   cat LICENCE | electrum verifymessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE $sig -


Show the values of your unspents
````````````````````````````````
未使用値を表示する。
`````````````````

The 'listunspent' command returns a list of dict objects,
with various fields. Suppose we want to extract the 'value'
field of each record. This can be achieved with the jq
command:

'listunspent'コマンドは様々なフィールドを持つ辞書オブジェクトのリストを返します。各レコードの 'value'フィールドを抽出したいとすると、これはjpコマンドを用いることで可能です。

.. code-block:: bash

   electrum listunspent | jq 'map(.value)'
          

Select only incoming transactions from history
``````````````````````````````````````````````
履歴から受信トランザクションのみを選択する。
```````````````````````````````````````

Incoming transactions have a positive 'value' field

受信トランザクションは正の'value'フィールドを持ちます。

.. code-block:: bash

   electrum history | jq '.[] | select(.value>0)'

Filter transactions by date
```````````````````````````
日付によってトランザクションをフィルタリングする
````````````````````````````````````````````

The following command selects transactions that were
timestamped after a given date:

次のコマンドは、指定された日付の後にタイムスタンプされたトランザクションを選択します。

.. code-block:: bash

   after=$(date -d '07/01/2015' +"%s")

   electrum history | jq --arg after $after '.[] | select(.timestamp>($after|tonumber))'
          

Similarly, we may export transactions for a given time
period:

同様に、一定期間のトランザクションをエクスポートすることができます。

.. code-block:: bash

   before=$(date -d '08/01/2015' +"%s")

   after=$(date -d '07/01/2015' +"%s")

   electrum history | jq --arg before $before --arg after $after '.[] | select(.timestamp&gt;($after|tonumber) and .timestamp&lt;($before|tonumber))'
          

Encrypt and decrypt messages
````````````````````````````
メッセージの暗号化と復号化
````````````````````````


First we need the public key of a wallet address:

最初にウォレットアドレスの公開鍵が必要です。

.. code-block:: bash

   pk=$(electrum getpubkeys 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE| jq -r '.[0]')
          

Encrypt:

暗号化：

.. code-block:: bash

   cat | electrum encrypt $pk -

Decrypt:
復号化：

.. code-block:: bash

   electrum decrypt $pk ?       

Note: this command will prompt for the encrypted message, then for the
wallet password

注：このコマンドは暗号化済みメッセージを要求し、その際にウォレットのパスワードを要求します。

Export private keys and sweep coins
```````````````````````````````````
秘密鍵のエクスポートとコインのスイープ
```````````````````````````````````

The following command will export the private keys of all wallet
addresses that hold some bitcoins:

次のコマンドでウォレットのbitcoinを持つすべてのアドレスをエクスポートします。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys -

This will return a list of lists of private keys. In most
cases, you want to get a simple list. This can be done by
adding a jq filer, as follows:

これによって秘密鍵のリストが返されます。多くの場合ではシンプルなリストが欲しいでしょう。これはjqフィルターを追加することで可能です。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])'
          
Finally, let us use this list of private keys as input to the sweep
command:

最後に、この秘密鍵のリストを使用してsweepコマンドに入力してみましょう。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])' | electrum sweep - [destination address]
