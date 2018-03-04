Invoices
========
請求書-インボイス-
===============

Invoices are Payment Requests signed by their requestor.

インボイスは請求主によって署名された支払いリクエストです。

When you click on a bitcoin: link, a URL is passed to
electrum:

bitcoinリンクをクリックするとURLがElecteumに渡されます。

.. code-block:: bash

   electrum "bitcoin:1KLxqw4MA5NkG6YP1N4S14akDFCP1vQrKu?amount=1.0&amp;r=https%3A%2F%2Fbitpay.com%2Fi%2FXxaGtEpRSqckRnhsjZwtrA"


This opens the send tab with the payment request:

これにより支払いリクエストとともに「送信(Send)」タブが開きます。

.. image:: png/PaymentRequest.png

The green color in the "Pay To" field means that the payment request
was signed by bitpay.com's certificate, and that Electrum verified the
chain of signatures.

「送金先(Pay To)」フィールドの緑色は支払いリクエストがbitpay.comの証明書で署名され、Electrumが一連の署名を検証したことを意味します。

Note that the "send" tab contains a list of invoices and their status.

「送信(Send)」タブにはインボイスのリストとそのステータスが含まれています。

