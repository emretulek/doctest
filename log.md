# Logger

Uygulamada yapılan işlemlerin belirli bir standartta loglanmasını sağlar. Log için dosya sistemi veya veritabanı kullanılabilir. config/app.php dosyasından ayarlar yapılır. Logger sınıfı kullanılsa dahi config/app.php dosyasından aktif edilemeden loglama yapmaz.

```php
/**
 * enable [true or false]
 * driver [file or database]
 */
'log' => [
    'enable' => true,
    'driver' => 'file'
],
```

Database için veritabanı tablo yapısı.

```php
CREATE TABLE `logger` (
    `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
    `type` VARCHAR(32) NOT NULL,
    `message` VARCHAR(256) NULL DEFAULT NULL,
    `data` TEXT NULL,
    `time` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;
```



### Methodlar

- Logger::writer($message, $data, $type)
- Logger::debug($message, $data = null)
- Logger::info($message, $data = null)
- Logger::notice($message, $data = null)
- Logger::warning($message, $data = null)
- Logger::error($message, $data = null)
- Logger::critical($message, $data = null)
- Logger::emergency($message, $data = null)



### Tüm methodların kullanımı aynıdıri aralarında ki fark log tipini değişmesidir.

Başarılı şekilde log yazılabilirse true aksi halde false döndürür.  **$data serialize edilerek yazılır.**

```php
Logger::debug("$user isimli kullanıcı sisteme giriş yaptı.", $user_info);
```

Logger.log

```verilog
08.20.2020 21:20:19	[DEBUG]	xxx isimli kullanıcı sisteme giriş yaptı.	a:1:{s:8:"username";s:3:"xxx";}
```

