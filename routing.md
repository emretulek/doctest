
# Routing
İstekleri işlemek için rotalar kullanılır, rota bir anonim fonksiyon yada belirtilen controller sınıfından bir method olabilir.
### Routing Methods

>
> - Router::get(string $pattern, callable | controller@method@param)
>   
> - Router::post(string $pattern, callable | controller@method@param)
>   
> - Router::put(string $pattern, callable | controller@method@param)
>   
> - Router::delete(string $pattern, callable | controller@method@param)
>   
> - Router::patch(string $pattern, callable | controller@method@param)
>   
> - Router::head(string $pattern, callable | controller@method@param)
>   
> - Router::options(string $pattern, callable | controller@method@param)
>   
> - Router::any(string $pattern, callable | controller@method@param)
>   
> - Router::useMethod(string $pattern, callable | controller@method@param, array $methods = ['GET', 'POST', 'PUT',
>             'DELETE', 'PATCH', 'HEAD', 'OPTIONS'])
>



#### Kullanım Örnekleri

```php
Router::get('porfile', function(){
    echo 'Şuan profil sayfasını görüntülüyorsunuz.';
});
//veya
Router::get('porfile', function(){
    page('profile'); //View/page/profile.php
});
//veya zorunlu parametre ile kullanım
Router::get('porfile/{id}', function($id){
    page('profile', ['id' => $id]); //View/page/profile.php
});
 //veya opsiyonel parametre ile kullanım
Router::get('porfile/{id?}', function($id){
    page('profile', ['id' => $id]); //View/page/profile.php
});
```
```php
Router::get('porfile', 'profileController@methodeName');
//veya
Router::get('porfile/{string}', 'profileController@methodeName');
 //profileController.php
public function methodeName($string)
{
    $this>view->page('profile', ['userName' => $string]);
}
```
```php
Router::useMethod('porfile', 'profileController@methodeName', ['POST', 'GET']);
//veya
Router::any('porfile', 'profileController@methodeName');
//veya
Router::get('porfile/{string}', 'profileController@methodeName');
//profileController.php
public function methodeName($string)
{
    $this->view->page('profile', ['userName' => $string]);
}
```
### Pattern
>  {id} => 0-9 pozitif,
>    {int} => 0-9,
>    {string} => yazı karakterleri
>    {*} => her hangi birşey
>    ? => {id?} => opsiyonal, zorunlu olmayan parametre



### Routing Name

Rotalar daha sonra erişilmesi için isimlendirilebilir.

```php
Router::get('porfile/{string}', 'profileController@methodeName')->name('profile');

//örnek
$routes = Router::getNames("profil.*");

//veya aktif route kontrol edileiblir
if(Router::matchName("profil")){
    //aktif menü belirleme gibi işlemler
}
```



### Routing Middleware

Rotalar tarafından yönlendirilen istekler çalışmadan önce yada sonra gerçekleştirilecek bir takım işlevler tanımlamanızı sağlar. Middleware sınıfları IMiddleware sınıfından türetilmeli, ::before ve ::after methodları içermelidir.

```php
Router::get('porfile/{string}', 'profileController@methodeName')->middleware([AjaxCheck::class]);

//veya group ile kullanılabilir
Router::group(['middleware' => AjaxCheck::class], function(){
    //sadece ajax isteklerinin kullanılacağı alan
    Router::get('porfile/info/{id}', 'profileController@methodeName');
});
```



### Routing Prefix

Ön ekleri kullanarak istekleri gruplandırabilirsiniz. İstek gelen uride ön ek dikkate alınmaz.

```php
Router::group(['prefix' => 'admin'], function(){
    //adres satırında admin/dashboard burada değerlendirilir
    Router::get('dashboard', 'dashboard@methodeName');
    //admin/settings
    Router::get('settings', 'settings@methodeName');
});
```



### Routing Namespace

Alt dizinde çalışacak dosyalar için namespace belirlenebilir, prefix ile kullanımına örnek

```php
Router::group(['prefix' =&gt; 'panel', 'namespace' =&gt; 'admin'], function(){
//adres satırında panel/dashboard
//Controller\Admin\Dashboard altındaki controller çalışacaktır.
Router::get('dashboard', 'dashboard@methodeName');
```



### Routing Errors

404 ve 405 için özel sayfalar belirlenmesini sağlar

```php
Router::errors(404, function(){
    page("custom-404");
});
```



### Routing diğer methodlar

```php
Routing::getNameSpace() // aktif namespace değeri
Routing::getPrefix() // aktif ön ek değeri
Routing::getParams() // gönderilen parametreler
Routing::getController() // aktif controller
Routing::getMethod() // aktif method
```


