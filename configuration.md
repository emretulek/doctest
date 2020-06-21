# Configuration

/config dizini altındaki .php uzantılı dosyalar otomatik olarak configürasyon dosyası olarak tanımlanır. Dosya bir dizi döndürmelidir.

```php
<?php
return [];
```

### app.php  Temel Ayarların yapıldığı dosya

```php
return  array(
    /**
     * Site base path default "/"
     */
    'path' => '/',

    /**
     * {app_key} projeye özgü anahtar şifreleme türüne göre ayarlanmalı mevcut key 256bit 32 karakter
     */
    'app_key' => 'eThWmZq4t6w9z$C&F)J@NcRfUjXn2r5u',

    /**
     * {debug} -1 tüm hatalar dosyaya yazılır ekrana ve consola çıktı verilmez.
     * {debug} 0 Fatal error tüm çıktıyı engeller. Ekrana ve consola çıktı verilmez. error_log notice hariç hataları kaydeder. (Tavsiye edilen)
     * {debug} 1 Notice hariç tüm hatalar javascript konsoluna ve error_loga yazılır. Gizli debug.
     * {debug} 2 tüm hatalar ekrana basılır ve error_loga yazılır. Geliştirici modu.
     * {debug} 3 debug_back_trace aktif olur. Geliştirici modu.
     */
    'debug' => 2,

    /**
     * {sql_driver} database.php ayarlarında kullanılacak driver
     */
    'sql_driver' => 'mysql',

    /**
     * Default language
     */
    'language' => [
        'key' => 'tr',
        'name' => 'Türkçe',
        'local' => 'tr_TR'
    ],
    /**
     * Charset
     */
    'charset' => 'UTF-8',

    /**
     * Timezone
     */
    'timezone' => 'Europe/Istanbul',

    /**
     * Şifrelemede kullanılacak algoritma
     * AES-256-CBC or AES-128-CBC
     */
    'encrypt_algo' => 'AES-256-CBC',

    /**
     * "md5", "sha256", "haval160,4" ve benzerleri
     */
    'hash_algo' => 'sha256',

    /**
     * true php password_hash DEFUALT_HASH yöntemi kullanılır, aksi halde hash algo ile belirtilen yöntem kullanılır
     * true , false
     */
    'password_hash' => true,

    /**
     * enable [true or false]
     * driver [file or database]
     */
    'log' => [
        'enable' => true,
        'driver' => 'file'
    ],

    /**
     * Cache ayarları
     * enable [true or false]
     * driver [file, memcache, database]
     * memcache [host, port]
     */
    'cache' => [
        'enable' => true,
        'driver' => 'file',
        'memcache' => [
            'host' => 'localhost',
            'port' => 11211
        ],
        'database' => [
            'table' => 'cache'
        ]
    ]
);
```



### autoload.php Uygulama başlatılmadan önce yüklenecek çıktı üretmeyen bileşenler.

```php
<?php
/**
 * Uygulama başlatılmadan önce yüklenecek çıktı üretmeyen bileşenler.
 * {functions} fonksiyon veya php dosyalarını dahil eder.
 * {alias} uzun namespace veya sınıfların takma isimle çağırılmasını sağlar.
 * {services} ilgili services sınıfının boot methodunu çağırır. (bkz. Core\Services )
 */

return [
    'functions' => [

    ],
    'alias' => [
        'Router' => Core\Router\Router::class,
        'Request' => Core\Http\Request::class,
        'View' => Core\View::class,
        'DB' => Core\Database\DB::class,
        'Valid' => Core\Validation\Valid::class,
        'Lang' => Core\Language\Language::class,
        'Exceptions' => Core\Exceptions\Exceptions::class,
        'Config' => Core\Config\Config::class,
        'Auth' => Core\Auth\Auth::class,
        'Hook' => Core\Hook\Hook::class,
        'Session' => Core\Session\Session::class,
        'Cookie' => Core\Cookie\Cookie::class,
        'Cache' => Core\Cache\Cache::class,
        'Logger' => Core\Log\Logger::class,
        'Tag' => Helpers\Html\Tag::class,
        'Meta' => Helpers\Html\Meta::class,
        'Html' => Helpers\Html\Html::class,
    ],
    'services' => [
        'default' => Services\DefaultServices::class,
        'language' => Services\LanguageService::class,
        //'rememberme' => Services\RememberMe::class
    ],
    'routes' => [
        ROOT . 'routes/routing' . EXT,
    ]
];
```



### database.php Veritabanı ayarlarının yapıldığı dosya

```php
<?php
/**
 * Veritabanı sınıfında kullanılacak sql bağlantıları.
 * Bir projede birden fazla sql driver kullanılabilir.
 */
return [
    'mysql' => [
        'driver'    => 'mysql',
        'host'      =>  'localhost',
        'database'  =>  'phpfw',
        'user'      =>  'root',
        'password'  =>  '',
        'charset'   =>  'utf8mb4',
        'collaction' =>  'utf8mb4_general_ci'
    ],
    'mysql2' => [
        'driver'    => 'mysql',
        'host'      =>  'localhost',
        'database'  =>  'test',
        'user'      =>  'root',
        'password'  =>  '',
        'charset'   =>  'utf8mb4',
        'collaction' =>  'utf8mb4_general_ci'
    ]
];
```



### path.php Dizin yapısının belirlendiği dosya

```php
<?php
/**
 * Temel dosya dizin yapıları
 */
return  array(
    'core'  => 'Core',
    'controller' => 'App/Controller',
    'model' => 'App/Model',
    'view' => 'App/View',
    'template' => 'App/View/template',
    'page' => 'App/View/page',
    'assets' => 'assets',
    'lang' => 'lang',
    'helpers' => 'Helpers',
    'routes' => 'routes',
    'services' => 'Services',
    'logs' => 'storage/logs',
    'cache' => 'storage/cache'
);
```