# Controller

App/Controller dizini altında controller sınıfları oluşturulur, bu sınıflara router ile erişilerek istekler işlenebilir. Controller sınıfları Core\Controller sınıflarından türetilerek kullanılır.

```php
<?php

namespace App\Controller;

use Core\Controller;
use App\Model\UserModel;

class UserController extends Controller
{
    public function profil($id)
    {
        return page('user_profil', ['user' => UserModel::static()->find($id)]);
    }
}
```

```php
Router:::get('user/{id}', 'UserController@profil');
```



Controller sınıflarından View ve Model sınıflarına kolay erişim için iki method bulunur.

### View $this->view()

```php
namespace App\Controller;

use Core\Controller;

class IndexController extends Controller
{
    public function home($id)
    {
        $this->view()->page("home")->render();
    }
}
```



### Models $this->model($model)

```php
namespace App\Controller;

use Core\Controller;

class UserController extends Controller
{
    public function profil($id)
    {
        if($user = $this->model("UserModel")->find($id)){
            return page('user_profil', compact('user'));
        }
    }
}
```

