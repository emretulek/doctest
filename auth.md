# Auth

Kimlik doğrulama ve yetki kontrol sınıfı olarak kullanılır, fakat yetki oluşturma, atama veya silme işlemleri için methodlar sunmaz. Kullanıcı ve yetkilendirme  işleveri geliştirici tarafından oluşturulabilir. Veritabanı tablolarına storage/database/ dizininden ulaşabilinir.

## Methodlar

- [Auth::login($id, $username, $password, $group, $userInfo = [], $remember = 0)](#login)
- [Auth::logout($clear_all)](#logout)
- [Auth::rememberMe()](#rememberMe)
- [Auth::check()](#check)
- A[uth::setInfo()](#setInfo)
- [Auth::info()](#info)
- [Auth::userName()](#userName)
- [Auth::userID()](#userID)
- [Auth::userGroupName()](#userGroupName)
- [Auth::guard($groupName)](#guard)
- [Auth::permission()](#permission)



### login

Bilgileri girilen kullanıcı için oturum verilerini ayarlar. $userInfo dizisinde kullanıcıya ait diğer veriler depolanabilir. $remember seçeneği 0  dığında bir değer alırsa beni hatırla `Auth::rememberMe()` için cookie oluşturulur. Süre saat cinsinden hesaplanır. 24 için bir süreli cookie oluşturulur.

```php
Auth::login(1, "admin", "hashed password", 1); // giriş yapan kullanıcı id'si döner.
Auth::login(1, "admin", "hashed password", 1, [], "1m"); //1 ay süreli oturum çerezi oluşturulur.
```



### logout

Kullanıcıya ait oturumu verilerini silerek, oturumu sonlandırır. $clear_all true belirtilirse, tüm oturum ve cookieler silinir, belirtilmezse sadece kullanıcı oturum bilgileri silinir.

```php
Auth::logout();//kullanıcı oturum bilgileri silindi.

Auth::logout(true); //Tüm cookie ve session bilgileri temizlendi.
```



### rememberMe

Bu method uygulama yüklenmeden önce çağırılırsa, beni hatırla seçeneğini seçen kullanıcılar otomatik giriş yapacaktır. Service olarak oluşturulabilir.

```php
<?php
namespace Services;

use Core\Services\Services;
use Core\Auth\Auth;
use DB;

class AuthServices extends Services
{
    protected function boot()
    {
        if($userID = Auth::rememberMe()){
            $user = DB::getRow("Select * from users where userID = ?", [$userID]);
            Auth::setInfo("email", $user->userEmail); //kullanıcı oturum bilgilerine mail ekleniyor.
        }
    }
}
```



### check

Kullanıcı giriş yapmışsa true aksi halde false döndürür.

```php
if(Auth::check()){
	//kullanıcı giriş yapmış
}
```



### gurad

Kullanıcı grubu için kontrol gerçekleştirir.

```php
if(Auth::guard("admin")){
	//kullanıcı admin grubunda
}

//veya

if(Auth::guard(["admin", "editor"])){
    //kullanıcı admin veya editör grubunda
}
```



### permission

Kullanıcılar için yetki bazında kontrol gerçekleştirir.

```php
if(Auth::permission("delete")){
    //sadece silme yetkisi olanlar
}

//veya

if(Auth::permission(["creat", "update", "delete"])){
    //sadece creati update ve delete yetkilerinin tümüne sahip olanlar
}
```



### setInfo

Kullanıcının oturum bilgilerini yeni bir değer ekler.

```php
Auth::setInfo("email", $userEmail);
```



### info

Kullanıcının oturum bilgilerine erişim sağlar, eğer anahtar girilmemişse tüm oturum bilgilerini döndürür.

```php
Auth::info(); //array
//veya
Auth::info("ip"); //ip adres
```



### userName

```php
Auth::userName(); //kullanıcı adı
```



### userID

```php
Auth::userID(); //kullanıcı id
```



### userGroupName

```php
Auth::userGroupName(); //kullanıcı grubu
```



Veritabanı mysql tabloları

### users.sql

```sql
CREATE TABLE IF NOT EXISTS `users` (
  `userID` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `userName` varchar(256) NOT NULL,
  `userEmail` varchar(256) NOT NULL,
  `userPassword` varchar(256) NOT NULL,
  `userGroup` int(11) unsigned DEFAULT NULL,
  `userIP` varchar(50) DEFAULT NULL,
  `activate` enum('0','1') NOT NULL DEFAULT '0',
  `nameSurname` varchar(256) NOT NULL,
  `registerIP` varchar(50) DEFAULT NULL,
  `registerDate` datetime DEFAULT CURRENT_TIMESTAMP,
  `lastLogin` datetime DEFAULT CURRENT_TIMESTAMP,
  `sessionID` varchar(64) DEFAULT NULL,
  PRIMARY KEY (`userID`),
  UNIQUE KEY `userName` (`userName`),
  UNIQUE KEY `userEmail` (`userEmail`),
  KEY `FK_users_user_groups` (`userGroup`),
  CONSTRAINT `FK_users_user_groups` FOREIGN KEY (`userGroup`) REFERENCES `user_groups` (`groupID`) ON DELETE SET NULL
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4;
```

user_groups.sql

```sql
CREATE TABLE IF NOT EXISTS `user_groups` (
  `groupID` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `groupName` varchar(32) NOT NULL,
  `groupDesc` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`groupID`),
  UNIQUE KEY `groupName` (`groupName`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

user_permissions.sql

```sql
CREATE TABLE IF NOT EXISTS `user_permissions` (
  `permID` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `permName` varchar(32) NOT NULL,
  `permDesc` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`permID`),
  UNIQUE KEY `permName` (`permName`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

user_group_perm.sql

```sql
CREATE TABLE IF NOT EXISTS `user_group_perm` (
  `groupID` int(11) unsigned NOT NULL,
  `permID` int(11) unsigned NOT NULL,
  PRIMARY KEY (`groupID`,`permID`),
  KEY `FK_user_group_perm_user_permissions` (`permID`),
  CONSTRAINT `FK_user_group_perm_user_groups` FOREIGN KEY (`groupID`) REFERENCES `user_groups` (`groupID`) ON DELETE CASCADE,
  CONSTRAINT `FK_user_group_perm_user_permissions` FOREIGN KEY (`permID`) REFERENCES `user_permissions` (`permID`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```