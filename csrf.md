# Csrf

Cross Site Request Forgery (Csrf) türkçeye Siteler Arası İstek Sahtekarılığı olarak çevirilir. Bu açığı engellemek için genellikle istek parametreleri için bir token dahil edilir. Bu sınıf sayesinde token otomatik oluşturulur ve gerektiğinde ilgili method ile doğrulanabilir. 

get veya post isteklerinde parametre adı `csrf_token` olmak zorundadır.



## Methodlar

Csrf::generateToken()

Csrf::token()

Csrf::check()

Csrf::checkPost()

Csrf::checkGet()

Csrf::checkCookie()



### generateToken

Her istek yapıldığında yeni bir token oluşturup, oturum verilerisi olarak saklar. Uygulamadan önce bir servis içinde çağırılması gerekir.

```php
Csrf::generateToken();
```



### token

Oluşturulan csrf tokeni döndürür.

```php
<input name="csrf_token" value="<?=Csrf::token()?>">
```



### check

post veye get isteklerinde `csrf_token` isimli parametreye göre kontrol gerçekleştirir.

```php
if(Csrf::check()){
	//csrf yakalandı
}
```



### checkPost

post isteklerinde `csrf_token` isimli parametreye göre kontrol gerçekleştirir.

```php
if(Csrf::checkPost()){
	//post isteğinde csrf yakalandı
}
```



### checkGet

get isteklerinde `csrf_token` isimli parametreye göre kontrol gerçekleştirir.

```php
if(Csrf::checkGet()){
	//get isteğinde csrf yakalandı
}
```



### checkCookie

Sadece referer bilgisi ve cookie üzerinden kontrol gerçekleştirir. Güvenilir değildir fakar basit formlarda f5 flood engelleme için kullanılabilir. Parametre olarak `csrf_token` eklenmesi gerekmez. Browserların f5 yapıldığında `max-age=0` header bilgisi kontrol edilir.

