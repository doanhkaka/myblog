---
layout: post
title:  "Hướng dẫn Public Ionic App lên Google Play"
date:   2018-07-17 17:17:17 +0700
categories: Mobile Ionic
---

Hi các bạn, Em đã trở lại.

Hôm nay Em sẽ chia sẻ cách upload ứng dụng viết bằng Ionic lên Google Play.
Em chọn Ionic để viết app vì nó dùng AngularJS và cú pháp giống Javascript.

I. Cập nhật thông tin của ứng dụng:
- Các bạn mở file `config.xml` ở ngay ngoài cùng ra rồi sửa các thẻ:
	+ widget: `attribute: id` => cái này sẽ là id của các bạn khi lên sóng, không thể thay đổi, `version`: cần phải thay đổi mỗi lần tạo release.
	+ name: Tên ứng dụng sẽ hiển thị sau khi cài đặt
	+ description: 
	+ author:

![Thông tin ứng dụng](/assets/images/posts/public-app-ionic-1.png "Thông tin ứng dụng")

II. Gõ lệnh ra sản phẩm:
- Chắc hẳn trong quá trình code các bạn đã build thử app lên điện thoại của mềnh và test chán chê rồi nên bỏ qua bước `add platform` nhóe.
- Gõ luôn lệnh: `ionic cordova build android --release` cho máu

![Gõ lệnh ra sản phẩm](/assets/images/posts/public-app-ionic-2.png "Gõ lệnh ra sản phẩm")

- File `app-release-unsigned.apk` đã xuất hiện ở `platforms\android\app\build\outputs\apk\release` nhưng chưa đem đi bán ngay được đâu nhóe.

- Các bạn cần chạy thêm 1 số lệnh để mã hóa và đóng gói file apk kia lại:

+ Lệnh này để sinh ra cái keystore, chỉ cần chạy 1 lần đầu tiên, phải nhớ 2 cái password các bạn đã nhập nhóe:
```php
keytool -genkey -v -keystore my-release-key.keystore -alias dml -keyalg RSA -keysize 2048 -validity 10000
```

+ Lệnh này để phang cái key bên trên kia vào file apk (Kiểu như là đóng dấu :D):
```php
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore app-release-unsigned.apk dml
```

+ Lệnh này để zip file apk lại theo yêu cầu của Google Play:
```php
zipalign -v 4 app-release-unsigned.apk dml.apk
```

- File apk xịn đã ra đời, upload lên Google Play ngay thôi.

III. Tài khoản Google Developer:
- Các bạn phải mất 25$ cho việc đăng ký tài khoản Google Developer <Cứ mua đê, đừng ngại ngùng>
- Cng có thể nhờ người có tài khoản Developer rồi share cho để upload app.

- Khi đã có tài khoản Developer, ấn nút `CREATE APPLICATION`:

![Upload app to Google Play](/assets/images/posts/public-app-ionic-3.png "Upload app to Google Play")

- Điền tên app và chọn ngôn ngữ mặc định:

![Enter app title and default language](/assets/images/posts/public-app-ionic-4.png "Enter app title and default language")

- Ứng dụng của bạn đã được tạo nhưng đang ở trạng thái `Draft`, cần phải điền đầy đủ thông tin (di chuột vào các dấu check sẽ có yêu cầu):

![Enter app information](/assets/images/posts/public-app-ionic-5.png "Enter app information")

- Sau khi điền đầy đủ thông tin và upload file .apk lên, Google sẽ review app của bạn và sẽ có thông báo khi app go live
- Chỉnh sửa thông tin của app ở đây nhóe:

![Enter app information](/assets/images/posts/public-app-ionic-6.png "Enter app information")

- Mỗi khi có bản cập nhật mới cho app, các bạn vào đây để upload file .apk và chờ review <Khoảng 30p - 2h>
- Nhớ đọc kĩ các điều khoản của Google Play, chẳng may bị suppend app thì đắng lắm :(

![App release](/assets/images/posts/public-app-ionic-7.png "App release")

Cơ bản là thế, còn nhiều vấn đề lắm, các bạn cứ mò đi nhóe.

Xin cảm ơn !

