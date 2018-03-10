Using cold storage with the command line
========================================
コマンドラインからコールドストレージを使用する
=========================================

This page will show you how to sign a transaction with
an offline Electrum wallet, using the Command line.

このページではコマンドラインを使って、オフラインのElectrumウォレットでトランザクションに署名する方法を説明します。

Create an unsigned transaction
------------------------------
未署名のトランザクションを作成する
-------------------------------

With your online (watching-only) wallet, create an
unsigned transaction:

オンライン（閲覧専用）ウォレットを使って、未署名のトランザクションを作成します。

.. code-block:: bash

   electrum payto 1Cpf9zb5Rm5Z5qmmGezn6ERxFWvwuZ6UCx 0.1 --unsigned > unsigned.txn

The unsigned transaction is stored in a file named 'unsigned.txn'.
Note that the --unsigned option is not needed if you use a
watching-only wallet.

未署名のトランザクションは、 'unsigned.txn'という名前のファイルに格納されます。閲覧専用ウォレットを使用する場合は、--unsignedオプションは不要です。


You may view it using:

以下のコマンドで中身を見ることができます。

.. code-block:: bash

   cat unsigned.txn | electrum deserialize -

Sign the transaction
--------------------
トランザクションに署名する
-----------------------

The serialization format of Electrum contains the master
public key needed and key derivation, used by the offline
wallet to sign the transaction.

Electrumのシリアライズ形式には、オフラインウォレットでトランザクションに署名する際に使用される、必要なマスター公開鍵と鍵導出(Key derivation)が含まれています。

Thus we only need to pass the serialized transaction to
the offline wallet:

したがって、シリアライズされたトランザクションをオフラインウォレットに渡すだけで済みます。

.. code-block:: bash

   cat unsigned.txn | electrum signtransaction - > signed.txn

The command will ask for your password, and save the
signed transaction in 'signed.txn'

このコマンドではあなたのパスワードが求められ、署名されたトランザクションは'signed.txn'に保存されます。

Broadcast the transaction
-------------------------
トランザクションをブロードキャストする
----------------------------------

Send your transaction to the Bitcoin network, using broadcast:

'roadcast'を使用してMonacoinネットワークにトランザクションを送信します。

.. code-block:: bash

   cat signed.txn | electrum broadcast -

If successful, the command will return the ID of the
transaction.

成功すると、コマンドはトランザクションIDを返します。
