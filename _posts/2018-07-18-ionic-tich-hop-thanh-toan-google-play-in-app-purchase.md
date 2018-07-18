---
layout: post
title:  "Tích hợp thanh toán qua Google Play - Ionic App"
date:   2018-07-18 11:18:18 +0700
categories: Mobile Ionic
---

Hi các bạn,

- Sau mấy vụ đánh bạc trực tuyến hàng ngàn tỉ đồng thì Nhà nước đã cấm các cổng nạp tiền sử dụng thẻ cào điện thoại (Viettel, Mobifone, Vinaphone, ...)
- Dẫn tới các dịch vụ số và cơ số máy chủ game lậu oẳng sml, ko biết ngày ngẩng đầu lên.
- Mình đang mò mẫm code app dùng Ionic thấy có cái Tích hợp thanh toán qua Google Play khá tiện lợi, đặc biệt có thể dùng tiền ở `Tài Khoản Chính` của sim để thanh toán.
- Google Play cũng có 1 vài yêu cầu: Sim phải có thời gian sử dụng > 90 ngày và có ít nhất 10k trong tài khoản.

![Thanh toán qua Google Play](/assets/images/posts/ionic-in-app-purchase-0.png "Thanh toán qua Google Play")

I. Plugin In-App-Purchase
- Cứ document mà táng thôi, mềnh đang sử dụng thằng này:

[Ionic Plugin - In App Purchase](https://ionicframework.com/docs/native/in-app-purchase/){:target="_blank"}.

- Cài cắm Plugin với các lệnh:
```php
$ ionic cordova plugin add cordova-plugin-inapppurchase
$ npm install --save @ionic-native/in-app-purchase
```

- Thêm key: `play_store_key` vào file `manifest.json`
![Key Google Play](/assets/images/posts/ionic-in-app-purchase-2.png "Key Google Play")

- Key store lấy ở: 
![Key Google Play](/assets/images/posts/ionic-in-app-purchase-3.png "Key Google Play")

- Theo như hướng dẫn trong sách giáo khoa thì các bác chỉ việc import plugin vào rồi gọi ra sử dụng:
- Khai báo: 

```php
import { InAppPurchase } from '@ionic-native/in-app-purchase';

constructor(private iap: InAppPurchase) { }
```

- Load danh sách sản phẩm đã khai báo ở Google Play: 


```php
public loadProducts() {
       this.iap
       .getProducts(['cash500', 'cash1000', 'cash2000', 'cash3000', 'cash4000', 'cash5000', 'cash6000', 'cash8000'])
       .then(function (productx) {
           console.log(productx);
        // alert("Products: " + JSON.stringify(productx));
    })
       .catch(function (err) {
        // alert("Error: " + JSON.stringify(err));
    });
}

```

![Danh sách sản phẩm Google Play](/assets/images/posts/ionic-in-app-purchase-1.png "Danh sách sản phẩm Google Play")

- Mỗi lần thanh toán tối đa là 500k VNĐ (Đã bao gồm 10% VAT), do vậy sản phẩm các bác nên để giá tối đa là 400k thôi nhé.

- Mua sản phẩm đã được load bên trên:

```php
this.iap.buy(prdId)
.then(function (data) {
    var inapp = new InAppPurchase();
    inapp.consume(data.productType, data.receipt, data.signature)
    .then(function(res) {
       //alert("Response: " + JSON.stringify(res));
       // alert("chargeResponse: " + JSON.stringify(chargeResponse));
   }).catch(function (err) {
        //alert("Purchase error cmnr: " + JSON.stringify(err, Object.getOwnPropertyNames(err)));
        
    });
})
.catch(function (err) {
    //alert("Error cmnr: " + JSON.stringify(err, Object.getOwnPropertyNames(err)));
    console.log(err);
});

```

- Trong cái `data` trả về kia sẽ có thông tin của order, signature, transaction_id, ... post cái đống đấy lên server của bạn để validate rồi + point hoặc làm gì đấy:

```php
const BILLING_KEY = 'MIIBIjANBgkqhkiG9.....';

public function point() {
    $raw = file_get_contents("php://input");

    try {
        parse_str($raw, $data); 
        $this->log(json_encode($data));

        $receipt       = @$data['receipt'];    // receipt data from Corona (event.transaction.receipt)
        $signature     = @$data['signature'];  // signature from Corona (event.transaction.signature)

        $googlePlayKey   = self::BILLING_KEY;
        $publicKey       = "-----BEGIN PUBLIC KEY-----\n" . chunk_split($googlePlayKey, 64, "\n") . '-----END PUBLIC KEY-----';

        $key = openssl_get_publickey( $publicKey );
        
        $result = (1 == openssl_verify($receipt, base64_decode($signature), $key, OPENSSL_ALGO_SHA1));

        if ($result) {
            //Xử lý Logic của bạn

            $this->log('User update point :', json_encode([$token, $user]));
        }
    } catch(Exception $e) {
        $this->log("Error update point: " . $e->getMessage());
    }
}
```

Xem thông tin Order tại đây:

![Order Management](/assets/images/posts/ionic-in-app-purchase-4.png "Order Management")

- Trong quá trình tích hợp plugin này sẽ có nhiều lỗi :( Mình mò mẫm mấy trang Google mãi mới chạy được:
	- Lỗi Plugin khi Build file APK
	- Lỗi không thể mua sản phẩm lần thứ 2
	- Các bạn phải release app mới test được
	- Không thể dùng tài khoản Developer để test, mình dùng Xiaomi có chức năng tạo 2 không gian nên đã login tài khoản khác để test.
	- Có thể tạo Promotion Code để mua, đỡ mất tiền thật.

Chỗ nào không qua được thì liên hệ mềnh, mềnh support nhóe :P

Cảm ơn các bạn đã xem 😀
