.. _spv:

Simple Payment Verification
===========================
簡易支払い検証
============

Simple Payment Verification (SPV) is a technique described
in Satoshi Nakamoto's paper. SPV allows a lightweight
client to verify that a transaction is included in the
Bitcoin blockchain, without downloading the entire
blockchain. The SPV client only needs download the block
headers, which are much smaller than the full blocks. To
verify that a transaction is in a block, a SPV client
requests a proof of inclusion, in the form of a Merkle
branch.

SPV clients offer more security than web wallets, because
they do not need to trust the servers with the information
they send.

Simple Payment Verification（SPV）は、ナカモトサトシの論文に記載されている手法です。SPVによって、ブロックチェーン全体をダウンロードすることなく、Bitcoinブロックチェーンにトランザクションが含まれていることを確認できる軽量クライアントができます。SPVクライアントでは、完全なブロックよりはるかにサイズが小さいブロックヘッダをダウンロードするだけで済みます。トランザクションがブロック内に含まれていることを確認するために、SPVクライアントはマークルブランチの形式で内容の証明を要求します。

SPVクライアントは、送信された情報とサーバを信用する必要がないため、Webウォレットよりも高いセキュリティを提供します。

Reference: `Bitcoin: A peer-to-peer Electronic Cash System <http://bitcoin.org/bitcoin.pdf>`_

参照： `Bitcoin: A peer-to-peer Electronic Cash System <http://bitcoin.org/bitcoin.pdf>`_

