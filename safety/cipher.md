Cipher 加解密库
==========

格式保留加密库，加密邮箱和电话号码等敏感数据，加密后依然是合法的邮箱和电话号码(格式保留)。

同时会保留部分字符以供快速预览。

部分细节说明<http://www.yunyin.org/pages/how-to-encrypt-phone-and-email.html>


方法列表
-----------
* [Cipher::encryptEmail($email) 邮箱格式保留加密](#email)
* [Cipher::decryptEmail($email) 邮箱格式保留解密](#email)
* [Cipher::encryptPhone($phone, $salt, $id=false)电话格式保留加密](#phone)
* [Cipher::decryptPhone($phone, $salt, $id=false)电话格式保留解密](#phone)
* [Cipher::encryptPhoneTail($tail) 电话后四位加密](#encryptPhoneTail)


## 配置 {#config}

使用此库之前请务必先配置`secret.product.ini`里的加密key。
加密key是保密和解密的关键，不可泄漏和丢失，生产环境和开发环境分开设置。

```ini
[encrypt]
;加密密钥32位字符
key_email     = '123xxxxx';邮箱加密密钥
key_phone_mid = 'xxxx';手机中间号码加密密钥
key_phone_end = 'xyz';尾号加密密钥

```


## 邮箱加密 {#email}

>```php
>Cipher::encryptEmail(string $email):string
>Cipher::decryptEmail(string $email):string
>```

* 加密后，第一个字符和邮箱域名保持不变以供快速预览。
* 邮箱加密每次加密后的结果一致，可以使用加密后的邮箱反查

Example:
```php
$en = Cipher::encryptEmail('yyf@yunyin.org'); //y***@yunyin.org
Cipher::decryptEmail($en);//yyf@yunyin.org

$en = Cipher::encryptEmail('12345678@qq.com'); //1***@qq.com
Cipher::decryptEmail($en);//12345678@qq.com
```

## 电话号码加密 {#phone}

电话号码的敏感度比邮箱更高，加密和还原方法细节上更为复杂。

>```php
>Cipher::encryptPhone(string $phone,string $salt, int $id = false):string
>Cipher::decryptPhone(string $phone,string $salt, int $id = false):string
>```

* `string $phone`: 用户手机号(>=4位),可以带国际编号(如+86)
* `string $salt`: 混淆字符，为每个号码生成唯一密码本，可以不随机，但是保证每个用户唯一且不变
* `string $id`: 偏移量，可以使用用户id 等不变的数据提高加密强调。

* 只有配置中两个key,以及$salt和$id两个混淆参数，这四个才能还原一个加密后的号码。


号码长度与加密位数对照表,为了方便预览，开头几位予以保留。

| 长度 | 加密位数 | 举例 | 说明|
| :---:| :---:  | :---: | :---: |
| >=11 | 8~10位 (6+4)| 138******** | 通常只有后8位变化|
| 8~10 | 8位 (4+4) | 9******** | 最后8位被加密|
| 4~7  | 4位 | 5**** | 最后4位被加密 |

Example:
```php
$ep = Cipher::encryptPhone('13888888888','salt for user',1); //138*********
Cipher::decryptEmail($ep,'salt for user',1);//13888888888

$ep = Cipher::encryptPhone('+8613888888888','another salt', 233); //+86138*********
Cipher::decryptEmail($ep,'another salt', 233);//+8613888888888
```

## 电话号反向查找 {#findPhone}

用户登录等情况可能需要根据真实手机号查找对应的用户。

### 尾号加密 {#encryptPhoneTail}

手机尾号后4位全局公用相同的密码表，与混淆参数无关

>```php
>Cipher::encryptPhoneTail(string $tail):string
>```

* $tail : 手机后四位尾号


### 反向查找

当拿到手机号，缺少两个混淆值不能直接查找到对应的手机号。
可以通过后四位缩小密码范围。

1. 加密后四位
2. 查找相同后4位的号码
3. 再查找结果中匹配验证手机号

