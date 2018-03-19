JSONRPC vulnerability in Electrum 2.6 to 3.0.4
==============================================
Electrum2.6から3.0.4までのJSONRPCの脆弱性
=======================================

On January 6th, a vulnerability was disclosed in the Electrum wallet
software, that allows malicious websites to execute wallet commands
through JSONRPC executed in a web browser. The bug affects versions
2.6 to 3.0.4 of Electrum, on all platforms. It also affects clones of
Electrum such as Electron Cash.

1月6日、Electrumウォレット内の脆弱性が開示され、それはWebブラウザで実行されるJSONRPCを通したウォレットコマンドの実行を悪意あるWebサイトに可能とするものでした。そのバグはバージョン2.6から3.0.4の全てのプラットフォーム上のElectrumに影響します。Electrum CashのようにElectrumのクローンにもまた影響します。


Can funds be stolen?
--------------------
資金が盗まれるの？
----------------

Wallets that are not password protected are at risk of theft, if they
are opened with a version of Electrum older than 3.0.5 while a web
browser is active.

パスワードで保護されていないウォレットは、もしそれらが3.0.5未満のバージョンのElectrumをWebブラウザがアクティブな状態でオープンしていた場合、盗難のリスクにあります。

In addition, the vulnerability allows an attacker to modify user
settings, the list of contacts in a wallet, and the "payto" and
"amount" fields of the user interface while Electrum is running.

加えて、その脆弱性はユーザー設定、ウォレット内のアドレス帳、ユーザーインターフェイスの"送信先"、"金額"フィールドをElectrumが実行中に変更することを攻撃者に可能にします。

Although there is no known occurrence of Bitcoin theft occurring
because of this vulnerability, the risk increases substantially now
that the vulnerability has been made public.

この脆弱性によるBitcoin盗難の発生は知られていませんが、脆弱性が公になってから今ではリスクは相当大きくなっています。


Can wallet data be leaked?
--------------------------
walletデータが漏れる可能性はあるのか？

Yes, an attacker can obtain private data, such as: Bitcoin addresses,
transaction labels, address labels, wallet contacts and master public
keys.

はい、攻撃者はプライベートなデータ、例えばBitcoinアドレス、トランザクションラベル、アドレスラベル、ウォレットのアドレス帳やマスター公開鍵を手に入れることができます。


Can a password-protected wallet be bruteforced?
-----------------------------------------------
パスワードで保護されたウォレットにブルートフォース（総当たり）攻撃されることはないの？
-------------------------------------------------------------------

Not realistically. The vulnerability does not allow an attacker to
access encrypted seed or private keys, which would be needed in order
to perform an efficient brute force attack. Without the encrypted
seed, an attacker must try passwords using the JSONRPC interface,
while the user is visiting a malicious page. This is several orders of
magnitude slower than an attack with the encrypted seed, and
restricted in time. Even a weak password will protect against that.

現実的ではありません。この脆弱性は効率的なブルートフォース攻撃をするのに必要な暗号化済Seedや秘密鍵に攻撃者がアクセスすることを許すわけではありません。暗号化済Seedなしでは攻撃者は、ユーザーが悪意あるページにアクセスしていてもJSONRPCインターフェイスを通してパスワードにトライしなければなりません。これは暗号化済Seedを用いる攻撃の数桁倍遅く、時間の制限を受けます。弱いパスワードだとしてもそれに対して防御します。


What should users do?
---------------------
ユーザーは何をするべきなの？
-------------------------

All users should upgrade their Electrum software, and stop using old
versions.

全てのユーザーは彼らのElectrumソフトウェアをアップグレードし、旧バージョンの使用をストップする必要があります。

Users who did not protect their wallet with a password should create a
new wallet, and move their funds to that wallet. Even if it never
received any funds, a wallet without password should not be used
anymore, because its seed might have been compromised.

ウォレットをパスワードで保護していないユーザーは新しいウォレットを作成し、資金をそのウォレットに移さなければなりません。たとえ資金を受け取ったことがなくてもパスワードなしのウォレットはいかなる場合も使用すべきではありません。なぜならSeedが感染しているかもしれないからです。

In addition, users should review their settings, and delete all
contacts from their contacts list, because the Bitcoin addresses of
their contacts might have been modified.

加えて、ユーザーは彼らの設定を見直し、アドレス帳からすべての送信先を削除しなければなりません。なぜなら、それらのアドレス帳のBitcoinアドレスは改ざんされているかもしれないからです。


How to upgrade Electrum
-----------------------
Electrumのアップグレード方法
--------------------------

Stop running any version of Electrum older than 3.0.5, and install
Electrum the most recent version. On desktop, make sure you download
Electrum from https://electrum.org and no other website. On Android,
the most recent version is available in Google Play.

3.0.5未満のいかなるバージョンのElectrumも実行を停止し、最新バージョンをインストールしてください。デスクトップでは、あなたがそれ以外のサイトではなくhttps://electrum.orgからダウンロードしたことを確かめて下さい。AndroidではGoogle Playで最新バージョンは利用可能です。

If Electrum 3.0.5 (or any later version) cannot be installed or does
not work on your computer, stop using Electrum on that computer, and
access your funds from a device that can run Electrum 3.0.5. If you
really need to use an older version of Electrum, for example in order
to access wallet seed, make sure that your computer is offline, and
that no web browser is running on the computer at the same time.

もしElectrum3.0.5（または他の新しいバージョン）がインストールできない、またはあなたのコンピュータで動かない場合、そのコンピュータでElectrumを使用するのは停止して、Electrum3.0.5が実行可能な端末から資金にアクセスしてください。もしあなたが本当にElectrumの旧バージョンを使わなければならない場合、例えばウォレットSeedにアクセスする場合、コンピュータがオフラインであること、Webブラウザがそのコンピュータ上で同時に実行していないことを確認してください。


Should all users move their funds to a new address?
---------------------------------------------------
すべてのユーザーが新しいアドレスに資金を移さなければならないの？
---------------------------------------------------------

No, we do not recommend moving funds from password-protected wallets. 

いいえ、パスワードで保護されたウォレットから資金を移動することは勧めません。

When was the issue reported and fixed?
--------------------------------------
問題はいつ報告されていつ修正されたの？
----------------------------------

The absence of password protection in the JSONRPC interface was
reported on November 25th, 2017 by user jsmad:
https://github.com/spesmilo/electrum/issues/3374

JSONRPCインターフェイス内のパスワード保護の欠乏は11月25日にユーザーであるjsmadから報告されました。

jsmad's report was about the Electrum daemon, a piece of software that
runs on web servers and is used by merchants in order to receive
Bitcoin payments. In that context, connections to the daemon from the
outside world must be explicitly authorized, by setting 'rpchost' and
'rpcport' in the Electrum configuration.

jsmadの報告はElectrumデーモン、Webサーバ上で実行していて、Bitcoinの支払いを受け取るために業者に使用されているソフトウェアの一つについてでした。その状況でデーモンへの外の世界からの接続は'rpchost'と'rpcport'をElectrumに設定して明白に権限を与えられている必要があります。

On January 6th, 2018, Tavis Ormandy demonstrated that the JSONRPC
interface could be exploited against the Electrum GUI, and that the
attack could be carried out by a web browser running locally, visiting
a webpage with specially crafted JavaScript.

2018年の1月6日、Tavis OrmandyがJSONRPCインターフェイスがElectrum GUIに対して悪用される可能性があること、Javascriptで特別に作成されたWebページに訪れることでローカルで実行されているWebブラウザによる攻撃が実行されることのデモンストレーションをしました。

We released a new version (3.0.4) in the hours following Tavis' post,
with a patch written by mithrandi (Debian packager), that addressed
the attack demonstrated by Tavis. In addition, the Github issue
remained open, because mithrandi's patch was not adding password
protection to the JSONRPC interface.

Tavisの投稿に続いて我々は新バージョン(3.0.4）をリリースしました。mithrandi(Debian Packager)によって書かれたパッチが充てられていてTavisによってデモされた攻撃に対処しています。加えて、mithrandiのパッチはJSONRPCインターフェイスへのパスワード保護は追加されていなかったためGithub issueは開いたままでした。

Shortly after the 3.0.4 release we started to work on adding proper
password protection to the JSONRPC interface of the daemon, and that
part was ready on Sunday, January 7th. We also learned on Sunday
afternoon that the first patch was not effective against another,
similar attack, using POST. This is why we did not delay the 3.0.5
release, which includes password protection, and completely disables
JSONRPC in the GUI.

3.0.4のリリース直後、デーモンのJSONRPCインターフェイスに対する適切なパスワード保護の追加に取り組み始めました。そのパートは1月7日、日曜日に始まりました。日曜の午後に最初のパッチはPOSTを使ったような同じような攻撃に対して効果的でないことを知りました。これが我々が3.0.5のリリースに遅れた理由であり、リリースにはパスワード保護とGUIにおける完全なJSONRPCの無効化を含んでいました。
