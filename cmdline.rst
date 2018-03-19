

コマンドライン
============


Electrumには強力なコマンドラインがあります。このページではいくつかの基本原則を示します。

インラインヘルプの使用
--------------------



 Electrumのコマンドリストを表示するには、次のように入力します。

.. code-block:: bash

   electrum help


コマンドのドキュメントを表示するには、次のように入力します。

.. code-block:: bash

   electrum help <command>


マジックワード
------------

コマンドに渡す引数には、次のマジックワードを使うことがあります：! ? - 

  
- エクスクラメーションマーク"!"は利用可能な最大の金額を意味するショートカットです。
  
  例：

  .. code-block:: bash

     electrum payto 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE !

  トランザクション手数料は計算され、総計から差し引かれることに気を付けてください。

- クエスチョンマーク"?"はそのパラメータを入力してもらいたいことを表すためのものです。

 
  例：

  .. code-block:: bash

     electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE ?

  
- コロン":"は入力したパラメータを隠したいとき（ターミナルに表示したくないとき）に使います。

  .. code-block:: bash

     electrum importprivkey :

　この例では、秘密鍵とウォレット・パスワードの2つのプロンプトが表示されます。

- ダッシュ"-"で置き換えられたパラメータは、標準入力（パイプ内）から読み込まれます。

  .. code-block:: bash

     cat LICENCE | electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -

エイリアス
---------


ほとんどのコマンドにおいて、Monacoinアドレスの部分にDNSエイリアスを使うことできます。

.. code-block:: bash

   electrum payto ecdsa.net !


jqを使って出力を成形する。
----------------------

コマンド出力はシンプルな文字列かjson形式のデータのどちらかになっています。jqプログラムというとても便利なユーティリティががあります。次のようにしてインストールできます。

.. code-block:: bash

   sudo apt-get install jq

以下の例ではjpプログラムを用いています。

例
--

メッセージの署名と検証
````````````````````



変数を使用して署名を格納し、検証することができます。


.. code-block:: bash

   sig=$(cat LICENCE| electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -)
          
そして：

.. code-block:: bash

   cat LICENCE | electrum verifymessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE $sig -


未使用値を表示する。
`````````````````


'listunspent'コマンドは様々なフィールドを持つ辞書オブジェクトのリストを返します。各レコードの 'value'フィールドを抽出したいとすると、これはjpコマンドを用いることで可能です。

.. code-block:: bash

   electrum listunspent | jq 'map(.value)'
          
履歴から受信トランザクションのみを選択する。
```````````````````````````````````````

受信トランザクションは正の'value'フィールドを持ちます。

.. code-block:: bash

   electrum history | jq '.[] | select(.value>0)'

日付によってトランザクションをフィルタリングする
````````````````````````````````````````````

次のコマンドは、指定された日付の後にタイムスタンプされたトランザクションを選択します。

.. code-block:: bash

   after=$(date -d '07/01/2015' +"%s")

   electrum history | jq --arg after $after '.[] | select(.timestamp>($after|tonumber))'
          

同様に、一定期間のトランザクションをエクスポートすることができます。

.. code-block:: bash

   before=$(date -d '08/01/2015' +"%s")

   after=$(date -d '07/01/2015' +"%s")

   electrum history | jq --arg before $before --arg after $after '.[] | select(.timestamp&gt;($after|tonumber) and .timestamp&lt;($before|tonumber))'
          

メッセージの暗号化と復号化
````````````````````````


最初にウォレットアドレスの公開鍵が必要です。

.. code-block:: bash

   pk=$(electrum getpubkeys 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE| jq -r '.[0]')
          

暗号化：

.. code-block:: bash

   cat | electrum encrypt $pk -

復号化：

.. code-block:: bash

   electrum decrypt $pk ?       


注：このコマンドは暗号化済みメッセージを要求し、その際にウォレットのパスワードを要求します。

秘密鍵のエクスポートとコインのスイープ
```````````````````````````````````


次のコマンドでウォレットのMonacoinを持つすべてのアドレスをエクスポートします。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys -


これによって秘密鍵のリストが返されます。多くの場合ではシンプルなリストが欲しいでしょう。これはjqフィルターを追加することで可能です。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])'
          

最後に、この秘密鍵のリストを使用してsweepコマンドに入力してみましょう。

.. code-block:: bash

   electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])' | electrum sweep - [destination address]
