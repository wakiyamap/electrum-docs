Electrum Seed Version System
============================
Electrum Seedバージョンシステム
=============================


This document describes the Seed Version System used in Electrum
(version 2.0 and higher).

このドキュメントではElectrum（バージョン2.0以上）で用いられているSeedバージョンシステムのことを説明しています。

Description
-----------
説明
----

Electrum derives its private keys and addresses from a seed phrase
made of natural language words. Starting with version 2.0, Electrum
seed phrases include a version number, whose purpose is to indicate
which derivation should be followed in order to derive private keys
and addresses.

Electrumは自然言語で出来たSeedフレーズから秘密鍵とアドレスを導き出しています。バージョン2.0以降、Electrum Seedフレーズはバージョン番号を含み、その目的は秘密鍵とアドレスを導き出すためにはどのデリベーションをたどるべきかを示すことです。


In order to eliminate the dependency on a fixed wordlist, the master
private key and the version number are both obtained by hashes of the
UTF8 normalized seed phrase. The version number is obtained by looking
at the first bits of:

固定単語リストへの依存を排除するために、マスター秘密鍵およびバージョン番号は両方とも、UTF8正規化されたSeedフレーズのハッシュによって得られます。バージョン番号は次の最初のビットから得られます。

.. code-block:: python

    hmac_sha_512("Seed version", seed_phrase)

The version number is also used to check seed integrity; in order to
be correct, a seed phrase must produce a registered version number.

バージョン番号はSeedの整合性を確かめるためにも使用されます。間違いのないように、Seedフレーズは登録済のバージョン番号を提示しなければなりません。

Motivation
----------
動機
----

Early versions of Electrum (before 2.0) used a bidirectional encoding
between seed phrase and entropy. This type of encoding requires a
fixed wordlist. This means that future versions of Electrum must ship
with the exact same wordlist, in order to be able to read old seed
phrases.

旧バージョンのElectrum(2.0より前)はSeedフレーズとエントロピーの双方向エンコーディングを用いていました。このタイプのエンコーディングは固定の単語リストを必要とします。これが意味するのは将来のバージョンのElectrumが過去のSeedフレーズの解読を可能にするために全く同じ単語リストを備えなければならないということです。

BIP39 was introduced two years after Electrum. BIP39 seeds include a
checksum, in order to help users figure out typing errors. However,
BIP39 suffers the same shortcomings as early Electrum seed phrases:

BIP39はElectrumの二年後に発表されました。BIP39のSeedはチェックサムを含み、タイプミスを検出するのを補助しています。しかしながらBIP39にはかつてのElectrum Seedフレーズと同じ欠点があります：

 - A fixed wordlist is still required. Following our recommendation,
   BIP39 authors decided to derive keys and addresses in a way that
   does not depend on the wordlist. However, BIP39 still requires the
   wordlist in order to compute its checksum, which is plainly
   inconsistent, and defeats the purpose of our recommendation. This
   problem is exacerbated by the fact that BIP39 proposes to create
   one wordlist per language. This threatens the portability of BIP39
   seed phrases.
   
 - 固定の単語リストが依然として要求されます。我々の勧告を受けて、BIP39の著者は鍵とアドレスを生成するのに単語リストには依存しない方法をとりました。しかしながら、BIP39はチェックサムを計算するためにその単語リストを必要とします。チェックサムには明らかに矛盾があり、我々の勧告の目的を台無しにさせています。この問題はBIP39が言語ごとに一つの単語リストを作成しようと提案したことでさらに悪化しました。これがBIP39 Seedフレーズの移植性を脅かしています。


 - BIP39 seed phrases do not include a version number. This means that
   software should always know how to generate keys and
   addresses. BIP43 suggests that wallet software will try various
   existing derivation schemes within the BIP32 framework. This is
   extremely inefficient and rests on the assumption that future
   wallets will support all previously accepted derivation
   methods. If, in the future, a wallet developer decides not to
   implement a particular derivation method because it is deprecated,
   then the software will not be able to detect that the corresponding
   seed phrases are not supported, and it will return an empty wallet
   instead. This threatens users funds.
   
 - BIP39 Seedフレーズはバージョン番号を含みません。つまりソフトウェアは常に鍵とアドレスを生成する方法を知っていなければなりません。BIP43ではウォレットソフトウェアはBIP32フレームワーク内に存在する様々な生成スキームを試行するようにすることを提案しています。これは著しく非効率的であり、今後のウォレットがそれまでに利用された生成メソッドのすべてをサポートするという仮定に基づいています。もし将来、ウォレットの開発者が特定の生成メソッドは廃止予定だからと、その実行をやめることに決めた場合、そのソフトウェアは、サポートしていない該当Seedフレーズを検出することができず、代わりに空のウォレットを返すでしょう。これはユーザーの資産を脅かします。


For these reasons, Electrum does not generate BIP39 seeds. Starting
with version 2.0, Electrum uses the following Seed Version System,
which addresses these issues.

これらの理由から、ElectrumはBIP39 Seedの生成は行ないません。バージョン2.0から、Electrumは以下のSeedバージョンシステムを使用してこれらの問題に対処しています。

Electrum 2.0 derives keys and addresses from a hash of the UTF8
normalized seed phrase with no dependency on a fixed wordlist.
This means that the wordlist can differ between wallets while the seed remains
portable, and that future wallet implementations will not need
today's wordlists in order to be able to decode the seeds
created today. This reduces the cost of forward compatibility.

Electrum2.0はUTF8正規化された固定の単語リストに依存しないSeedフレーズのハッシュから鍵とアドレスを生成します。つまり単語リストはSeedは移植性を保ったままウォレット間で異なることができます。そして今日生成されたSeedをデコードするこために、将来のウォレットの実装では今日の単語リストを必要としません。これによって前方互換性のコストを減らしています。



Version number
--------------
バージョン番号
------------

The version number is a prefix of a hash derived from the seed
phrase. The length of the prefix is a multiple of 4 bits. The prefix
is computed as follows:

バージョン番号はSeedフレーズから生成されたハッシュのプレフィックスです。長さは4bitの倍数で、以下のように計算されます：

.. code-block:: python

  def version_number(seed_phrase):
    # normalize seed
    normalized = prepare_seed(seed_phrase)
    # compute hash
    h = hmac_sha_512("Seed version", normalized)
    # use hex encoding, because prefix length is a multiple of 4 bits
    s = h.encode('hex')
    # the length of the prefix is written on the fist 4 bits
    # for example, the prefix '101' is of length 4*3 bits = 4*(1+2)
    length = int(s[0]) + 2
    # read the prefix
    prefix = s[0:length]
    # return version number
    return hex(int(prefix, 16))

The normalization function (prepare_seed) removes all but one space
between words. It also removes diacritics, and it removes spaces
between Asian CJK characters.

正規化関数(prepare_seed)は単語の間のスペース1つを除いてすべて削除します。また発音区別記号、アジアのCJK文字(CJK characters)も削除されます。



List of reserved numbers
------------------------
登録済み番号のリスト
------------------

The following version numbers are used for Electrum seeds.

以下のバージョン番号がElectrum Seedに使われています。

======== ========= =====================================
Number   Type      Description
======== ========= =====================================
0x01     Standard  P2PKH and Multisig P2SH wallets
0x100    Segwit    Segwit: P2WPKH and P2WSH wallets
0x101    2FA       Two-factor authenticated wallets
======== ========= =====================================

In addition, the version bytes of master public/private keys indicate
what type of output script should be used. The following prefixes are
used for master public keys:

加えて、マスター公開鍵/秘密鍵のバージョンバイトどのタイプのアウトプットスクリプトが使用されるべきかを示しています。以下のプレフィックスがマスター公開鍵に使用されています。

========== =========== ===================================
Version    Prefix      Description
========== =========== ===================================
0x0488b21e xpub        P2PKH or P2SH
0x049d7cb2 ypub        P2WPKH in P2SH
0x0295b43f Ypub        P2WSH in P2SH
0x04b24746 zpub        P2WPKH
0x02aa7ed3 Zpub        P2WSH
========== =========== ===================================

And for master private keys:

マスター秘密鍵

========== =========== ===================================
Version    Prefix      Description
========== =========== ===================================
0x0488ade4 xprv        P2PKH or P2SH
0x049d7878 yprv        P2WPKH in P2SH
0x0295b005 Yprv        P2WSH in P2SH
0x04b2430c zprv        P2WPKH
0x02aa7a99 Zprv        P2WSH
========== =========== ===================================


Seed generation
---------------
Seed生成
--------

When the seed phrase is hashed during seed generation, the resulting hash must
begin with the correct version number prefix. This is achieved by enumerating a
nonce and re-hashing the seed phrase until the desired version number is
created. This requirement does not decrease the security of the seed (up to the
cost of key stretching, that might be required to generate the private keys).

Seed生成においてSeedフレーズがハッシュ化されると、結果のハッシュ値は正しいバージョン番号プレフィックスで始まらなければいけません。これは望むバージョン番号が作成されるまでnonceを挙げてSeedフレーズをハッシュ化しなおすことで達成されます。この要求はSeedのセキュリティを下げることはありません。（秘密鍵を生成するのに要求されるキーストレッチングのコスト次第）


Security implications
---------------------
セキュリティ推測
---------------

Electrum currently use the same wordlist as BIP39 (2048 words). A
typical seed has 12 words, which results in 132 bits of entropy in the
choice of the seed.

Electrumは現在はBIP39(2048単語)と同じ単語リストを使用しています。代表的なSeedは12単語を有しており、Seedを選ぶ際には132bitのエントロピーとなるようにします。

Following BIP39, 2048 iterations of key stretching are added for the
generation of the master private key. In terms of hashes, this is
equivalent to adding an extra 11 bits of security to the seed
(2048=2^11).

BIP39に従って、キーストレッチングの2048ループがマスター秘密鍵の生成には追加されます。ハッシュに関して、これはSeedのセキュリティにさらなる11bitを追加することと同等です（2048=2^11）。

From the point of view of an attacker, the constraint added by
imposing a prefix to the seed version hash does not decrease the
entropy of the seed, because there is no knowledge gained on the seed
phrase. The attacker still needs to enumerate and test 2^n candidate
seed phrases, where n is the number of bits of entropy used to
generate the seed.

攻撃者の観点からすると、Seedバージョンハッシュのプレフィックスを課すことで追加された制約はSeedのエントロピーを減少させることはありません。なぜならSeedフレーズから得られる情報はないからです。攻撃者は2^nの候補Seedフレーズを列挙する必要があり、nはSeedを生成するのに使われたエントロピーのbit数です。


However, the test made by the attacker will return faster if the
candidate seed is not a valid seed, because the attacker does not need
to generate the key. This means that the imposed prefix reduces the
strength of key stretching.

しかしながら、攻撃者によって作成されたテストは候補Seedが有効なSeedでない場合即座に返ってきます。なぜなら攻撃者は鍵を生成する必要がないからです。つまり付与されたプレフィックスはキーストレッチングの長さを削減しているということです。

Let n denote the number of entropy bits of the seed, and m the number
of bits of difficulty added by key stretching: m =
log2(stretching_iterations). Let k denote the length of the prefix, in
bits.

nはSeedのエントロピーbitの数を示しており、mはキーストレッチングによって追加された難易度のbit数を示しています。m=log2(stretching_iterations)。kはビットの中のプレフィックスの長さを示しています。

On each iteration of the attack, the probability to obtain a valid seed is p = 2^-k

攻撃の各反復における、有効なSeedを得る確率 p = 2^-k

The number of hashes required to test a candidate seed is: p * (1+2^m) + (1-p)*1 = 1 + 2^(m-k)

候補Seedをテストするのに求められるハッシュの数 p * (1+2^m) + (1-p)*1 = 1 + 2^(m-k)

Therefore, the cost of an attack is: 2^n * (1 + 2^(m-k))

ゆえに、攻撃コストは 2^n * (1 + 2^(m-k))

On each iteration of the attack, the probability to obtain a valid seed is p = 2^-k


The number of hashes required to test a candidate seed is: p * (1+2^m) + (1-p)*1 = 1 + 2^(m-k)


Therefore, the cost of an attack is: 2^n * (1 + 2^(m-k))


This can be approximated as 2^(n + m - k) if m>k and as 2^n otherwise.

これは2^(n + m - k)、または2^nと近似することができます。

With the standard values currently used in Electrum, we obtain:
2^(132 + 11 - 8) = 2^135. This means that a standard Electrum seed
is equivalent, in terms of hashes, to 135 bits of entropy.

Electrumに現在使用されている標準値は 2^(132 + 11 - 8) = 2^135 です。つまり標準のElectrum Seedはハッシュに関して135bitエントロピーに相当するということです。

