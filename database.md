# Database

Veritabanı işlemleri için PDO tabanlı sınıf. Bağlantı ayarları config/database.php dosyasından yapılır. Default veritabanı config/app.php dosyasından belirlenir. 

namespace Core\Database\DB

### Methodlar

- [DB::get](#get)(string $query, array $bindings, $fetchStyle = PDO::FETCH_OBJ): object | array
- [DB::getRow](#getRow)(string $query, array $bindings, $fetchStyle = PDO::FETCH_OBJ): object | array
- [DB::getVar](#getVar)(string $query, array $bindings, $fetchStyle = PDO::FETCH_OBJ): string | int  | mixed
- [DB::getCol](#getCol)(string $query, array $bindings): object | array
- [DB::insert](#insert)(string $query, array $bindings): int //inserted_id
- [DB::update](#update)(string $query, array $bindings): int //etkilenen satır sayısı "etkilenen satır sayısı 0 ise ve sorgu sorunsuz çalışmışsa true"
- [DB::delete](#delete)(string $query, array $bindings): int //silinen satır sayıs
- [DB::selectDB](#selectDB)(string $database): Database
- [DB::transaction](#transaction)():void
- [DB::commit](#commit)():void
- [DB::rollBack](#rollBack)():void
- [DB::pdo](#pdo)(): PDO
- [DB::stm](#stm)(): PDOStatement
- [DB::selectDriver](#selectDriver)(string $driver): Database



### get

Tüm sorgu sonuçlarını döndürür.

```php
DB::get("select * from users where active = ?", [1]); //object ön tanımlı
DB::get("select * from users where active = ?", [1], PDO::FETCH_ASSOC); // dizi
```



### getRow

Sorgu sonuçlarından sadece bir satır döndürür.

```php
DB::getRow("select * from users where id = ?", [1]); //object tek bir satır
```



### getVar

Sorgu sonuçlarından tek satırdan bir stun döndürür.

```php
DB::getVar("select userName from users where id = ?", [1]); //string userName
```



### getCol

Sorgu sonuçlarından sadece belirtilen stunu döndürür.

```php
DB::getCol("select userName from users limit 0, 50"); //object 
```



### insert

Sorgu sonucu insert edilen son id döner.

```php
DB::insert("insert into Table set column = ?", [$data]); //inserted_id
```



### update

Update işleminden etkilenen satır sayısını döndürür. Etkilenen satır sayısı 0 ise ve sorgu sorunsuz çalışmışsa true döner.

```php
DB::update("update Table set column = ?", [$data]); //etkilenen satır sayısı
```



### delete

Silme işleminden etkilenen satır sayısını döndürür.

```php
DB::delete("delete from Table where column = ?", [$data]);
```



### selectDB

Veritabanları arasında geçiş yapar, parametre olarak veritabanı adı alır.

```php
DB::selectDB("DatabaseName");
```



### transaction

Bir transaction işlemi başlatır.

```php
DB::transaction(); //void
```



### commit

Bir transaction işlemini çalıştırır.

```php
DB::commit(); //void
```



### rollBack

Bir transaction işlemini geri alır.

```php
DB::rollBack(); //void
```



### pdo

Aktif pdo sınıfını döndürür, bu sınıf üzerinden diğer tüm pdo özelliklerine ulaşılabilir.

```php
DB::pdo()->exec("delete from Table where status = 'delete'");
```



### stm

Aktif PDOStatement nesnesini döndürür, üzerinden diğer sınıf özellikleri kullanılabilir.

```php
DB::stm()->lastInsertId();
```



### selectDriver

Veritabanı için farklı bir driver üzerinden bağlantı açar. config/database.php dosyasından bir driver ismi.

```php
DB::selectDriver("remootMysql"); // Database
```

