---
layout: post
title:  "API Authencation trong Laravel sử dụng JWT"
date:   2018-05-16 14:22:22 +0700
categories: PHP Laravel
---

I. Hoàn cảnh
- Hôm trước mềnh có đi phỏng vấn ở 1 cty và có bài test yêu cầu như sau:

![Bài test](/assets/images/posts/api-authentication-laravel-1.png "Bài test")

- Nhìn có vẻ đơn giản nhưng động vào mới thấy căng đét đẹt o.O
- Bản chất của việc này mình hiểu là tạo ra 1 đoạn mã loằng ngoằng (`token`) rồi dùng mã đó có thể xác thực được thông tin người dùng mà ko phải dùng `username + password` cho các lần request sau.
- Bất cứ khi nào người dùng muốn truy cập vào route hoặc tài nguyên cần có quyền, họ phải gửi lại JWT qua header hoặc phang thẳng lên URL cũng được, tùy yêu cầu của server side.
- Mềnh đã sử dụng Laravel (5.5.*) và plugin `jwt` (JSON Web Token) để pass qua bài test trên.

II. Quẩy thôi
1. Cài cắm

- Sau khi cài đặt xong Laravel các bạn cần cài thêm package JWT:

```php
	composer require tymon/jwt-auth
```

- Sửa file `config/app.php`
```php
	'providers' => [
		....
		Tymon\JWTAuth\Providers\JWTAuthServiceProvider::class,
	],
	'aliases' => [
		....
		'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
	],
```
- Publish file config JWT, khi không gặp lỗi bạn sẽ thấy file `config/jwt.php` xuất hiện trên bản đồ. Chạy lệnh dưới nhá:

```php
	php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"
```

- Sau khi cài cắm xong bạn phải tạo 1 secret key với lệnh dưới:
```php
	php artisan jwt:generate
```

2. Cập nhật route `routes/api.php`

- Khai báo các đường dẫn để user gọi đến:
```php
	Route::post('v1/login', 'UserController@login');

	Route::group(['prefix' => 'v1', 'middleware' => 'user.auth'], function() {	
		Route::get('me', 'UserController@info');
		Route::put('me', 'UserController@update');
		Route::get('logout', 'UserController@logout');
	});
```

- Tạo middleware `user.auth` để check xem lúc gọi API user đã cung cấp `token` chựa:
```php
	php artisan make:middleware VerifyUserToken
```
- Function handle():
```php
public function handle($request, Closure $next)
{
    $token = $request->header('token_access');
    try {
        $user = JWTAuth::toUser($token);
    } catch (JWTException $ex) {            
        if($ex instanceof TokenExpiredException) {                
            $exceptions['error'] = self::ERROR_TOKEN_IS_EXPIRED;
        } elseif ($ex instanceof TokenInvalidException) {                
            $exceptions['error'] = self::ERROR_TOKEN_IS_INVALID;
        } elseif(empty($token)) {                
            $exceptions['error'] = self::ERROR_TOKEN_IS_REQUIRED;
        } else {
            //TODO: Don't response all error to client.
            //$exceptions['error'] = self::ERROR_TOKEN_GENERAL_ERROR;
            $exceptions['error'] = $ex->getMessage();                
        }
        Log::error('VerifyUserToken:', ['error' => $ex->getMessage()]);
        return response()->json($exceptions, $ex->getStatusCode());
    }
    return $next($request);
}
```
- Nhớ use thêm đống thư viện này:
```php
	use JWTAuth;
	use \Tymon\JWTAuth\Exceptions\JWTException;
	use Log; 
```

- Khai báo Middleware mới ở file `app/Http/Kernel.php`
```php
	protected $routeMiddleware = [
		....,
		'user.auth' => \App\Http\Middleware\VerifyUserToken::class,
	];
```

- Cũng ra rì phết rồi, giờ mà user gọi đến mấy cái route như `/me`, `/logout` mà ko có `token` là bị lỗi ngay (lỗi là cái chắc vì đã khởi tạo `UserController` đâu :P).

- Tạo ngay `UserController` với câu lệnh dưới:
```php
	php artisan make:controller UserController
```

- Quẩy thêm 4 functions tương ứng với route:

```php
	/**
	* User login
	*/
    public function login() {
    	$userData = request(['email', 'password']);
    	
        try {
        	$token = JWTAuth::attempt($userData);
            if (empty($token)) {
            	$result = ['message' => self::ERROR_LOGIN_INVALID_EMAIL_OR_PASSWORD];
            	$this->log('UserLogin', $userData, $result);
                return response()->json($result, 422);
            }
        } catch (JWTAuthException $ex) {
        	$this->log('UserLoginException', $userData, $ex->getMessage());
            return response()->json(['message' => $ex->getMessage()], 500);
        }
        return response()->json(['token_access' => $token]);
    }
    /**
	* Update user information
	*/
    public function update(Request $request) {    	
    	$isValid = $this->validateUser($request);
    	//TODO: Unset token_access in $request before storage to log.
    	$this->log('UserUpdate', $request->all(), $isValid);
    	if ($isValid) {
    		$user = JWTAuth::toUser(request('token_access'));
    		$user->fill($request->all());
        	$user->save();
    		return response()->json(['user' => $user]);
    	}
    	$errors = $this->getErrors();
    	return response()->json(['message' => $errors], 500);
    }

     /**
	* Get user information
	*/
    public function info(Request $request) {
    	$token = $request->header('token_access');
    	$userInfo = JWTAuth::toUser($token);
    	$this->log('UserInfo', [], $userInfo);
    	return response()->json(['user' => $userInfo]);
    }
    /**
	* User logout
	*/
    public function logout(Request $request) {
    	$token = $request->header('token_access');
    	JWTAuth::invalidate($token);
    	$result = ['message' => self::USER_LOGOUT_SUCCESSFULLY];
    	$this->log('UserLogout', $result);
    	return response()->json($result);
    }
```

III. Check hàng:
- Code cơ bản là như thế, chi tiết thì các bạn có thể xem [ở đây](https://github.com/doanhkaka/user-management/){:target="_blank"}.
- Có thể sử dụng `Postman` hoặc chạy `Unit test` nhóe.

Cảm ơn các bạn đã theo dõi !


