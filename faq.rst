Frequently Asked Questions
==========================
よくある質問
==========================


How does Electrum work?
-----------------------
Electrumはどのように動作しますか？
-----------------------

Electrum's focus is speed, with low resource usage and
simplifying Bitcoin. Startup times are instant because it
operates in conjunction with high-performance servers that
handle the most complicated parts of the Bitcoin system.

Electrumが焦点にあてているのはスピード、少ない計算資源の使用量、Bitcoinを簡単にすることです。Bitcoinのシステムの最も複雑な部分は高性能なサーバーが操作し、Electrumはこれと連携して動作するので起動時間はわずかです。

Does Electrum trust servers?
----------------------------
Electrumはサーバーを信頼していますか？
---------------------------------

Not really; the Electrum client never sends private keys
to the servers. In addition, it verifies the information
reported by servers, using a technique called :ref:`Simple Payment Verification <spv>`

そうでもないです;Electrumクライアントは秘密鍵をサーバーに送信しません。さらに、サーバーから受け取った情報は
Simple Payment Verification=SPVと呼ばれる技術で検証されます。

What is the seed?
-----------------
シードとは何ですか？
-----------------

The seed is a random phrase that is used to generate your private
keys.

シードは秘密鍵を生成するために使用されるランダムなフレーズです。

Example:

.. code-block:: none

   slim sugar lizard predict state cute awkward asset inform blood civil sugar

Your wallet can be entirely recovered from its seed. For this, select
the "restore wallet" option in the startup.

例：

.. code-block:: none

   slim sugar lizard predict state cute awkward asset inform blood civil sugar
   
あなたのウォレットはシードから完全に復元することができます。そのためには、起動時に「restore wallet」オプションを選択してください。


How secure is the seed?
-----------------------
シードはどれくらい安全ですか？
-----------------------


The seed phrase created by Electrum has 132 bits of entropy. This
means that it provides the same level of security as a Bitcoin private
key (of length 256 bits). Indeed, an elliptic curve key of length n
provides n/2 bits of security.

Electrumによって作成されたシードフレーズは132bitのエントロピーを持ちます。つまり、Bitcoinの秘密鍵（長さ256bit）と同じレベルの
セキュリティを提供します。実際、長さnの楕円曲線キーは、n / 2bitのセキュリティを提供します。

I have forgotten my password. What can I do?
--------------------------------------------
パスワードを忘れてしまいました。何ができるでしょう。
--------------------------------------------

It is not possible to recover your password. However, you can restore
your wallet from its seed phrase and choose a new password.
If you lose both your password and your seed, there is no way
to recover your money. This is why we ask you to save your seed
phrase on paper.

パスワードを復元することはできません。ただし、シードフレーズからウォレットを復元し、新しいパスワードを選ぶことができます。
パスワードとシードの両方がわからなくなった場合、あなたの資金を取り戻す方法はありません。これがシードフレーズを紙に書き留めるようにお願いする理由です。

To restore your wallet from its seed phrase, create a new wallet, select
the type, choose "I already have a seed" and proceed to input your seed
phrase.

シードフレーズからウォレットを復元するには、create a new walletを選んだのち、「I already have a seed」を選択してシードフレーズを入力してください。


My transaction has been unconfirmed for a long time. What can I do?
-------------------------------------------------------------------
私のトランザクションが長い間承認されていません。何ができますか？
----------------------------------------------------------

Bitcoin transactions become "confirmed" when miners accept to write
them in the Bitcoin blockchain. In general, the speed of confirmation
depends on the fee you attach to your transaction; miners prioritize
transactions that pay the highest fees.

Bitcoinトランザクションはマイナーがブロックチェーンに対してその書き込みを許可した時に「承認」されます。一般に承認スピードはあなたがトランザクションに添付した手数料に依存します。マイナーは最も高い手数料を支払うトランザクションを優先します。

Recent versions of Electrum use "dynamic fees" in order to make sure
that the fee you pay with your transaction is adequate. This feature
is enabled by default in recent versions of Electrum.

Electrumの最近のバージョンでは、トランザクションに支払う手数料を十分にするために「ダイナミックフィー」を使用しています。この機能はElectrumの最近のバージョンではあらかじめ有効になっています。

If you have made a transaction that is unconfirmed, you can:

未承認のトランザクションを作成してしまった場合、次の操作を実行できます。：

 - Wait for a long time. Eventually, your transaction will either be
   confirmed or cancelled. This might take several days.
   
 - しばらく待つ。最終的にはあなたのトランザクションは承認されるかキャンセルされます。これには数日かかることがあります。

 - Increase the transaction fee. This is only possible for
   "replaceable" transactions. To create this type of transaction, 
   you must have checked "Replaceable" on the send tab before sending
   the transaction. If you're not seeing the "Replaceable" option on 
   the send tab go to Tools menu > Preferences > Fees tab and set 
   "Propose Replace-By-Fee" to "Always". Transactions that are
   replaceable have the word "Replaceable" in the date column on the
   history tab. To increase the fee of a replaceable transaction right 
   click on its entry on the history tab and choose "Increase Fee". 
   Set an appropriate fee and click on "OK". A window will popup with 
   the unsigned transaction. Click on "Sign" and then "Broadcast".
   
 - トランザクション手数料を増やす。これは「置き換え可能な(replaceable)」トランザクションでのみ可能です。このタイプのトランザクションを作成するには、トランザクションを送信する前に、[送信(send)]タブで[Replaceable]をチェックしておく必要があります。[send]タブの[Replaceable]オプションが表示されない場合は、[ツール(Tool)]メニュー> [設定(Preference)] > [手数料(Fee)]タブに移動し、[Propose Replace-By-Fee]を[Always]に設定します。置き換え可能なトランザクションの場合、historyタブの日付列に「Replaceable」と表示されます。交換可能な取引の手数料を増額するには、[履歴(history)]タブのエントリを右クリックし、「手数料を増やす(Increase Fee)」を選択します。適切な料金を設定し、「OK」をクリックします。未署名のトランザクションがウィンドウにポップアップ表示されます。「署名(Sign)」をクリックして「発信(Broadcast)」をクリックします。

 - Create a "Child Pays for Parent" transaction. A CPFP is a new
   transaction that pays a high fee in order to compensate for the
   small fee of its parent transaction. It can be done by the
   recipient of the funds, or by the sender, if the transaction has a
   change output. To create a CPFP transaction right click on the 
   unconfirmed transaction on the history tab and choose 
   "Child pays for parent". Set an appropriate fee and click on "OK". 
   A window will popup with the unsigned transaction. Click on "Sign"
   and then "Broadcast".
   
 - 「親のための子どもの支払い(Child Pays for Parent)」トランザクションの作成をする。CPFPはその親であるトランザクションのわずかな手数料を補うために高い手数料を支払おうとする新しいトランザクションです。これは資金の受領者によってのみ、またはトランザクションがお釣りアウトプットを場合に送信者が行うことができます。CPFPトランザクションを作成するには、[履歴(history)]タブの未承認のトランザクションを右クリックし[Child pays for parent]を選択します。適切な手数料を設定したら[OK]をクリックします。未署名のトランザクションがウィンドウにポップアップ表示されます。「署名(Sign)」をクリックして「発信(Broadcast)」をクリックします。


What does it mean to "freeze" an address in Electrum?
-----------------------------------------------------
Electrumのアドレスを「フリーズ」するとはどういう意味ですか？
-------------------------------------------------------

When you freeze an address, the funds in that address will not be used
for sending bitcoins. You cannot send bitcoins if you don't have
enough funds in the non-frozen addresses.

アドレスをフリーズすると、そのアドレスの資金はBitcoinの送信に使用されません。フリーズされていないアドレスに十分な資金がない場合、Bitacoinは送信できません。


How is the wallet encrypted?
----------------------------
ウォレットはどのように暗号化されていますか？
----------------------------------------

Electrum uses two separate levels of encryption:

Electrumは、別々の2つのレベルの暗号化を使用しています。

 - Your seed and private keys are encrypted using AES-256-CBC. The
   private keys are decrypted only briefly, when you need to sign a
   transaction; for this you need to enter your password. This is done
   in order to minimize the amount of time during which sensitive
   information is unencrypted in your computer's memory.

 - シードと秘密鍵はAES-256-CBCを使用して暗号化されます。秘密鍵は、トランザクションに署名する必要がある短かい間だけ復号されます。このためにはあなたはパスワードを入力する必要があります。これは、保護が必要な情報がコンピュータのメモリ内で暗号化されていない時間を最小限に抑えるために行われます。

 - In addition, your wallet file may be encrypted on disk. Note that
   the wallet information will remain unencrypted in the memory of
   your computer for the duration of your session. If a wallet is
   encrypted, then its password will be required in order to open
   it. Note that the password will not be kept in memory; Electrum
   does not need it in order to save the wallet on disk, because it
   uses asymmetric encryption (ECIES).
   
 - さらに、ウォレットファイルはWalletファイルはディスク上で暗号化されている可能性があります。暗号化されている場合は、ウォレットを開くためにパスワードを求められます。パスワードはメモリには保持されません。Electrumは非対称暗号化（ECIES）をしているため、ウォレットをディスクに保存する際にパスワードは必要ありません。

Wallet file encryption is activated by default since version 2.8. It
is intended to protect your privacy, but also to prevent you from
requesting bitcoins on a wallet that you do not control.

ウォレットファイルの暗号化は、バージョン2.8以降ではデフォルトで有効になっています。これはあなたのプライバシーを保護することを目的としていますが、あなたが管理していないウォレットにおいてBitcoinを請求できないようにするためでもあります。


Does Electrum support cold wallets?
-----------------------------------
Electrumはコールドウォレットをサポートしていますか？
------------------------------------------------

Yes, see :ref:`Cold Storage <coldstorage>`.

はい、ref： `Cold Storage <coldstorage>`を参照してください。


Can I import private keys from other Bitcoin clients?
-----------------------------------------------------
他のBitcoinクライアントから秘密鍵をインポートできますか？
----------------------------------------------------

In Electrum 2.0, you cannot import private keys in a wallet that has a
seed. You should sweep them instead.

Electrum 2.0では、シードを持つウォレット内に秘密鍵をインポートすることはできません。代わりにそれらをスイープするしなくてはなりません。

If you want to import private keys and not sweep them, you need to
create a special wallet that does not have a seed.  For this, create a
new wallet, select "restore", and instead of typing your seed, type a
list of private keys, or a list of addresses if you want to create a
watching-only wallet.

秘密鍵をスイープせずにインポートしたい場合は、シードを持たない特別なウォレットを作成する必要があります。このためには、新しいウォレットを作成し「復元(restore)」を選択し、シードを入力するか、秘密鍵のリストを入力するか、閲覧専用ウォレットを作成する場合はアドレスのリストを入力します。


.. image:: png/import_addresses.png


You will need to back up this wallet, because it cannot be
recovered from a seed.

このウォレットはシードから復元できないため、バックアップする必要があります。

Can I sweep private keys from other Bitcoin clients?
----------------------------------------------------
他のBitcoinクライアントから秘密鍵をスイープすることはできますか？
------------------------------------------------------------

Sweeping private keys means to send all the bitcoins they control to
an existing address in your wallet. The private keys you sweep do not
become a part of your wallet.  Instead, all the bitcoins they control
are sent to an address that has been deterministically generated from
your wallet seed.

秘密鍵のスイープとは、その秘密鍵が管理しているすべてのBitcoinをあなたのウォレットの既存アドレス宛に送信することを意味します。スイープする秘密鍵はウォレットの一部にはなりません。代わりに、その秘密鍵が管理しているすべてのBitcoinはあなたのウォレットのシードから確定的に生成されたアドレスに対して送信されます。

To sweep private keys, go to the Wallet menu -> Private Keys ->
Sweep. Enter the private keys in the appropriate field. Leave the
"Address" field unchanged. That is the destination address and it will
be from your existing electrum wallet. Click on "Sweep". It'll now take 
you to the send tab where you can set an appropriate fee and then click
on "Send" to send the coins to your wallet.

秘密鍵をスイープするには、「ウォレット(wallet)」メニュー -> 「秘密鍵(Private Key)」 -> 「スイープ(Sweep)」に移動します。適切なフィールドに秘密鍵を入力します。「アドレス(Address)」フィールドは変更しないでください。それは宛先アドレスであり、あなたの既存のelectrumウォレットから選ばれています。「スイープ(Sweep」をクリックします。「送信(send)」タブに移動するので適切な手数料を設定したらコインをウォレットに送信するために「送信(Send)」をクリックします。

Where is my wallet file located?
--------------------------------
ウォレットファイルはどこにありますか？
----------------------------------

The default wallet file is called default_wallet, which is created when
you first run the application and is located in the /wallets folder.

デフォルトのWalletファイルはdefault_walletと呼ばれ、アプリケーションを最初に実行したときに作成され、/walletsフォルダに格納されています。


On Windows:

 - Show hidden files
 - Go to \\Users\\YourUserName\\AppData\\Roaming\\Electrum\\wallets (or %APPDATA%\\Electrum\\wallets)

Windowsの場合：

 - 隠しファイルを表示する
 - \\Users\\YourUserName\\AppData\\Roaming\\Electrum\\wallets（または％APPDATA％\\Electrum\\wallets）に移動

On Mac:

- Open Finder
- Go to folder (shift+cmd+G) and type ~/.electrum

Macの場合：

- Finderを開く
- フォルダに移動し（shift + cmd + G）、~/.electrumと入力

On Linux:

- Home Folder
- Go -> Location and type ~/.electrum

Linuxの場合

- Homeフォルダ
- ロケーションに移動して ~/.electrumと入力


Can I do bulk payments with Electrum?
-------------------------------------
Electrumで一括支払いができますか？
-------------------------------

You can create a transaction with several outputs. In the GUI, type
each address and amount on a line, separated by a comma.

複数の出力を持つトランザクションを作成することができます。GUIでは各アドレスとその送信額を1行に、カンマで区切ることで入力します。

.. image:: png/paytomany.png

Amounts are in the current unit set in the client. The
total is shown in the GUI.

金額(Amount)は現在クライアントに設定されている単位で指定します。合計がGUIに表示されます。

You can also import a CSV file in the "Pay to" field, by clicking on
the folder icon.

また、フォルダアイコンをクリックして[支払(Pay to)]フィールドにCSVファイルをインポートすることもできます。


Can Electrum create and sign raw transactions?
----------------------------------------------
Electrumは生のトランザクションを作成して署名することはできますか？
------------------------------------------------------------

Electrum lets you create and sign raw transactions right from the user
interface using a form.

Electrumでは、フォームを使用してユーザーインターフェイスから生のトランザクションを作成し署名することができます。

Electrum freezes when I try to send bitcoins.
--------------------------------------------
Bitcoinを送信しようとするとElectrumがフリーズします。
-------------------------------------------------


This might happen if you are trying to spend a large number of
transaction outputs (for example, if you have collected hundreds of
donations from a Bitcoin faucet). When you send Bitcoins, Electrum
looks for unspent coins that are in your wallet in order to create a
new transaction. Unspent coins can have different values, much like
physical coins and bills.

これは多数のトランザクションアウトプットを費やそうとしている場合（たとえばBitcoinのfaucetから数百もの寄付を集めた場合など）に発生する可能性があります。Bitcoinを送信する際に、Electrumは新しいトランザクションを作成するためにウォレット内にある未使用のコインを探します。未使用のコインは、物理的な効果や紙幣と同じように異なった数値を持つことができます。

If this happens, you should consolidate your transaction inputs by
sending smaller amounts of bitcoins to one of your wallet addresses;
this would be the equivalent of exchanging a stack of nickels for a
dollar bill.

このような場合は、ウォレットアドレスの1つに少量のBitcoinを送信してトランザクションインプットを統合する必要があります。これはたくさんの5セント硬貨のを1ドル紙幣と交換するのと同じです。

.. _gap limit:

What is the gap limit?
----------------------
gap limitとは何ですか？
---------------------

gap limit

The gap limit is the maximum number of consecutive unused addresses in
your deterministic sequence of addresses. Electrum uses it in order
to stop looking for addresses. In Electrum 2.0, it is set to 20 by
default, so the client will get all addresses until 20 unused
addresses are found.

gap limitとは決定性を持つ一連のアドレスのうち連続して使用されていないアドレスの最大数です。アドレスをどこまで検索したのち停止するかを決めるためにElectrumはこれを使用しています。Electrum 2.0では、デフォルトで20に設定されているので、クライアントは20の未使用アドレスが見つかるまですべてのアドレスを取得します。

How can I pre-generate new addresses?
-------------------------------------
新しいアドレスを事前に生成するにはどうすればよいですか？
--------------------------------------------------

Electrum will generate new addresses as you use them,
until it hits the `gap limit`_.

Electrumは、あなたがgap limitに達するまで、新しいアドレスを生成してそれらを使用します。

If you need to pre-generate more addresses, you can do so by typing
wallet.create_new_address(False) in the console. This command will generate
one new address. Note that the address will be shown with a red
background in the address tab to indicate that it is beyond the gap
limit. The red color will remain until the gap is filled.

さらに多くのアドレスを事前に生成する必要がある場合は、コンソールにwallet.create_new_address（False）と入力してアドレスを事前に生成することができます。このコマンドは新しいアドレスを1つ生成します。アドレスは、「アドレス(Address)」タブに赤い背景で表示され、gap limitを超えていることを表します。gapが埋まるまで赤色のままです。

WARNING: Addresses beyond the gap limit will not automatically be
recovered from the seed. To recover them will require either increasing
the client's gap limit or generating new addresses until the used
addresses are found.

警告：gap limitを超えたアドレスは自動的にはシードから回復されません。回復するには、クライアントのgap limitを増やすか、使用されたアドレスが見つかるまで新しいアドレスを生成する必要があります。


If you wish to generate more than one address, you can use a "for"
loop. For example, if you wanted to generate 50 addresses, you could
do this:

複数のアドレスを生成する場合は"for"ループを使用できます。たとえば50個のアドレスを生成する場合には次のようにします。

.. code-block:: python

   for x in range(0, 50):
	print wallet.create_new_address(False)


How do I upgrade Electrum?
--------------------------
Electrumをアップグレードするには？
-------------------------------

Warning: always save your wallet seed on paper before
doing an upgrade.

警告：警告：アップグレードを実行する前に、必ず紙にウォレットのシードを保存してください。

To upgrade Electrum, just install the most recent version.
The way to do this will depend on your OS.

Electrumをアップグレードするには、単に最新バージョンをインストールするだけです。方法はお使いのOSによって異なります。

Note that your wallet files are stored separately from the
software, so you can safely remove the old version of the
software if your OS does not do it for you.

ウォレットファイルはソフトウェアとは別に保管されるため、OSが行わない場合には自分自身でソフトウェアの古いバージョンを安全に削除できます。

Some Electrum upgrades will modify the format of your
wallet files.

一部のElectrumアップグレードでは、ウォレットファイルの形式が変更されます。

For this reason, it is not recommended to downgrade
Electrum to an older version once you have opened your
wallet file with the new version. The older version will
not always be able to read the new wallet file.

このため、一度新しいバージョンでウォレットファイルを開いてからElectrumを古いバージョンにダウングレードすることはお勧めしません。古いバージョンでは新しいウォレットファイルを常に読み取ることができるとは限りません。

The following issues should be considered when upgrading
Electrum 1.x wallets to Electrum 2.x:

Electrum 1.xのWalletをElectrum 2.xにアップグレードするときは、次の点を考慮する必要があります。

- Electrum 2.x will need to regenerate all of your
  addresses during the upgrade process. Please allow it
  time to complete, and expect it to take a little longer
  than usual for Electrum to be ready.
  
- Electrum 2.xでは、アップグレード処理中にすべてのアドレスを再生成する必要があります。Electrumが準備完了するまで待ってください。またその際には通常より少し多く時間がかかると考えてください。

- The contents of your wallet file will be replaced with
  an Electrum 2 wallet. This means Electrum 1.x will no
  longer be able to use your wallet once the upgrade is
  complete.
  
- ウォレットファイルの中身はElectrum2ウォレットに置き換えられます。これは一度アップグレードが完了すると、Electrum 1.xはウォレットを使用できなくなることを意味します。

- The "Addresses" tab will not show any addresses the
  first time you launch Electrum 2. This is expected
  behavior. Restart Electrum 2 after the upgrade is
  complete and your addresses will be available.
  
- 始めてElectrum2を起動したときは「アドレス(Addresses)」タブにはアドレスは表示されません。これは想定された動作です。アップグレードが完了したらElectrum2を再起動してください。そうすればアドレスは利用可能になります。

- Offline copies of Electrum will not show the
  addresses at all because it cannot synchronize with
  the network. You can force an offline generation of a
  few addresses by typing the following into the
  Console: wallet.synchronize(). When it's complete,
  restart Electrum and your addresses will once again
  be available.

- Electrumのオフラインコピーには、ネットワークと同期できないためアドレスはまったく表示されません。コンソールに次のように入力すると、少数のアドレスをオフライン生成するように強制できます。：wallet.synchronize()　完了したらElectrumを再起動してください、するとあなたのアドレスが再び利用可能になります。
