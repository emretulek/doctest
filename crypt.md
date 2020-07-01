# Crypt

Encrypt, Decrypt ve Has işlemleri için kullanılır. Aynı namepsace altında iki farklı sınıf mevcuttur.

## Crypt

Şifreleme ve şifre çözme methodları içerir, key olarak config/app.php içindeki `app_key, encrypt_algo` kullanılır.

### Methodlar

- Crypt::encrypt($string)
- Crypt::decrypt($encodedString)
- Crypt::encryptSerial($data)
- Crypt::decryptSerial($data)



### encrypt

Belirtilen algoritma ve key ile veriyi şifreler. Çıktı base64_encode ile çıktılanır. Hata oluşursa bir Exception fırlatır.

```php
$encrypted = Crypt::encrypt("Bu şifrelenecek veridir"); //encrypted base64 data
```



### decrypt

encrypt ile şifrelenen verinin şifresini çözerek string olarak çıktılar. Hata oluşursa bir Exception fırlatır.

```php
$decrypted = Crypt::decrypt($encrypted); //decrypted data
```

### encryptSerial

encrypt ile aynı olup array ve object için kullanılabilir, veriyi şifrelemeden önce serialize eder.

### decryptSerial

decrypt ile aynı olup şifresi çözülen veriyi unserialize eder.



## Hash

Hash alma işlemleri için kullanılır. config/app.php içindeki `app_key, hash_algo, password_hash` kullanılır.

### Methodlar

- Hash::make($string)
- Hash::makeWithKey($string)
- Hash::password($password)
- Hash::passwordCheck($password, $hashedPassword)
- Hash::passwordRehash($password, $hashedPassword)



### make

Girilen verinin  ayarlarda belirtilen yöntem ile hash değerini hesaplar.

```php
Hash::make($text); //return hash
```



### makeWithKey

Girilen veriyi ayarlarda belirtilen `app_key` kullanılarak hash değerini hesaplar.

```php
Hash::makeWithKey($text); //return hash
```



### password

config/app.php dosyasında `password_hash` değeri true atanmışsa, php password_hash fonksiyonu kullanılır, `password_hash` false atanmışsa `hash_algo` ile belirtilen yöntem kullanılarak şifreler korunur. "md5", "sha256", "haval160,4" ve benzerleri yöntemler kullanılabilir.

```php
Hash::password($password) 
```



### passwordCheck

Birinci parametre password ikinci parametre daha önce hash değeri alınmış password alır. Eğer şifrelere aynı hash değerine sahipse true, değilse false döndürür.

```php
if(Hash::passwordCheck("password", $hashedPassword)){
	//Şifreler aynı hash değerine sahip
}
```



### passwordRehash

php password_needs_rehash fonksiyonu kullanılarak yeniden hash oluşturma ihtiyacı olup olmadığını kontrol eder. İhtiyaç varsa yeniden hash oluşturup döndürür, ihtiyaç yoksa false döndürür. passwordCheck gibi şifrelerin aynı olup olmadığını da kontrol eder.

```php
if($newHash = Hash::passwordRehash("password", $hashedPassword)){
	//update hash
}
```

