# Model

App/Model dizini altında mvc yapısına uygun olarak model dosyaları oluşturulur. Model sınıfları **Core\Model** sınıfından türetilir.

```php
<?php

namespace App\Model;

use Core\Model;

class UserModel extends Model
{
	//modelin kullanacağı ana tablo
    protected string $table = 'table';
    //tablo primary key
    protected string $primary = 'primary';

    public function findFromUserName($userName)
    {
        return $this->DB::getRow("select * from users where userName = ?", [$userName]);
    }
    
    //DB sınıfı static
    public function findFromUserName2($userName)
    {
        return DB::getRow("select * from users where userName = ?", [$userName]);
    }
}
```

**Örnek:**

```php
//Kullanım şekli iki farklı yöntemle gerçekleşir
$this->model('UserModel')->methodName();
//veya UserModel::static() methodu üzerinden diğer methodlara erişilebilir.
UserModel::static()->methodName();
```



### App\UserController içinde model kullanımı

```php
<?php

namespace App\Controller;

use App\Model\UserModel;

class UserController extends Core\Controller
{
    //default controller
    public function main($userName)
    {
        if($user = UserModel::static()->findFromUserName($userName)){
            return page('index', compact('user'));
        }
        
        return page('error', ['message' => 'Kullanıcı bulunamadı.']);
    }
    
    //Core\Model sınıfından bazı kalıtımlar model sınıflarına aktarılır
    public function example($id)
    {
        //getAll modelin bağlandığı tablonun tamamı
        $userTable = UserModel::static()->getAll();
        
        //getFirst modelin bağlandığı tablonun ilk satırı
        $first = UserModel::static()->getFirst();
        
        //getLast modelin bağlandığı tablonun son satırı
        $last = UserModel::static()->getLast();
        
        //find($primary) modelin bağlandığı tabloda id ile arama
        $find = UserModel::static()->find(2);
        
        //delete modelin bağlandığı tabloda id ile silme işlemi
        $userTable = UserModel::static()->delete(3);
    }
}
```