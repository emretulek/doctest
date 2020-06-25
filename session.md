# Session

Session oluşturma, okuma ve silme işlemleri.

### Methods



- Session::start($lifetime = 0, $path = '/', $domain = null, $secure = false, $httponly = true)
- Session::set($key, $vaue)
- Session::get($key)
- Session::remove($key)
- Session::destroy()
- Session::tempSet(string $key, $value, int $lifecycle = 1)
- Session::tempGet($key);
- Session::tempClear();



### start

Oturumun başlatılması için kullanılır. Oturum süresi ve diğer değişkenleri belirlenebilir.

```php
Session::start(60 * 60 * 2); // oturum 2 saat süre için başlatılır.
```



### set

Session oluşurma.

```php
Session::set($key, $value);

Session::set("sessionName", "value");

Session::set("settings.setting_1", "value"); //array
```



### get

Session verilerine ulaşma

```php
Session::get("settings");
Session::get("settings.setting_1");
```



### remove

Session verilerinin silinmesi

```php
Session::remove("settings.setting_1");//remove setting_1
Session::remove("settings");//remove settings
```



### destroy

Tüm session verileri silinerek oturum kapatılır.

```php
Session::destroy();
```



### tempSet

Belirli bir yaşam döngüsüne ait oturum verileri oluşturur. Ön tanımlı olarak sadece 1 sayfadan diğerine aktarılır. İkinci bir sayfa geçişinde silinmiş olur.

```php
//bir sonraki sayfaya aktarıldıktan sonra silinir
Session::tempSet("sessionName", "sessionValue"); 

//Bir sonraki sayfaya akatırlmaz sadece sayfanın çalışması sonlanana kadar hayatta kalır
Session::tempSet("sessionName", "sessionValue", 0); 

//İki sayfa süresince aktarılır ve silinir.
Session::tempSet("sessionName", "sessionValue", 2); 
```



### tempGet

tempSet tarafından oluşturulan oturum bilgilerine erişir.

```php
Session::tempGet("sessionName"); 
```



### tempClear

tempSet tarafından oluşturulan tüm oturum verilerini temizler.

```php
Session::tempClear();
```

