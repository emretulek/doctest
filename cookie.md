# Cookie

Cookie oluşturma, okuma ve silme işlemleri.

### Methods

Cookie::set($key, $vaue, $lifetime = 24, $path = '/', $domain = null, $secure = false, $http_only = true, $same_site = 'strict')

Cookie::get($key)

Cookie::remove($key)

Cookie::destroy()



### set

Cookie oluşurma.

```php
Cookie::set($key, $value, $lifetime = 24, $path = '/', $domain = null, $secure = false, $http_only = true, $same_site = 'strict');
/**
* int|string $lifetime hours
*/
Cookie::set("cookieName", "value", "3m"); // 3 ay süreli cookie

Cookie::set("settings.setting_1", "value"); //array
```



### get

Cookie verilerine ulaşma

```php
Cookie::get("settings");
Cookie::get("settings.setting_1");
```



### remove

Cookieleri silme işlemi

```php
Cookie::remove("settings.setting_1");//remove setting_1
Cookie::remove("settings");//remove settings
```



### destroy

Domain altındaki tüm cookieleri silme.

```php
Cookie::destroy();
```

