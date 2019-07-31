Electrumのプロトコル仕様
======================

(NOTE: このドキュメントは古くなっています。プロトコルの0.10より新しいバージョンの場合はこちらを見てください https://electrumx.readthedocs.io/en/latest/protocol.html )

StratumはMonacoinクライアントであるElectrumとマイナーに主に使用されている一般的なMonacoin通信プロトコルです。
フォーマット
-----------


StratumプロトコルはJSON-RPC 2.0に基づいています。（ただし、メッセージに「jsonrpc」情報は含まれていません）各メッセージは、終端文字（\n）で終わらなければなりません。

.. _JSON-RPC 2.0: http://www.jsonrpc.org/specification

リクエスト
`````````

Typical request looks like this:

典型的なリクエストは次のようになります。

.. code-block:: json

   { "id": 0, "method":"some.stratum.method", "params": [] }


- idは0から始まり、すべてのメッセージは固有のID番号を持つ
- 可能なメソッドのリストと説明は以下のとおり
- paramsは配列。例：["1myfirstaddress"、 "1mysecondaddress"、 "1andonemoreaddress"]

応答
```
応答は似ています：

.. code-block:: json

   { "id": 0, "result": "616be06545e5dd7daec52338858b6674d29ee6234ff1d50120f060f79630543c"}

- idはリクエストメッセージからコピーされます（このようにしてクライアントは各応答とリクエストをペアにすることができます）
- result can be:

       
 - 結果は次のようになります
 
   - null
   - 文字列（上記のように）
   - ハッシュ　例：
   
    .. code-block:: json

       { "nonce": 1122273605, "timestamp": 1407651121, "version": 2, "bits": 406305378 }   
       
   - ハッシュの配列　例：
   
    .. code-block:: json

       [ { "tx_hash:
       "b87bc42725143f37558a0b41a664786d9e991ba89d43a53844ed7b3752545d4f",
       "height": 314847 }, { "tx_hash":
       "616be06545e5dd7daec52338858b6674d29ee6234ff1d50120f060f79630543c",
       "height": 314853 } ]   

メソッド
-------

server.version
``````````````


これは通常、クライアントの最初のメッセージであるきーっぷアライブメッセージとして毎分送信されます。クライアントは自身のバージョンとサポートするプロトコルのバージョンを送信します。サーバはサポートするバージョンのプロトコルで応答します（通常サーバ側の数値が高いほど互換性があります）。


このドキュメントで説明されているプロトコルのバージョンは0.10です。

*request:*

.. code-block:: json

   { "id": 0, "method": "server.version", "params": [ "1.9.5", "0.6" ] }

*response:*

.. code-block:: json

   { "id": 0, "result": "0.8" }

server.banner
`````````````
*request:*

.. code-block:: json

   { "id": 1, "method": "server.banner", "params": [] }

server.donation_address
```````````````````````

server.peers.subscribe
``````````````````````

クライアントは、このやり方で他のアクティブなサーバのリストを要求することができます。サーバはIRCチャネル（#electrum at freenode.net）に接続されており、互いに見ることができます。各サーバはそのバージョン、各アドレスの履歴プルーンリミット（"p100", "p10000" 等。数字はそれぞれのアドレスのためにいくつのトランザクションをサーバが保存しておくかを意味している）、サポートプロトコル（"t" = tcp@50001, "h" = http@8081,
"s" = tcp/tls@50002, "g" = https@8082; 非標準のポートはこのように告知される: "t3300" for tcp on port 3300）を告知します。

**注意** 執筆時点ではこのメソッドの真のサブスクリプション実装は存在せず、サーバは応答を一度だけ送信します。サーバは今のところ通知の送信をしません。

*request:*

.. code-block:: json

   { "id": 3, "method":
   "server.peers.subscribe", "params": [] }<br/>

*response:*

.. code-block:: json

   { "id": 3, "result": [ [ "83.212.111.114",
   "electrum.stepkrav.pw", [ "v0.9", "p100", "t", "h", "s",
   "g" ] ], [ "23.94.27.149", "ultra-feather.net", [ "v0.9",
   "p10000", "t", "h", "s", "g" ] ], [ "88.198.241.196",
   "electrum.be", [ "v0.9", "p10000", "t", "h", "s", "g" ] ] ]
   }

blockchain.numblocks.subscribe
``````````````````````````````

新たなブロックの高さに関する通知をクライアントに送信するリクエスト。現在のブロック高を返答します。

*request:*

.. code-block:: json

   { "id": 5, "method":
   "blockchain.numblocks.subscribe", "params": [] }


*response:*

.. code-block:: json

   { "id": 5, "result": 316024 }

*message:*

.. code-block:: json

   { "id": null, "method":
   "blockchain.numblocks.subscribe", "params": 316024 }

blockchain.headers.subscribe
````````````````````````````

解析したブロックヘッダの形式で新たなブロックについての通知をクライアントに送信するリクエスト。

*request:*

.. code-block:: json

   { "id": 5, "method":
   "blockchain.headers.subscribe", "params": [] }

*response:*

.. code-block:: json

   { "id": 5, "result": { "nonce":
   3355909169, "prev_block_hash":
   "00000000000000002b3ef284c2c754ab6e6abc40a0e31a974f966d8a2b4d5206",
   "timestamp": 1408252887, "merkle_root":
   "6d979a3d8d0f8757ed96adcd4781b9707cc192824e398679833abcb2afdf8d73",
   "block_height": 316023, "utxo_root":
   "4220a1a3ed99d2621c397c742e81c95be054c81078d7eeb34736e2cdd7506a03",
   "version": 2, "bits": 406305378 } }

*message:*

.. code-block:: json

   { "id": null, "method":
   "blockchain.headers.subscribe", "params": [ { "nonce":
   881881510, "prev_block_hash":
   "00000000000000001ba892b1717690900ae476857120a78fb50825f8b67a42d4",
   "timestamp": 1408255430, "merkle_root":
   "8e92bdbf1c5c581b5942fc290c6c52c586f091b279ea79d4e21460e138023839",
   "block_height": 316024, "utxo_root":
   "060f780c0dd07c4289aaaa2ef24723f73380095b31d60795e1308170ec742ffb",
   "version": 2, "bits": 406305378 } ] }

blockchain.address.subscribe
````````````````````````````

与えられたアドレスのステータス（例えばトランザクション履歴）が変化したときに通知をクライアントに送信するリクエスト。ステータスとはトランザクション履歴のハッシュのことです。アドレスにトランザクションがまだない場合、ステータスはnullです。

*request:*

.. code-block:: json

   { "id": 6, "method":"blockchain.address.subscribe", "params": ["1NS17iag9jJgTHD1VXjvLCEnZuQ3rJDE9L"] }

*response:*

.. code-block:: json

   { "id": 6, "result":"b87bc42725143f37558a0b41a664786d9e991ba89d43a53844ed7b3752545d4f" }

*message:*

.. code-block:: json

   { "id": null, "method":"blockchain.address.subscribe", "params": ["1NS17iag9jJgTHD1VXjvLCEnZuQ3rJDE9L","690ce08a148447f482eb3a74d714f30a6d4fe06a918a0893d823fd4aca4df580"]}

blockchain.address.get_history
``````````````````````````````

指定されたアドレスに対して、トランザクションのリストとその高さ（および新しいバージョンの手数料）が返されます。

*request:*

.. code-block:: json

   {"id": 1, "method": "blockchain.address.get_history", "params": ["1NS17iag9jJgTHD1VXjvLCEnZuQ3rJDE9L"] }

*response:*

.. code-block:: json

   {"id": 1, "result": [{"tx_hash": "ac9cd2f02ac3423b022e86708b66aa456a7c863b9730f7ce5bc24066031fdced", "height": 340235}, {"tx_hash": "c4a86b1324f0a1217c80829e9209900bc1862beb23e618f1be4404145baa5ef3", "height": 340237}]}
   {"jsonrpc": "2.0", "id": 1, "result": [{"tx_hash": "16c2976eccd2b6fc937d24a3a9f3477b88a18b2c0cdbe58c40ee774b5291a0fe", "height": 0, "fee": 225}]}


blockchain.address.get_mempool
``````````````````````````````

blockchain.address.get_balance
``````````````````````````````

*request:*

.. code-block:: json

   { "id": 1, "method":"blockchain.address.get_balance", "params":["1NS17iag9jJgTHD1VXjvLCEnZuQ3rJDE9L"] }

*response:*

.. code-block:: json

   {"id": 1, "result": {"confirmed": 533506535, "unconfirmed": 27060000}}


blockchain.address.get_proof
````````````````````````````

blockchain.address.listunspent
``````````````````````````````

*request:*

.. code-block:: json

   { "id": 1, "method":
   "blockchain.address.listunspent", "params":
   ["1NS17iag9jJgTHD1VXjvLCEnZuQ3rJDE9L"] }<br/>

*response:*

.. code-block:: json

   {"id": 1, "result": [{"tx_hash":
   "561534ec392fa8eebf5779b233232f7f7df5fd5179c3c640d84378ee6274686b",
   "tx_pos": 0, "value": 24990000, "height": 340242},
   {"tx_hash":"620238ab90af02713f3aef314f68c1d695bbc2e9652b38c31c025d58ec3ba968",
   "tx_pos": 1, "value": 19890000, "height": 340242}]}

blockchain.utxo.get_address
```````````````````````````

blockchain.block.get_header
```````````````````````````

blockchain.block.get_chunk
``````````````````````````

blockchain.transaction.broadcast
````````````````````````````````

生のトランザクション（シリアル化、16進数エンコード済）をネットワークに送信します。トランザクションidを返すか、トランザクションが何かしらの理由で無効な場合はエラーを返します。

*request:*

.. code-block:: json

   { "id": 1, "method":
   "blockchain.transaction.broadcast", "params":
   "0100000002f327e86da3e66bd20e1129b1fb36d07056f0b9a117199e759396526b8f3a20780000000000fffffffff0ede03d75050f20801d50358829ae02c058e8677d2cc74df51f738285013c260000000000ffffffff02f028d6dc010000001976a914ffb035781c3c69e076d48b60c3d38592e7ce06a788ac00ca9a3b000000001976a914fa5139067622fd7e1e722a05c17c2bb7d5fd6df088ac00000000" }<br/>

*response:*

.. code-block:: json

   {"id": 1, "result": "561534ec392fa8eebf5779b233232f7f7df5fd5179c3c640d84378ee6274686b"}

blockchain.transaction.get_merkle
`````````````````````````````````

  blockchain.transaction.get_merkle [$txid, $txHeight]

blockchain.transaction.get
``````````````````````````

与えられたtxidの生のトランザクション（16進数エンコード済）を入手するためのメソッド。トランザクションが存在しない場合、エラーが返されます。

*request:*

.. code-block:: json

   { "id": 17, "method":"blockchain.transaction.get", "params": [
   "0e3e2357e806b6cdb1f70b54c3a3a17b6714ee1f0e68bebb44a74b1efd512098"
   ] }

*response:*

.. code-block:: json

   { "id": 17, "result":"01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0704ffff001d0104ffffffff0100f2052a0100000043410496b538e853519c726a2c91e61ec11600ae1390813a627c66fb8be7947be63c52da7589379515d4e0a604f8141781e62294721166bf621e73a82cbf2342c858eeac00000000"}

*error:*

.. code-block:: json

   { "id": 17, "error": "{ u'message': u'No information available about transaction', u'code': -5 }" }


blockchain.estimatefee
``````````````````````



特定のブロック数以内にトランザクションが取り込まれるために必要なkbyteあたりのトランザクション手数料を推定します。推定するのに十分な情報をノードが有していない場合、値-1が返されます。

Parameter:トランザクションが取り込まれるまでに待つであろうブロック数

*request:*

.. code-block:: json

   { "id": 17, "method": "blockchain.estimatefee", "params": [ 6 ] }

*response:*

.. code-block:: json

   { "id": 17, "result": 0.00026809 }
   { "id": 17, "result": 1.169e-05 }

*error:*

.. code-block:: json

   { "id": 17, "result": -1 }


外部リンク
---------

- https://docs.google.com/a/palatinus.cz/document/d/17zHy1SUlhgtCMbypO8cHgpWH73V5iUQKk_0rWvMqSNs/edit?hl=en_US" SlushのStratumプロトコルのオリジナル詳細
- http://mining.bitcoin.cz/stratum-mining 

Stratumマイニング拡張の詳細
