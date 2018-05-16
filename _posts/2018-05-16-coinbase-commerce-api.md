---
layout: post
title:  "Tích hợp Coinbase Commerce API"
date:   2018-05-16 11:23:23 +0700
categories: php
---

Hi các bạn,

Hôm trước mình có cái task tích hợp phiên bản mới của Coinbase là [Coinbase Commerce API](https://commerce.coinbase.com/docs/api/).

Đau cái là bọn này chưa có sanbox để test, phải xài tiền thật cho nên mình sẽ chia sẻ log request của Coinbase Commerce cho các bạn khỏi mất $.

I. Log type: `charge:created`

```json
{
  "attempt_number": 1,
  "event": {
    "api_version": "2018-03-22",
    "created_at": "2018-05-11T10:46:10Z",
    "data": {
      "code": "YRDKM6DA",
      "name": "Doanh Ka Ka",
      "pricing": {
        "local": {
          "amount": "1.00",
          "currency": "USD"
        },
        "bitcoin": {
          "amount": "0.00011454",
          "currency": "BTC"
        },
        "ethereum": {
          "amount": "0.001448000",
          "currency": "ETH"
        },
        "litecoin": {
          "amount": "0.00704597",
          "currency": "LTC"
        },
        "bitcoincash": {
          "amount": "0.00071835",
          "currency": "BCH"
        }
      },
      "metadata": {
        "ahihi": "ihaha",
        "ref_id": "d1234567890"
      },
      "payments": [],
      "timeline": [
        {
          "time": "2018-05-11T10:46:10Z",
          "status": "NEW"
        }
      ],
      "addresses": {
        "bitcoin": "174ysP94X16Y6gEX5Jrmq3ZQaXG6Ykox4o",
        "ethereum": "0x6e7f062e27a89a3925cce58bee4631d07c2a764b",
        "litecoin": "LLTdrPqCyCpePmrJp6SfD2EmcAnRuUXFD3",
        "bitcoincash": "qph7j5wuc2duggp522n6ykvpgf6qhuyr4sugyyaqlw"
      },
      "created_at": "2018-05-11T10:46:10Z",
      "expires_at": "2018-05-11T11:01:10Z",
      "hosted_url": "https://commerce.coinbase.com/charges/YRDKM6DA",
      "description": "Paymentwall Inc",
      "pricing_type": "fixed_price"
    },
    "id": "98c10800-9897-466a-ba81-b79d4c6573ec",
    "type": "charge:created"
  },
  "id": "a3c36bd9-79a6-453b-9857-530475016be4",
  "scheduled_for": "2018-05-11T10:46:10Z"
}
```

II. Log type: `charge:confirmed`

```json
{
  "attempt_number": 1,
  "event": {
    "api_version": "2018-03-22",
    "created_at": "2018-05-11T10:58:49Z",
    "data": {
      "code": "YRDKM6DA",
      "name": "Doanh Ka Ka",
      "pricing": {
        "local": {
          "amount": "1.00",
          "currency": "USD"
        },
        "bitcoin": {
          "amount": "0.00011454",
          "currency": "BTC"
        },
        "ethereum": {
          "amount": "0.001448000",
          "currency": "ETH"
        },
        "litecoin": {
          "amount": "0.00704597",
          "currency": "LTC"
        },
        "bitcoincash": {
          "amount": "0.00071835",
          "currency": "BCH"
        }
      },
      "metadata": {
        "ahihi": "ihaha",
        "ref_id": "d1234567890"
      },
      "payments": [
        {
          "block": {
            "hash": "0xcccf08e7f01c5436b973cde3583fc196ef63b0926f0a49a44f8172a22ee0b0e1",
            "height": 5594594,
            "confirmations": 7,
            "confirmations_required": 8
          },
          "value": {
            "local": {
              "amount": "1.00",
              "currency": "USD"
            },
            "crypto": {
              "amount": "0.001448000",
              "currency": "ETH"
            }
          },
          "status": "CONFIRMED",
          "network": "ethereum",
          "transaction_id": "0x2e9f62593c69eed999f1632d68e60f327e708771e780f05d0c71f1db9d6eeac8"
        }
      ],
      "timeline": [
        {
          "time": "2018-05-11T10:46:10Z",
          "status": "NEW"
        },
        {
          "time": "2018-05-11T10:55:27Z",
          "status": "PENDING",
          "payment": {
            "network": "ethereum",
            "transaction_id": "0x2e9f62593c69eed999f1632d68e60f327e708771e780f05d0c71f1db9d6eeac8"
          }
        },
        {
          "time": "2018-05-11T10:58:49Z",
          "status": "COMPLETED",
          "payment": {
            "network": "ethereum",
            "transaction_id": "0x2e9f62593c69eed999f1632d68e60f327e708771e780f05d0c71f1db9d6eeac8"
          }
        }
      ],
      "addresses": {
        "bitcoin": "174ysP94X16Y6gEX5Jrmq3ZQaXG6Ykox4o",
        "ethereum": "0x6e7f062e27a89a3925cce58bee4631d07c2a764b",
        "litecoin": "LLTdrPqCyCpePmrJp6SfD2EmcAnRuUXFD3",
        "bitcoincash": "qph7j5wuc2duggp522n6ykvpgf6qhuyr4sugyyaqlw"
      },
      "created_at": "2018-05-11T10:46:10Z",
      "expires_at": "2018-05-11T11:01:10Z",
      "hosted_url": "https://commerce.coinbase.com/charges/YRDKM6DA",
      "description": "Paymentwall Inc",
      "confirmed_at": "2018-05-11T10:58:49Z",
      "pricing_type": "fixed_price"
    },
    "id": "6272ef4e-6377-49c5-bd8c-bdefbcf2cbd8",
    "type": "charge:confirmed"
  },
  "id": "453f1795-9d07-40e2-b7b3-463c22525101",
  "scheduled_for": "2018-05-11T10:58:49Z"
}
```
III. Đây là đống header mà Coinbase gửi cho mềnh khi Payment completed:

```json
{
  "user-agent": "weipay-webhooks",
  "content-type": "application/json",
  "x-cc-webhook-signature": "1a0f37052ae875f067f4d510efd146d50ef51bc5b315d19d5d2c9e62773d3cf5",
  "accept-encoding": "gzip;q=1.0,deflate;q=0.6,identity;q=0.3",
  "accept": "*/*",
  "x-newrelic-id": "XA4HVVZTGwoGUlFVAgEE",
  "x-newrelic-transaction": "PxQGWFcACwRUVVlXAwAHUAZXFB8EBw8RVU4aUwEJVwEEAQhZBFUCUlJUUUNKQV1VCQcCUgZQFTs=",
  "host": "pingback-pw-doanhkaka.c9users.io",
  "content-length": "1866",
  "x-forwarded-proto": "https",
  "x-forwarded-port": "443",
  "x-region": "usw",
  "x-forwarded-for": "54.175.255.204",
  "Connection": "keep-alive"
}
```

IV. Webhook security: 

- Trong document nó ghi đơn giản thế này thôi:

`Every Coinbase Commerce webhook request includes an X-CC-Webhook-Signature header. This header contains the SHA256 HMAC signature of the raw request payload, computed using your webhook shared secret as the key. You can obtain your shared webhook secret from your settings page. Always make sure that you verify the webhook signature before acting on it inside your system.`

- Nhưng Mình phải mò mất mấy tiếng đồng hồ mới tính ra được cái Signature của nó T_T, sau khi mò ra thì thấy mình gà vãi :p.
- Các bạn hãy dùng đống json bên trên kết hợp với `secret key` ở trong [settings](https://commerce.coinbase.com/dashboard/settings) để nhào nặn ra được `signature`.
- Nhớ cài đặt Webhook subscriptions trước khi test để Coinbase bắn thông tin tới URL đó với yêu cầu domain phải là `https` (Mình sử dụng hàng của `c9.io`).

V. Check hàng nào:

- Test được hàng luôn [ở đây](https://www.freeformatter.com/hmac-generator.html), nhớ chọn SHA256 nhóe.

- Code php thì thế này:
```php
	//Coinbase gửi thông tin signature qua header
	$signature  = $this->getRequest()->getHeader('x-cc-webhook-signature');
	//Lấy raw request
	$rawRequest = file_get_contents('php://input');
	//Tính toán signature
	$calculatedSignature = hash_hmac('sha256', $rawRequest, $secretKey);
	//Check hàng
	var_dump($signature == $calculatedSignature);
```


Ngắn gọn thế thoai, cảm ơn các bạn đã xem !