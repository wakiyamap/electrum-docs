Two Factor Authentication
=========================
2段階認証
========

Electrum offers two-factor authenticated wallets, with a remote server
acting to co-sign transactions, adding another level of security in
the event of your computer being compromised.

Electrumはトランザクションに共同署名するように動作するリモートサーバを使い、あなたのコンピュータが感染した場合には違うレベルのセキュリティを追加する2段階認証ウォレットを提供しています。

The remote server in question is a service offered by TrustedCoin.
Here is a guide_ on how it works.

問題のリモートサーバはTrustedCoinによってサービスが提供されています。それがどのように動いているかについては下のガイドがあります。

.. _guide: https://api.trustedcoin.com/#/electrum-help

Creating a Wallet
-----------------
ウォレットを作成する
------------------

.. image:: png/Create_2fa.png


.. image:: png/TrustedCoin.png


Restoring from seed
-------------------
Seedから復元する
--------------

Even if TrustedCoin is compromised or taken offline, your coins are
secure as long as you still have the seed of your wallet. Your seed
contains two master private keys in a 2-of-3 security scheme. In
addition, the third master public key can be derived from your seed,
ensuring that your wallet addresses can be restored. In order to
restore your wallet from seed, select "wallet with two factor
authentication", as this tells Electrum to use this special variety of
seed for restoring your wallet.

もしTrustedCoinが感染するかオフラインになったとしても、あなたがウォレットのSeedを持っている限りコインは安全です。あなたのSeedは2of3セキュリティスキームの中の二つのマスター秘密鍵を含んでいます。加えて、三つ目のマスター公開鍵はあなたのSeedから生成することができ、確実にあなたのウォレットアドレスは復元することができます。Seedからウォレットを復元するためには、"wallet with two factor authentication"を選択し、Electrumにこの特別でさまざまなSeedをウォレットの復元のために使用することを伝えます。

.. image:: png/Restore_2fa.png


Note: The "restore" option should be used only if you no longer want
to use TrustedCoin, or if there is a problem with the service. Once
you have restored your wallet in this way, two of three factors are on
your machine, negating the special protection of this type of
wallet.

注意：復元オプションはあなたがもうTrustedCoinを利用したくない場合、もしくはTrustedCoinサービスに問題が生じた場合のみ、使用してください。一度この方法でウォレットを復元した場合、三つの構成要素のうち二つはあなたのマシン上にあり、2段階認証タイプのこのウォレットの特別な保護は無効になります。



