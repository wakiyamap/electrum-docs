
How to split your coins using Electrum in case of a fork
========================================================
フォークが発生した場合のElectrumを用いたコインの分離方法
===================================================

Note:
-----
注意：
-----

This document has been updated for Electrum 2.9.

この文書はElectrum2.9用に更新されています。

What is a fork?
---------------
フォークとは何ですか？
-------------------

A blockchain fork (or blockchain split) occurs when a deviating
network begins to generate and maintain a conflicting chain of blocks
branching from the original, essentially creating another "version of
bitcoin" or cryptocurrency, with its very own blockchain, set of
rules, and market value.

ブロックチェーンのフォーク（分岐）は、逸脱するネットワークがオリジナルのチェーンから枝分かれして競合するブロックのチェーンが生成、維持されると発生します。本質的にBitcoin（暗号通貨）の別のバージョンが生まれ、独自のブロックチェーン、ルールセット、市場価値を持ちます。

If there is a fork of the Bitcoin blockchain, two distinct currencies
will coexist, having different market values.

Bitcoinブロックチェーンのフォークが発生した場合、二つの別々の通貨が同時に存在し、異なる市場価値を持ちます。


What does it mean to 'split your coins'?
----------------------------------------
コインを分離するとはどういう意味ですか？
------------------------------------

An address on the original blockchain will now also contain the same
amount on the new chain.

オリジナルブロックチェーン上のアドレスには新しいチェーン上での同じ金額が含まれています。

If you own Bitcoins before the fork, a transaction that spends these
coins after the fork will, in general, be valid on both chains. This
means that you might be spending both coins simultaneously. This is
called 'replay'. To prevent this, you need to move your coins using
transactions that differ on both chains.

フォーク以前にBitcoinを所持していた場合、フォーク後にこれらのコインを使用するトランザクションは一般的には両方のチェーンで有効です。つまり、あなたは両方のコインを同時に使用するかもしれないということです。これは「リプレイ」と呼ばれています。これを防ぐには、両方のチェーンで異なるトランザクションを使用してコインを移動させる必要があります。



Fork detection
--------------
フォークの検出
-------------

Electrum (version 2.9 and higher) is able to detect consensus failures
between servers (blockchain forks), and lets users select their branch
of the fork.

Electrum(バージョン2.9以上)はサーバ間のコンセンサス障害（ブロックチェーンのフォーク）を検出し、ユーザーがフォークのブランチを選択できるようにします。

* Electrum will download and validate block headers sent by servers
  that may follow different branches of a fork in the Bitcoin
  blockchain. Instead of a linear sequence, block headers are
  organized in a tree structure. Branching points are located
  efficiently using binary search. The purpose of MCV is to detect and
  handle blockchain forks that are invisible to the classical SPV
  model.
  
* ElectrumはBitcoinブロックチェーンにおけるフォークの、異なるブランチに追従しているかもしれない複数のサーバから送信されるブロックヘッダをダウンロードし検証します。線形性シーケンスの代わりにブロックヘッダは木構造で編成されます。分岐点は、バイナリ検索を使用して効率的に配置されます。MCVの目的は、古典的なSPVモデルでは見えないブロックチェーンフォークを検出して処理することです。
    
* The desired branch of a blockchain fork can be selected using the
  network dialog. Branches are identified by the hash and height of
  the diverging block. Coin splitting is possible using RBF
  transaction (a tutorial will be added).
  
* ブロックチェーンフォークの希望のブランチはネットワークダイヤログを使用して選択できます。ブランチは、分岐ブロックのハッシュと高さによって識別されます。RBFトランザクションを使用してコインの分離が可能です（チュートリアルが追加されます）。


This feature allows you to pick and choose which chain and network you spend on.

この機能を使用すると、あなたがコインを消費するチェーンとネットワークを選別することができます。


Procedure
---------
手順
----

   1. Preparation

      a. Menu ➞ View ➞ Show Coins
      b. Menu ➞ Tools ➞ Preferences ➞ Propose Replace-By-Fee ➞ "Always"
      
   1. 準備
  
     a. メニュー ➞ 表示 ➞ 表示 コイン
     b. メニュー ➞ ツール ➞ 設定 ➞ Propose Replace-By-Fee ➞ "Always"
     
  2. Select a chain / network

      a. Menu ➞ Tools ➞ Network

         Notice how the branches have different hashes at different heights.
         You can verify which chain you're on by using block explorers to verify
         the hash and height.
  
  2. チェーン/ネットワークを選択する
  
     a. メニュー ➞ ツール ➞ ネットワーク
     
         ブランチは異なる高さでは異なるハッシュを持つことに注意してください。ブロックエクスプローラーを使ってハッシュと高さを確認することであなたが度のチェーン上にいるか確認することができます。

            .. image:: png/coin_splitting/select_main_chain.png
            .. image:: png/coin_splitting/chain_search_height.png
            .. image:: png/coin_splitting/chain_verify_hash.png

   3. Send your coins to yourself

      a. Copy your receiving address to the sending tab.
      b. Enter how many coins you'd like to split. (enter " ! " for ALL)
      c. Check "Replaceable"
      d. Send ➞ Sign ➞ Broadcast
      
   3. あなた自身にコインを送信する
   
      a. 受信アドレスを送信タブにコピー
      b. 分離したいコインの数を入力し素（全ての場合"!"を入力）
      c. "Replaceable"をチェック
      d. 送信 ➞ 署名 ➞ 発信

   4. Wait for the transaction to confirm on one network.

      a. You'll want to switch between chains (via the network panel)
         to monitor the transaction status.
         
   4. 1つのネットワークにトランザクションが承認されるまで待つ
   
      a. ネットネットワークパネルを介してチェーンを切り替え、トランザクションステータスを監視する

      b. Wait until you see the transaction confirm on one chain.
      
      b. 1つのチェーン上でトランザクションが承認されるのを確認するまで待つ

         .. image:: png/coin_splitting/unconfirmed.png
         .. image:: png/coin_splitting/confirmed.png

      c. Immediately use "RBF" on the unconfirmed transaction to "Increase fee"
      
      c. 即座に未承認トランザクションで"RBF"を使用して"Increase fee"を行う

         .. image:: png/coin_splitting/increase_fee.png

   5. Wait for both chains to confirm the transaction.
   
   5. トランザクションが両方のチェーンで承認されるのを待つ

   6. Verify the transaction has a different TXID on each chain.

   6. トランザクションがそれぞれのチェーンで異なるTXIDを持っていることを確認する

         .. image:: png/coin_splitting/main_chain_txid.png
         .. image:: png/coin_splitting/alternate_chain_txid.png

You will now have coins seperately spendable on each chain.  If it failed,
no harm done, you sent to yourself!  Just try again.

これでもうコインがそれぞれのチェーンで別々に使用できます。もし失敗しても、あなた自身に送信しているため害はありません。もう一度挑戦してください。
