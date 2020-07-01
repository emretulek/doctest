# Language

Uygulamaya session üzerinden çoklu dil desteği kazandırır. Default dil `config/app.php` dosyasından **language** ile belirlenir. Dil dosyaları lang/ dizini altında tutulur.

config/app.php

```php
'language' => [
    'key' => 'tr',
    'name' => 'Türkçe',
    'local' => 'TR-tr'
]
```

Dil dosyaları dil anahtarı isminde bir klasörde tutulur. Yukarıda ki ayara göre `lang/tr/` olacaktır. Örnek dil dosyası;

```php
<?php
return [
    'home' => 'Anasayfa',
    'contact' => 'İletişim',
    'about' => 'Hakkımızıda',
];
```



## Methodlar

- Language::init()
- Language::useUrl()
- Language::setDefault($key)
- Language::getDefault()
- Language::setActive($key)
- Language::getActive()
- Language::loadFile($key, $file_path)
- Language::translate($key, ...$args)
- Language::newTranslate($key, $value)
- Language::exists($key)
- Language::add($key, $name, $local)
- Language::remove($key)



### init

config/app.php ayar dosyasında tanımlı dil default dil olarak belirlenir ve aktif dil yapılır.

```php
Language::init()
```



### useUrl

Aktif dil url adresinden ilk segmente bakılarak ayarlanır, aktif dil ve default dil aynı ise url adresinde gösterilmez.

```php
Language::useUrl()
```



### setDefault

Belirtilen dil anahtarı default dil olarak belirlenir. Dil dosyası veya çeviri bulanamazsa default dil çevirileri kullanılır.

```php
Language::setDefault("tr")
```



### getDefault

Default dil anahtar, isim ve local değerini içeren bir obje döndürür.

```php
Language::getDefault() // object

Language::getDefault()->key
Language::getDefault()->name
Language::getDefault()->local
```



### setActive

Aktif dili belirler.

`Language::setActive("en");`  dil anahtarı daha önce  `Language::add("en", "English", "en-US")` methodu ile eklenmişse aktif dil olarak belirlenir.

```php
Language::setActive("en")
```



### getActive

Aktif dil anahtar, isim ve local değerini içeren bir obje döndürür.

```php
Language::getActive() // object

Language::getActive()->key
Language::getActive()->name
Language::getActive()->local
```



### loadFile

Farklı bir dizinden dil dosyası eklenmesini sağlar. Birinci parametre eklenecek dik anahtarı, ikinci parametre dosya yolu.

```php
Language::loadFile("en", "external/language/filename.php")
```



### translate

Dosya adından başlayarak indexi girilen çeviriyi döndürür. Eğer çeviri yoksa default dilde aranır, yine bulunamazsa aynen girilen değeri döndürür.

/lang/tr/messages.php

```php
return [
	'hello' => "merhaba"
    'hello_dear' => "Merhaba sayın %s"
];
```

```php
Language::translate("messages.hello"); //merhaba
Language::translate("messages.hello_dear", "Ahmet"); // Merhaba sayın Ahmet
```



### newTranslate

Dile yeni bir çeviri ekler.

```php
Language::newTranslate("goodbye_dear", "Hoşça kalın sayın %s");

Language::translate("goodbye_dear", "Ahmet")
```



### exists

Dil anahatarı üzerinden dilin var olup olmadığında bakar.

```php
if(Language::exists("tr")){
	//dil daha önce eklenmiş
}
```



### add

Sırasıyla anahtar, isim ve local değerleri girilen yeni bir dil tanımlar.

```php
Language::add('en', 'English', 'en_US');
```



### remove

Anahtarı girilen dili kaldırır.

```php
Language::remove("en");
```



### prefix

Dilin adres satırında gösterilecek formu. Seçili dil default dil ile aynı ise boş dönecektir.

```php
http:://site.com/
Language::prefix() //boş döner
Language::getActive() //default dil döner

http:://site.com/en
Language::prefix() //en döner
Language::getActive() //en döner
```



Service içinde örnek kullanım

```php
<?php

namespace Services;

use Core\Language\Language;
use Core\Services\Services;


class LanguageService extends Services
{
    public function boot()
    {
        //default dil aktif edildi. config/app.php -> language
        Language::init();

        //kullanılabilir dillere yeni dil eklendi
        Language::add('en', 'English', 'en_US');

        //default dil olarak ingilizce belirtildi
        Language::setDefault('en');

        //ingilizce seçili dil olarak işaretlendi
        Language::setActive('en');

        //Seçili dil url adresinden otomatik olarak algılanacak
        Language::useUrl();
    }
}
```



Router için prefix ile dil kullanımına örnek

```php
Router::group(['prefix' => Lang::prefix()], function (){
    Router::get('/', function (){
       echo Lang::getActive()->key;
    });
});

```