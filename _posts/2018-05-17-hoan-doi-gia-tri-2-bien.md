---
layout: post
title:  "Hoán đổi giá trị 2 biến không dùng biến trung gian"
date:   2018-05-17 08:23:23 +0700
categories: PHP
---

Hi các bác,

Cũng là 1 câu hỏi phỏng vấn khá khoai nếu ai không bình tĩnh sẽ dễ bị tâm lý mà buông súng.

Cho 2 biến:
```php
	$a = 6; $b = 9;
```

Hãy hoán đổi giá trị của 2 biến này mà ko cần dùng biến trung gian.

Rất đơn giản.

Chỉ cần cộng trừ vài đường cơ bản là xong, và đây là đáp án:
```php
	$a = 6;
	$b = 9;
	 
	$a = $a - $b;
	$b = $a + $b;
	$a = $b - $a;
	 	 
	print_r($a);
	echo " - ";
	print_r($b);
```

Cảm ơn các bạn đã xem 😀
