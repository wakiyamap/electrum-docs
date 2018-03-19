Multisig Wallets
================
マルチシグウォレット
==================

This tutorial shows how to create a 2 of 2 multisig wallet. A 2 of 2
multisig consists of 2 separate wallets (usually on separate machines
and potentially controlled by separate people) that have to be used in
conjunction in order to access the funds. Both wallets have the same
set of Addresses.

このチュートリアルでは2of2マルチシグウォレットを作成する方法をお見せします。2of2マルチシグは2つの別々のウォレット（通常は別々のマシン上にありもしかすると別々の人物に管理されているかもしれない）で構成されており、資金にアクセスするためにはこれらを共に使用する必要があります。両方のウォレットに同じアドレスのセットがあります。

- A common use-case for this is if you want to collaboratively control
  funds: maybe you and your friend run a company together
  and certain funds should only be spendable if you both
  agree.

- よくある利用法として資金を共同で管理したい場合が挙げられます：もしかしたらあなたとあなたの友人が一緒に会社を経営していて、一定の資金は両方が同意しなければ使用することができないかもしれません。

- Another one is security: One of the wallets can be on
  your main machine, while the other one is on a offline
  machine. That way you make it very hard for an attacker
  or malware to steal your coins.
  
- もう1つはセキュリティです。ウォレットの1つはメインマシンに、もう1つはオフラインマシンに置くことができます。そうすれば攻撃者やマルウェアがあなたのコインを盗むのは非常に難しくなります。


Create a pair of 2-of-2 wallets
-------------------------------
2-of-2ウォレットのペアを作成する
-----------------------------

Each cosigner needs to do this: In the menu select File->New, then
select "Multi-signature wallet". On the next screen, select 2 of 2.

各共同署名者(cosigner)はこれを行う必要があります。メニューで「ファイル(File)」 - >「新規(New)」を選択し、「Multi-signature wallet」を選択します。次の画面で、2 of 2を選択します。

.. image:: png/create_multisig.png

After generating a seed (keep it safely!) you will need to
provide the master public key of the other wallet.

シードを生成したら（安全に保管してください）、もう一つのウォレットのマスター公開鍵を提供する必要があります。

.. image:: png/create_2of2.png

Put the master public key of the other wallet into the
lower box. Of course when you create the other wallet, you
put the master public key of this one.

もう一つのウォレットのマスター公開鍵を下のボックスに入れます。もちろんもう一つのウォレットを作成するときには、このウォレットのマスター公開鍵を入れます。

You will need to do this in parallel for the two wallets.
Note that you can press cancel during this step, and reopen
the file later.

2つのウォレットに対してこれを並行して実行する必要があります。このステップの間にキャンセルを押せば、ファイルを後から再度開くことができるということを留意しておいてください。

Receiving
---------
受信
----

Check that both wallets generate the same set of Addresses. You can
now send to these Addresses (note they start with a "3") with any
wallet that can send to P2SH Addresses.

両方のウォレットが同じアドレスセットを生成していることを確認してください。あなたはP2SHアドレスに送信できるウォレットを使用してこれらのアドレス（「P」で始まることに注意）に送金できるようになりました。


Spending
--------
送金
----

To spend coins from a 2-of-2 wallet, two cosigners need to
sign a transaction collaboratively.

2of2ウォレットからコインを使うには、2人の共同署名者が協力してトランザクションに署名する必要があります。

To accomplish this, create a transaction using one of the
wallets (by filling out the form on the "send" tab)

これを達成するには、ウォレットの1つを使用してトランザクションを作成します（「送信(Send)」タブのフォームを埋めてください）

After signing, a window is shown with the transaction
details.

署名後、トランザクションの詳細がウィンドウに表示されます。

.. image:: png/Partially_Signed.png

The transaction has to be sent to the second wallet.

トランザクションを2番目のウォレットに送る必要があります。

For this you have multiple options:

- you can transfer the file on a usb stick
- you can use QR codes
- you can use a remote server, with the CosignerPool plugin.

これには複数の選択肢があります：

- USBメモリでファイルを転送することができます
- QRコードを使用することができます
- CosignerPoolプラグインを使用してリモートサーバーを使用できます


Transfer a file
```````````````
ファイルを転送する
````````````````

You can save the partially signed transaction to a file (using the
"save" button), transfer that to the machine where the second wallet
is running (via usb stick, for example) and load it there (using Tools
-> Load transaction -> from file)

「保存(save)」ボタンを押して部分的に署名されたトランザクションをファイルに保存し、2番目のウォレットが実行されているマシンに（usbメモリ等を通して）転送したら、ファイルを読み込ませましょう。（ツール(Tools)->取引情報を読み込む(Load transasction)->ファイルから(form file)）

Use QR-Code
```````````
QRコードを使う
`````````````

There's also a button showing a qr-code icon. Clicking
that will display a qr-code containing the transaction that
can be scanned into the second wallet (Tools -> Load
Transaction -> From QR Code)

QRコードアイコンを表示するボタンもあります。これをクリックすると、2番目のウォレットにスキャンできるトランザクションが入ったQRコードが表示されます（「ツール(Tools)」->「取引情報を読み込む(Load transaction)」->「QRコードから(From QR code)」）


Use the Cosigner Pool Plugin
````````````````````````````
Cosiner Poolプラグインを使う
``````````````````````````

For this to work the Plugin "Cosigner Pool" needs to be
enabled (Tools -> Plugins) with both wallets.

この機能のためには、両方のウォレットでプラグイン"Cosigner Pool"を有効にする必要があります（ツール(Tools)->プラグイン(Plugin)）。


Once the plugin is enabled, you will see a button labeled "Send to
cosigner". Clicking it sends the partially signed transaction to a
central server. Note that the transaction is encrypted with your
cosigner's master public key.

プラグインが有効になると「send to cosigner」というラベルの付いたボタンが表示されます。クリックすると部分的に署名されたトランザクションが中央サーバに送信されます。トランザクションはあなたの共同署名者のマスター公開鍵で暗号化されていることに注意してください。

.. image:: png/Sent_to_Cosigner.png
	    
When the cosigner wallet is started, it will get a
notification that a partially signed transaction is
available:

共同署名者のウォレットが起動すると、部分的に署名されたトランザクションが使用可能であるという通知が表示されます。

.. image:: png/Cosigner_Retrieve.png
	    
The transaction is encrypted with the cosigner's master
public key; the password is needed to decrypt it.

トランザクションは、共同署名者のマスター公開鍵で暗号化されているので復号するためにパスワードが必要です。

With all of the above methods, you can now add the seconds
signature the the transaction (using the "sign" button). It
will then be broadcast to the network.

上記の順序を全て経ると、「署名」ボタンを押すことで2つめの署名をトランザクションに追加できるようになりました。その後、トランザクションはネットワークにブロードキャストされます。

