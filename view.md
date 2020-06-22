# View

App/View altında 4 farklı temel dizin yapısı vardır. Bu dizinler altında view dosyalarınızı oluşturup değişken göndeilebilir.

> App/View/page/
>
> App/View/part/
>
> App/View/template/
>
> App/View/error/

Örnek

```php
page("index", ["hello"=>"Merhaba dünya"]);
```

App/View/page/index.php

```php+HTML
<h3><?=$hello?></h3>
```

<h3>Merhaba Dünya</h3>



## Methodlar

page(string $fileName, array $data = [])

path(string $fileName, array $data = [], $ext = EXT)

template(string $fileName, array $data = [])

json($data)

render($code = null, $headers = null)

getBuffer()

setTemplate(string $template)

::insertData(array $data)



## Page

App/View/page/ dizini altındaki dosyaları kullanır.

```php
$view = new View();
$view->page("home", ["hollo" => "Merhaba dünya"])->render();
```

App/View/page/home.php

```php+HTML
<span><?=$hello?></span>
```



## Path

App/View/ dizini altında farklı dizinlere ve dosya uzantılarına erişebilir. Örnek dizin part/ ise;

```php
$view = new View();
$view->path("part/header")->render();
```

```php
$this->view()->path('part/header', ['hello' => 'Merhaba dünya'])->page('home')->path('part/footer')->render();
```

**Sayfa çıktısı**

```html
<body>
    <h3>Header</h3>	//App/View/part/header.php
    <div>Merhaba dünya</div> //App/View/page/home.php
    <h3>Footer</h3> //App/View/part/footer.php
</body>
```



## Template / setTemplate

App/View/template/ dizini altında farklı sayfa mizanpajlarını oluşturmak için kullanılır. Bir önceki örnekte oluşturulan sayfa bölümlerinin oluşturulmasını basitleştirmek için kullanılabilir.

App/View/template/default.php

```php+HTML
<body>
<?php
viewPath('part/header');
page(View::$dynamicPage);
viewPath('part/footer');
?>
</body>
```

App/View/template/withSidebar.php

```php+HTML
<body>
<?php
viewPath('part/header');
viewPath('part/leftSide');
page(View::$dynamicPage);
viewPath('part/rightSide');
viewPath('part/footer');
?>
</body>
```

**Kullanımı**

```php
$view = new View();
$view->template("home", ["data" => $data])->render(); //default.php dosyası kullanılacaktır
```

### setTemplate

```php
$view = new View();
$view->setTemplate("withSidebar")->template("home", ["data" => $data])->render(); //withSidebar.php dosyası kullanılacaktır.
```



## Json

Header bilgisi dahil edilerek json çıktısı oluşturur.

```none
Content-Type: application/json
```

```php
json(["data" => $data]);
```

```php
$view = new View();
$view->json(["data" => $data])->render();
```



## Render

View methodları tarafından oluşturulan önbellek verilerinin ekrana çıktısını döndürür. Sayfa durum kodu ve ekstra header bilgisi gönderilebilir.

render($code = 200, $headers = string | array)

```php
$view = new View();
echo $view->page("home");
```

```php
$view = new View();
$view->page("sitemap")->render(200, ["Content-Type" => "application/xml; charset=utf-8"]);
//veya
$view->page("sitemap")->render(200, "Content-Type: application/xml; charset=utf-8");
```



## getBuffer

View methodları tarafından oluşturulan önbellek verilerini döndürür. Örneğin cache için kullanılabilir.

```php
$cacheVariable = $view->page("home")->getBuffer();
```



## insertData

View::insertData static fonksiyondur, tüm viev sayfalarında gerekli olan veya önceden dahil edilmesi gerek değişkenler için kullanılabilir.

```php
View::insertData(["menu" => $menuVariables], "newMessage" => 0);
```



### Helper

**page**

```php
$view = new View();
$view->page("home")->render();
//veya
page("home");
```

viewPath

```php
$view = new View();
$view->path("errors/custom_error_page")->render();
//veya
page("errors/custom_error_page");
```

**template**

```php
$view = new View();
$view->template("home", ['data' => $data])->render();
//veya
template("home", ['data' => $data]);
```

**setTemplate**

```php
$view = new View();
$view->setTemplate("customTemplate");
//veya
setTemplate("customTemplate");
```

**json**

```php
$view = new View();
$view->json($data);
//veya
json($adata);
```

