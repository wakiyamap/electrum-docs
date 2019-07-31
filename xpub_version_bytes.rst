BIP32拡張公開鍵と秘密鍵のバージョンバイト
========================================================

このドキュメントでは、Electrumでマスターキーに使用されるバージョンバイトについて説明します。

要約
--------

`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__ は
拡張キーのシリアル化フォーマットを定義します。
このシリアル化には、バージョンバイトとして割り当てられた4バイトが含まれます。
これらのバージョンバイトを使用して、ウォレットがこのHDサブツリーに沿って導出する出力スクリプト(scriptPubKeys)のタイプをエンコードします。

動機
----------

SegWitのアクティベーションがなされたことにより
(`BIP141 <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki>`__)
Monacoinメインネットで使える新しい出力スクリプトテンプレートが導入されました。
これはHDウォレットがマスター鍵からどのような種類のスクリプトを導出すべきかという点で新しい問題を提起します。
これまでは、ほとんどのWalletに
`BIP16 <https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki>`__
で導入されP2PKHまたはマルチ署名が埋め込まれて提供されていた
P2SHの出力であり、通常は2つのうちどちらを使用すべきかは属性から推論されました。
私たちはこの知識を明確に持っていた方が良いと信じます。

スクリプトタイプのエンコーディングでは
`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__
拡張キーはWalletに役立ちます。たとえば、閲覧専用ウォレット等。
そうでなければ、拡張された公開鍵から構築されたウォレットは、(1) サブツリー [1]_ 内のすべての可能なスクリプトを派生させるか、 
(2)ユーザにサイドチャネルでスクリプトタイプを入力するように促すかのいずれかでなければなりません。

可能なすべてのスクリプトの導出 (1)

-  リソースを浪費する可能性がある。
   Walletは、着信トランザクションの出力スクリプトをさらに監視する必要があります。
-  key-reuse(鍵の再利用)を導入します。公開鍵は、スクリプトタイプごとに再利用されます。
-  新しいウォレット・ソフトウェアの開発者にとって、
   あらゆるタイプのUTXOからの検出と支出を実装することがますます困難になっています。
   そうでない場合は、資金を検出できなくなる可能性のある従来のスクリプト・タイプを実装しないことを選択します。

拡張された公開キーの他に追加情報としてスクリプトタイプ (2) を入力するようにユーザに促すことは、
より複雑なユーザインタフェースと準最適のエクスペリエンスをもたらすだけです。

ユーザーは、マスターパブリックキー、閲覧専用ウォレット、またはHDマルチシグの連帯保証人を指定するときに、直接操作できます。
これらのキーは、通常次のようにシリアル化されます。
`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__
のスクリプトタイプのエンコーディングをユーザに見えるようにすることは理にかなっています。
このような方法でネットワークを符号化するためにバージョンバイトがすでに使用されているので、
この文書はスクリプトタイプをさらに符号化するためにバージョンバイトの新しい定数を導入します。

考慮事項
--------------

出力スクリプト・タイプの明示的な知識がない場合、
ウォレットは、ユーザーが導出すると予想されるスクリプト・タイプがサポートまたは実装されているかどうかを
ユーザーに伝える明確な方法を持ちません。
一般ユーザーは残高を把握していれば単に残高が不足していると考えるでしょう。

`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__
で定義されているバージョンバイト値はP2PKHとP2SH-multisig出力を区別せず、
下位互換性を維持するために、これは変更されません。
しかしこれは既にいくつかのケースで資金の損失をもたらしている可能性があり、
例えばユーザは彼が制御することができなかったP2SH-multisigに参加しているマスター公開鍵から、
監視のみのP2PKHウォレットの取引を復元し、受け取った(それは別の連帯保証人の鍵だった)。
この種の状況を防ぐために、例えば、P2WPKHとP2WSH-multisigは、この文書で区別されます。

仕様
-------------


-  P2SH stands for a
   `BIP11 <https://github.com/bitcoin/bips/blob/master/bip-0011.mediawiki>`__
   multi-signature script embedded in a
   `BIP16 <https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki>`__
   pay-to-script-hash output
-  P2WPKH stands for pay-to-witness-public-key-hash (witness version 0),
   as in
   `BIP141 <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki#p2wpkh>`__
-  P2WPKH-P2SH stands for a P2WPKH script (witness version 0) nested in
   a
   `BIP16 <https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki>`__
   P2SH output, as in
   `BIP141 <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki#p2wpkh-nested-in-bip16-p2sh>`__
-  P2WSH stands for a
   `BIP11 <https://github.com/bitcoin/bips/blob/master/bip-0011.mediawiki>`__
   multi-signature pay-to-witness-script-hash (witness version 0)
   script, as in
   `BIP141 <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki#p2wsh>`__
-  P2WSH-P2SH stands for a
   `BIP11 <https://github.com/bitcoin/bips/blob/master/bip-0011.mediawiki>`__
   multi-signature pay-to-witness-script-hash (witness version 0) script
   nested in a
   `BIP16 <https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki>`__
   P2SH output, as in
   `BIP141 <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki#p2wsh-nested-in-bip16-p2sh>`__

M-of-Nマルチシグネチャスクリプトは、通常、N個の拡張鍵(Mはサイドチャネルで提供されます。)から構築されることに注意してください。
そのため、ほとんどの場合このようなスクリプトを作成するには複数の拡張キーが必要です。;これはこの文書の範囲外です。

+---------+---------------+----------+---------------+------------------+
| network | script type   | pub/priv | version bytes | | human-readable |
|         |               |          |               | | prefix         |
+=========+===============+==========+===============+==================+
| mainnet | p2pkh or p2sh | public   | 0x0488b21e    | xpub             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2pkh or p2sh | private  | 0x0488ade4    | xprv             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wpkh-p2sh   | public   | 0x049d7cb2    | ypub             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wpkh-p2sh   | private  | 0x049d7878    | yprv             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wsh-p2sh    | public   | 0x0295b43f    | Ypub             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wsh-p2sh    | private  | 0x0295b005    | Yprv             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wpkh        | public   | 0x04b24746    | zpub             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wpkh        | private  | 0x04b2430c    | zprv             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wsh         | public   | 0x02aa7ed3    | Zpub             |
+---------+---------------+----------+---------------+------------------+
| mainnet | p2wsh         | private  | 0x02aa7a99    | Zprv             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2pkh or p2sh | public   | 0x043587cf    | tpub             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2pkh or p2sh | private  | 0x04358394    | tprv             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wpkh-p2sh   | public   | 0x044a5262    | upub             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wpkh-p2sh   | private  | 0x044a4e28    | uprv             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wsh-p2sh    | public   | 0x024289ef    | Upub             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wsh-p2sh    | private  | 0x024285b5    | Uprv             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wpkh        | public   | 0x045f1cf6    | vpub             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wpkh        | private  | 0x045f18bc    | vprv             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wsh         | public   | 0x02575483    | Vpub             |
+---------+---------------+----------+---------------+------------------+
| testnet | p2wsh         | private  | 0x02575048    | Vprv             |
+---------+---------------+----------+---------------+------------------+

下位互換性
-----------------------

この文章では
`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__;
の下位互換性について既存の拡張キーは機能し続けます。新しいスクリプトタイプ用に派生した拡張キーが有効でないという意味で、
`BIP32 <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__
のバージョンバイト互換性は意図的に損なわれています。

脚注
---------

.. [1]
   可能なスクリプトの数が事実上無限であることを考えると、これは不可能です。
   ただし、ウォレットはすべての標準テンプレートのスクリプトを導出できます。

参考資料
---------

-  `BIP11 - M-of-N Standard Transactions
   <https://github.com/bitcoin/bips/blob/master/bip-0011.mediawiki>`__
-  `BIP16 - Pay to Script Hash
   <https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki>`__
-  `BIP32 - Hierarchical Deterministic Wallets
   <https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki>`__
-  `BIP141 - Segregated Witness (Consensus
   layer) <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki>`__
