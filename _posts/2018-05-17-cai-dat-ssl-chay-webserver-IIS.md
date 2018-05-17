---
layout: post
title:  "Hướng dẫn cài đặt SSL chạy webserver IIS"
date:   2018-05-17 08:25:23 +0700
categories: PHP
---

I. Giới thiệu:
- Trước khi cài đặt SSL chạy webserver IIS trên Windows, bạn cần đăng kí chứng chỉ SSL tại các tổ chức chứng thực CA.

- Thông thường khi đăng ký chứng chỉ SSL, bạn sẽ nhận được các file như: domain.key, domain.crt… Tuy nhiên trên Windows sẽ không thể add các chứng thực trên vào vì không hỗ trợ các định dạng trên. Vì vậy cần convert qua định dạng windows hỗ trợ là PFX. Ta có thể sử dụng openssl để convert các định dạng trên sang PFX.
- Để tạo ra file pfx trên windows cần cài đặt openssl trên window để tạo.
- Truy cập link `http://slproweb.com/products/Win32OpenSSL.html` để tải các ứng dụng sau: `Visual C++ 2008 Redistributables` và `Win64 OpenSSL v0.9.8ze` sau đó tiến hành cài đặt bình thường.
- Sau khi đã cài đặt các phần mềm trên, mặc định folder openssl sẽ nằm `C:\OpenSSL\bin\openssl.exe` 
- Chạy lệnh `pkcs12 –export –out domain.pfx –inkey domain.key –in domain.crt`
- Điền password để lát add vào IIS 

![Cài đặt SSL](/assets/images/posts/2018-05-17-cai-dat-ssl-chay-webserver-IIS-1.png "Cài đặt SSL")

![Cài đặt SSL](/assets/images/posts/2018-05-17-cai-dat-ssl-chay-webserver-IIS-2.png "Cài đặt SSL")

![Cài đặt SSL](/assets/images/posts/2018-05-17-cai-dat-ssl-chay-webserver-IIS-3.png "Cài đặt SSL")

![Cài đặt SSL](/assets/images/posts/2018-05-17-cai-dat-ssl-chay-webserver-IIS-4.png "Cài đặt SSL")

II. Cài đặt IIS Webserver:

+ Mở Server Manager từ Administrative tools. Trên cửa sổ Server Manager, chọn Role sau đó chọn Add Roles để cài đặt Web Server (IIS) role.
+ Trên cửa sổ Before You Begin chọn Next để tiếp tục
+ Trên cửa sổ Select Server Roles, đánh dấu chọn vào mục Web Server (IIS)
+ Trên hộp thoại Add Roles Wizard chọn Add Required Features để bổ sung các dịch vụ đi kèm
+ Trên cửa sổ Select Role Services tick chọn IIS và chọn Next để tiếp tục
+ Trên cửa sổ Confirm Installation Selections chọn Install để tiến hành cài đặt.

III. Cấu hình Web Server chạy http và https:

+ Sử dụng notepad tạo một file index.html với nội dung Welcome To Demo được đặt trong thư mục web nằm trong ổ C. Đây là file nội dung của trang web
+ Mở Internet Information Service (IIS) Manager từ Administrative Tools. Mở CODE23H, sau đó chuột phải vào Site chọn Add Web Site…
+ Trên hộp thoại Add Web Site nhập Demo vào ô Site name trong ô Physical Path trỏ đường dẫn đến D:\www\code23.com, trong ô Type chọn http trong ô Host name nhập code23h.com, sau đó chọn OK

Add SSL certificate cho Web Server:
+ Mở Internet Information Services (IIS) Manage từ Administrative Tools. Chọn CODE23H, Trên cửa sổ giữa chọn Server Certificates. Trong phần Action, chọn Import.

Cấu hình Website với SSL:
+ Trên màn hình Internet Information Services (IIS) Manager, chọn Site, sau đó chuột phải vào trang web code23h.com và chọn Edit Binding
+ Tại Type chọn https, Ip address chọn IP của máy Web server CODE23H SSL Certificate chọn code23h.com đã tạo ở bước trên, click OK.

+  Vào máy client hoặc bất kỳ máy nào trong mạng, vào địa chỉ https://code23h.com ta thấy truy cập thành công.

+ Nhưng hiện tại chưa trả tiền nên nó đang báo đỏ =))


Cảm ơn các bạn đã theo dõi ~