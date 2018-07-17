---
layout: post
title:  "Hướng dẫn Public Ionic App lên Google Play"
date:   2018-07-17 17:17:17 +0700
categories: Mobile Ionic
---

Hi các bác, Em đã trở lại.

Hôm nay Em sẽ chia sẻ cách upload ứng dụng viết bằng Ionic lên Google Play.
Em chọn Ionic để viết app vì nó dùng AngularJS và cú pháp giống Javascript.

I. Cập nhật thông tin của ứng dụng:
- Các bác mở file `config.xml` ở ngay ngoài cùng ra rồi sửa các thẻ:
	+ widget: `attribute: id` => cái này sẽ là id của bạn khi lên sóng, không thể thay đổi, `version`: cần phải thay đổi mỗi lần tạo release.
	+ name: Tên ứng dụng sẽ hiển thị sau khi cài đặt
	+ description: 
	+ author:

![Thông tin ứng dụng](/assets/images/posts/public-app-ionic-1.png "Thông tin ứng dụng")

II. Gõ lệnh ra sản phẩm:
- Chắc hẳn trong quá trình code các bác đã build thử app lên điện thoại của mềnh và test chán chê rồi nên bỏ qua bước `add platform` nhóe.
- Gõ luôn lệnh: `ionic cordova build android --release` cho máu

![Gõ lệnh ra sản phẩm](/assets/images/posts/public-app-ionic-1.png "Gõ lệnh ra sản phẩm")

- File `app-release-unsigned.apk` đã xuất hiện ở `platforms\android\app\build\outputs\apk\release` nhưng chưa đem đi bán ngay được đâu nhóe.

