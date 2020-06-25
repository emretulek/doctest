# Cache

Önbellek için dosya sistemi, veritabanı ve memcache gibi üç farklı yöntem kullanılabilir. cache/app.php ayar dosyasından kullanılacak driver ve gerekli ayarlar yapılabilir.

```php
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
```

Database için veritabanı tablo yapısı.

```sql
CREATE TABLE `cache` (
    `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
    `key` VARCHAR(32) NOT NULL,
    `value` BLOB NULL,
    `compress` TINYINT(1) UNSIGNED NOT NULL DEFAULT '0' COMMENT '0,1',
    `expires` TIMESTAMP NOT NULL,
    PRIMARY KEY (`id`, `key`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;
```



## Methodlar

- Cache::add($key, $value, $compress = false, $expires = 2592000)
- Cache::set($key, $value, $compress = false, $expires = 2592000)
- Cache::get($key)
- Cache::use($key, Clouser $clouser, $compress = false, $expires = 2592000)
- Cache::delete($key)
- Cache::flush()



### add

Önbelleğe yeni bir veri ekler aynı anahtara sahip veri varsa yazılmaz. Sıkıştırma aktif edilirse veriler ziplenerek saklanır. Saklama süresi saniye cinsinden olup belirtilmezse 30 gündür.

```php
Cache::add("userList", $userList);

Cache::add("userList", $userList, false, 60 * 60); // Saklama süresi 1 saat olarak değiştirildi.
```



### set

add ile aynı olup önbellek anahtarı varsa üzerine yazılır.

```php
Cache::add("userList", $userList);
```



### get

Anahtara ait önbellek verisini getirir veri bulunamazsa false döner.

```php
//Aşağıdaki örnekte userList önbellekten kullanılır,
//önbellekte yoksa veya süresi dolmuşsa veritabanından alınıp önbelleğe depolanır
if($userList = Cache::get("userList")){
	return $userList;
}else{
    $userList = DB::get("select * from users");
    Cache::set("userList", $userList);
    return $userList;
}
```



### use

Önbellek için bir clouser belirtilir, eğer önbellek yoksa veya süresi dolmuşsa fonksiyon sonucu kullanılarak önbellek oluşturulur.

```php
//Aşağıdaki örnek get methodunda verilen örnek ile aynı sonucu üretir.
return Cache::use('userList', function(){
	return DB::get("select * from users");
});
```



### delete

Önbellekte depolanan bir veriyi silmek için kullanılır.

```php
Cache::delete("userList");
```



### flush

Önbellekteki tüm veriler silinir.

```php
Cache::flush();
```

