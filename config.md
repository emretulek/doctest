# Config

Temel uygulama ayarlarına ulaşabilir, çalışma anında ayarları değiştirebilir, silebilir veya yenisini oluşturabilirsiniz.

Namespace Core\Config

### Methodlar

- [Config::get](#get)(string $configName):mixed
- [Config::set](#set)(string $configName, $value):void
- [Config::remove](#remove)(string $configName):bool



### get

config/app.php dosyasından charset değeri için

```php
Config::get("app.charset"); //karakter seti
```



### set

Ayar tanımlı ise günceller, değilse yeni oluşturur. Oluşturulan yada değiştirilen ayarların kalıcı değil sadece çalışma anında geçerli olduğu unutulmamalı.

```php
Config::set("app.charset", "ISO-8859-9"); // karakter seti güncellenir

Config::set("settings.siteTitle", "Site Name"); //yeni bir tanım eklenir
Config::get("settings.siteTitle"); // Site Name
```



### remove

Tanımlı olan ayarı siler, ayar var ve silinmişse true, bulunamamışsa false döner.

```php
Config::remove("settings"); 
```

