# Request

Gelen http isteklerine daha hızlı ve daha güvenli erişim sağlar.

- [path](#path)
- [requestUri](#requestUri)
- [matchUri](#matchUri)
- [currentUrl](#currentUrl)
- [baseUrl](#baseUrl)
- [segments](#segments)
- [request](#request)
- [get](#get)
- [post](#post)
- [files](#files)
- [raw](#raw)
- [method](#method)
- [isAjax](#isAjax)
- [scheme](#scheme)
- [host](#host)
- [local](#local)
- [userAgent](#userAgent)
- [referer](#referer)
- [ip](#ip)
- [forwardedIp](#forwardedIp)
- [server](#server)



### path

İstek yapılan url içinden path değerini döndürür. 

```php
www.site.com/user/comments?page=2

Request::path(); 
# user/comments
```



### requestUri

path methodu ile aynı değer döner, uygulama alt dizinde çalışıyorsa, requestUri alt dizin öğesini path değerinden düşürecektir. 

Path congif/app.php ayar dosyasında belirtilmiş olmalıdır. Uygulama test dizininde çalışıyor varsayılırsa;

```php
www.site.com/test/user/comments?page=2

Request::path(); 
# test/user/comments

Request::requestUri(); 
# user/comments
```



### matchUri

Girilen değer mevcut urlde geçiyorsa true aksi halde false döndürür;

```php
Request::matchUri("user");
//veya
Request::matchUri("user.*");
```



### currentUrl

Mevcut url adresini döndürür.

```php
Request::currentUrl();
# http://www.site.com/test/user/comments?page=2
```



### baseUrl

Uygulamanın çalıştığı url site adresini döndürür, alt dizinde çalışıyorsa alt dizin ile birlikte.

```php
Request::baseUrl();
```



### segments

Url "/" her slaş için parçalanır, indis girilirse değerini, girilmezse tüm parçaları dizi olarak döndürür.

```php
# http://www.site.com/user/comments
Request::segments(1);  # comments
Request::segments(); # ['user','comments']
```



### request

$_REQUEST ile aynıdır. İstenen dizi anahtarı bulunamazsa false döndürür.

```php
$_REQUEST['key1']['key2'];
Request::request("key1.key2");
```



### get

$_GET ile aynıdır. Farklı olarak strip_tags ve trim kullanılarak temizlenmiştir.

```php
$_GET['key1']['key2'];
Request::get("key1.key2"); //baştaki, sondaki boşluklar ve html etiketleri temizlendi.
```



### post

$_POST ile aynıdır. Farklı olarak trim ile baş ve sondaki boşluklar temizlenir.

```php
_POST ['key1']['key2'];
Request::post("key1.key2"); //baştaki ve sondaki boşluklar temizlendi.
```



### files

$_FILES dan farklı olarak dizi yapısını yeniden düzenler.

$_FILES

```php
Array
(
    [name] => Array
        (
            [0] => foo.txt
            [1] => bar.txt
        )

    [type] => Array
        (
            [0] => text/plain
            [1] => text/plain
        )

    [tmp_name] => Array
        (
            [0] => /tmp/phpYzdqkD
            [1] => /tmp/phpeEwEWG
        )

    [error] => Array
        (
            [0] => 0
            [1] => 0
        )

    [size] => Array
        (
            [0] => 123
            [1] => 456
        )
)
```

Request::files()

```php
Array
(
    [0] => Array
        (
            [name] => foo.txt
            [type] => text/plain
            [tmp_name] => /tmp/phpYzdqkD
            [error] => 0
            [size] => 123
        )

    [1] => Array
        (
            [name] => bar.txt
            [type] => text/plain
            [tmp_name] => /tmp/phpeEwEWG
            [error] => 0
            [size] => 456
        )

)
```



### raw

Json tipinde post edilen verilere erişmek için kullanılabilir. **php://input** değerini döndürür.

```php
Request::raw();

{ "name":"John", "age":30, "car":null } 
```



### method

Parametre girilmezse yapılan istek türünü döndürür. $_SERVER['REQUEST_METHOD'] değerini döndürür. İstek türü girilen parametre ile aynı ise true aksi halde false döndürür.

```php
Request::method(); // GET, POST, DELETE vb.

if(Request::method('POST')){
	//post ise yapılacak işlemler
}
```



### isAjax

İstek header bilgisinde HTTP_X_REQUESTED_WITH ve HTTP_X_REQUESTED_WITH varsa true yoksa false döndürür.

```php
if(Request::isAjax()){
	//ajax isteği
}

if(Request::isAjax('POST')){
	//ajax post isteği
}
```



### scheme

Sunucu protokolünü döndürür. http veya https

```php
Request::scheme(); //http veya https
```



### host

server name değerini döndürür.

```
Request::host() == $_SERVER['SERVER_NAME'];
```



### local

Browser dilini "tr-TR" 5 karakter olarak döndürür. parametre true ayarlanırsa 2 hane "tr" olarak döndürür.

```php
Request::local(); // en-GB

Request::local(true); // en
```



### userAgent

Browser user agent bilgisini döndürür.

```php
Request::userAgent(); //Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:77.0) Gecko/20100101 Firefox/77.0
```



### referer

Browser referer bilgisini döndürür.

```php
Request::referer(); //www.site.com
```



### ip

$_SERVER['REMOTE_ADDR'] değerini  döndürür.

```php
Request::ip();
```



### forwardedIp

Proxy ip adresi belirtilmişse HTTP_CLIENT_IP, HTTP_X_FORWARDED_FOR bilgilerinden birini döndürür. Eğer ikiside yoksa $_SERVER['REMOTE_ADDR'] bilgisi döner.

```php
Request::forwardedIp();
```



### server

$_SERVER değişkenlerinin tamamına erişilebilir.

```php
Request::server("user-agent");
Request::server("referer");
Request::server("accept-language");
...
```

