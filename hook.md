# Hook

Hook sınıfı özellikle içerik yönetim sistemlerinde ihtiyaç duyulabilen kanca sistemi için kullanılır.

## Methodlar

- Hook::add($hook_name, $callback, $priority = 50)
- Hook::exec($hook_name, $param)
- Hook::remove($hook_name)
- Hook::list()
- Hook::exists($hook_name)



### add

Adı girilen kanca için yeni bir işlem tanımlar. Callback işlem sonucunu return etmelidir. priority ise işlemin önceliğini belirler, sayı ne kadar düşükse öncelik o kadar yüksektir.

```php
Hook::add('top_menu', function($items) use ($item) {
    return $items[] = $item;
});
```



### exec

Kanca kuyruğunu "priority" öncelik sırasına göre çalıştırarak işlem sonucunu döndürür.

```php
$menu_items = Hook::exec('top_menu', $menu_items);
```



### remove

Adı girilen kanca kuyuruğunu temizler.

```php
Hook::remove('top_menu');
```



### list

Tüm kanca kuyruklarını listeler.

```php
Hook::list()
```



### exists

Adı girilen kanca var mı kontrol eder.

```php
if(Hook::exists('top_menu')){
	//kanca varsa yapılacak işlem
}
```

