コマンドラインからコールドストレージを使用する
=========================================

このページではコマンドラインを使って、オフラインのElectrumウォレットでトランザクションに署名する方法を説明します。

未署名のトランザクションを作成する
-------------------------------

オンライン（閲覧専用）ウォレットを使って、未署名のトランザクションを作成します。

.. code-block:: bash

   electrum payto 1Cpf9zb5Rm5Z5qmmGezn6ERxFWvwuZ6UCx 0.1 --unsigned > unsigned.txn


未署名のトランザクションは、 'unsigned.txn'という名前のファイルに格納されます。閲覧専用ウォレットを使用する場合は、--unsignedオプションは不要です。

以下のコマンドで中身を見ることができます。

.. code-block:: bash

   cat unsigned.txn | electrum deserialize -

トランザクションに署名する
-----------------------


Electrumのシリアライズ形式には、オフラインウォレットでトランザクションに署名する際に使用される、必要なマスター公開鍵と鍵導出(Key derivation)が含まれています。

したがって、シリアライズされたトランザクションをオフラインウォレットに渡すだけで済みます。

.. code-block:: bash

   cat unsigned.txn | electrum signtransaction - > signed.txn

このコマンドではあなたのパスワードが求められ、署名されたトランザクションは'signed.txn'に保存されます。

トランザクションをブロードキャストする
----------------------------------

'broadcast'を使用してMonacoinネットワークにトランザクションを送信します。


.. code-block:: bash

   cat signed.txn | electrum broadcast -

成功すると、コマンドはトランザクションIDを返します。
