.. _coldstorage:

Cold Storage
============
コールドストレージ
================

This document shows how to create an offline wallet that
holds your Bitcoins and a watching-only online wallet that
is used to view its history and to create transactions that
have to be signed with the offline wallet before being
broadcast on the online one.

このドキュメントでは、Monacoinを保持するオフラインウォレットを作成、そしてその履歴を表示するための閲覧専用のオンラインウォレットの作成方法と、オンラインウォレットでブロードキャストする前に、オフラインウォレットでの署名が必要なトランザクションの作成方法を紹介します。


Create an offline wallet
------------------------
オフラインウォレットを作成する
---------------------------

Create a wallet on an offline machine, as per the usual process (file
-> new) etc.

通常の手順(ファイル(File)->新規(New))などのようにして、オフラインのマシンでウォレットを作成します。

After creating the wallet, go to Wallet -> Information.

ウォレットを作成したら、ウォレット(Wallet)->情報(Information)に移動します。

.. image:: png/wallet_info.png

The Master Public Key of your wallet is the string shown in this popup
window.  Transfer that key to your online machine somehow.

ウォレットのマスター公開鍵がポップアップウィンドウに文字列で表示されます。そのキーをあなたのオンラインマシンに何かしらの方法で移動してください。

Create a watching-only version of your wallet
---------------------------------------------
ウォレットの閲覧専用バージョンを作成する
------------------------------------

On your online machine, open up Electrum and select File ->
New/Restore. Enter a name for the wallet and select "Standard wallet".

オンラインマシン上でElectrumを開き「ファイル(File)」->「新規/復元(New/Restore)」を選択します。ウォレットの名前を入力し、「standard wallet」を選択してください。

.. image:: png/standard_wallet.png

Select "Use public or private keys"

「Use public or private keys」を選択。

.. image:: png/public_or_private.png

Paste your master public key in the box.

マスター公開鍵をボックスに貼り付けます。

.. image:: png/restore_key.png

Click Next to complete the creation of your wallet. 
When you're done, you should see a popup informing you that you are opening a watching-only wallet.

「次へ(Next)」をクリックしてウォレットの作成を完了します。作業が終わると、現在閲覧専用ウォレットを開いていることを通知するポップアップが表示されます。

.. image:: png/watchingonly.png

Then you should see the transaction history of your cold wallet.

その後、コールドウォレットの取引履歴が表示されます。

Create an unsigned transaction
------------------------------
未署名のトランザクションを作成する
-------------------------------

Go to the "send" tab on your online watching-only wallet,
input the transaction data and press "Preview". A window pops up:

オンラインの閲覧専用ウォレットで「送信(Send)」タブを開き、トランザクションデータを入力したら「プレビュー(Preview)」を押してください。ポップウィンドウが開きます：

.. image:: png/unsigned.png


Press "save" and save the transaction file somewhere on your computer. Close the
window and transfer the transaction file to your offline
machine (e.g. with a usb stick).

「保存(save)」を押してコンピュータにトランザクションファイルを保存してください。ウィンドウを閉じてトランザクションファイルをあなたのオフラインマシンに転送してください。（USBメモリなどを使って）

Get your transaction signed
---------------------------
トランザクションに署名する
-----------------------

On your offline wallet, select Tools -> Load transaction -> From file
in the menu and select the transaction file created in the previous
step.

オフラインウォレットで、メニューから「ツール(Tools)」->「取引情報の読み込み(Load transaction)」->「ファイルから(From file)」を選択し、先ほどのステップで作成したトランザクションファイルを選択します。

.. image:: png/sign.png

Press "sign". Once the transaction is signed, the Transaction ID
appears in its designated field.

「署名(sign)」を押してください。トランザクションが署名されると、トランザクションIDが所定のフィールドに表示されます。

.. image:: png/signed.png

Press save, store the file somewhere on your
computer, and transfer it back to your online machine.

「保存(save)」を押して、コンピュータにファイルを保存したら、オンラインマシンにそのファイルを転送します。

Broadcast your transaction
--------------------------
トランザクションをブロードキャストする
----------------------------------


On your online machine, select Tools -> Load transaction -> From File
from the menu. Select the signed transaction file. In the window that
opens up, press "broadcast". The transaction will be broadcasted over
the Bitcoin network.

オンラインマシンでメニューから「ツール(Tools)」->「取引情報の読み込み(Load transaction)」->「ファイルから(From file)」を選択します。署名済みトランザクションのファイルを選択します。開いたウィンドウで「発信(broadcast)」を押します。トランザクションはMonacoinネットワークを通してブロードキャストされます。

