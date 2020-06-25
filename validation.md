# Validation

Veri formatlarının kontrol edilmesi veya filtrelenmesi işlemleri için kullanılınır. Biri static iki farklı sınıf mevcuttur.

- [Core\Valid](#Valid)
- [Core\Filter](#Filter)

# Valid  

Static methodlar ile veri kontrolü yapar. Genellikle başarı durumunda veriyi başarısızlık durumunda false döndürür. Hata mesajı üretmez.

### Methods

- Valid::bool($input):bool
- Valid::float($input, $min = null, $max = null):float|false
- Valid::int($input, $min = null, $max = null):int|false
- Valid::ip($ip):false|string ip address
- Valid::ipv4($ip):false|string ipv4 address
- Valid::ipv6($ip):false|string ipv6 address
- Valid::mac($input):false|string mac address
- Valid::url($input):false|string url
- Valid::domain($input):false|string domain address
- Valid::email($input):false|string email address
- Valid::phone($input):false|string phone number
- Valid::date($date, $format = "Y-m-d H:i:s"):false|string date
- Valid::username($username, $pattern = '/^[\w]{4,32}$/i'):false|string userName
- Valid::name($username, $pattern = '/^[\w\s]{2,64}$/iu'):false|string name
- Valid::filename($username, $pattern = '/^[^\s\.][\w\-\.\s]{4,255}$/iu'):false|string filename
- Valid::length($input, $min = 1, $max = null):false|int length
- Valid::alpha($input, $unicode = false):false|string
- Valid::number($input):false|string
- Valid::alnum($input, $unicode = false):false|string
- Valid::creditCard($cardNumber):false|string cardnumber
- Valid::tcNo($input):false|string Tc kimlik numarası

```php
Valid::float("2.5555", 0, 3); //return float 2.5555
Valid::float("2.5555", 0, 2); //return false
Valid::float("0", 0, 2); //return 0 
Valid::float("2.5555", 0, 2) === false // biçiminde kontrol edilmeli
```



### phone

Rakam dışında tüm karakterleri kaldırı 9-15 karakter arası değilse false döner.

```php
Valid::phone("+90 555 666 55 55"); //return 905556665555
```



### date

Girilen tarih ile format uyuşuyorsa tarihi döndürü, aksi halde false döner.

```php
Valid::date("2020-09-30 20:10:15", "Y-m-d H:i:s"); //return 2020-09-30 20:10:15
Valid::date("30.08.2020", "Y-m-d H:i:s"); //return false
```



### username

Geçerli username olup olmadığını kontrol eder, ikinci parametre ile desen değiştirilebilir, ön tanımlı desen [a-z0-9_]  4-32 karakter arası.

```php
Valid::username("Ali Osman", '/^[\w\s]{4,32}$/i'); // isteğe bağlı desen kullanılabilir 
```



# Filter

Dizilerin filtrelenmesi için kullanılır. İstenen formata uymayan veriler için aynı indexe sahip bir hata mesajı üretir.

- Filter::__construct(array $params, string $lang = "")
- Filter::input($key, $required = false)
- Filter::float($min = null, $max = null)
- Filter::int($min = null, $max = null)
- Filter::ip()
- Filter::ipv4()
- Filter::ipv6()
- Filter::mac()
- Filter::url()
- Filter::domain()
- Filter::email()
- Filter::phone()
- Filter::date(string $format = "Y-m-d H:i:s")
- Filter::username()
- Filter::name()
- Filter::filename()
- Filter::password(string $repassword = null): return password hash
- Filter::creditCard()
- Filter::tcNo()
- Filter::length($min = null, $max = null)
- Filter::regex($pattern)
- Filter::toAlpha($unicode = false)
- Filter::toNumber()
- Filter::toAlnum($unicode = false)
- Filter::toText(array $allowed = null) 
- Filter::toHtmlEntity()
- Filter::toSecureHtml()
- Filter::toPermaLink(string $allowed = null)
- Filter::label($label)
- Filter::message(string $error_message)
- Filter::result()
- Filter::error()



**Kullanım Örnekleri;**

```php
$filter = new Filter($_POST);
$nameSurname = $filter->input('name_surname', true)->name()->result(); //required name surname
$userName = $filter->input('user_name', true)->username()->result(); //required username
$password = $filter->input('password', true)->password('re_password')->result(); //required password and repassword
$phone = $filter->input('phone')->phone()->result(); //opitonal phone number
$site = $filter->input('site')->url()->message("Web site adresiniz hatalı.")->result();
$birthDay = $filter->input('birthday', true)->date('d-m-Y')->message('Doğum tarihinizi gün-ay-yıl olarak belirtin.')->result();
$age = $filter->input('age')->int(18, 65)->label('Yaşınız için: ')->result();

if($error = $filter->error()){
    return json($error);
}
//Tüm alanlar indexleri ile aynı hata mesajı döndürür.
[
    'name_surname' => 'Ad ve Soyad alanlarında sadece harf kullanılabilir, en az %s, en fazla %s karakter.',
    'user_name' => 'Kullanıcı adında harf ve rakam kullanın. En az %s en fazla %s karakter.',
    'password' => 'Şifreniz en az %s en fazla %s karakter uzunluğunda olmalı',
    'repassword' => 'Şifreleriniz uyuşmuyor.',
    'phone' => 'Geçersiz telefon numarası.',
    'site' => 'Web site adresiniz hatalı.', //message methodu ile değiştirildi
    'birthday' => 'Doğum tarihinizi gün-ay-yıl olarak belirtin.' //message methodu ile değiştirildi
    'age' => 'Yaşınız için: En az 18, en fazla 65 arasında bir değer kullanın.' //label methodu ile başına ekleme yapıldı
]
```

**Tüm Mesajlar**

```php
$messages = [
    'no_index' => 'Geçersiz index',
    'required' => 'Zorunlu alan.',
    'email' => 'Geçersiz E-posta adresi.',
    'url' => 'Hatalı web adresi.',
    'username' => 'Kullanıcı adında harf ve rakam kullanın. En az %s en fazla %s karakter.',
    'name' => 'Ad ve Soyad alanlarında sadece harf kullanılabilir, en az %s, en fazla %s karakter.',
    'filename' => 'Uygunsuz dosya adı.',
    'password' => 'Şifreniz en az %s en fazla %s karakter uzunluğunda olmalı',
    'repassword' => 'Şifreleriniz uyuşmuyor.',
    'min_len' => 'En az %s karakter kullanın.',
    'max_len' => 'En fazla %s karakter kullanın.',
    'between_len' => 'En az %s, en fazla %s karakter kullanın.',
    'min' => 'En az %s olmalı.',
    'max' => 'En fazla %s olmalı.',
    'between' => 'En az %s, en fazla %s arasında bir değer kullanın.',
    'regex' => 'İstenen desene %s uymalısınız.',
    'ip' => 'Geçersiz ip adresi.',
    'ipv4' => 'Geçersiz ipv4 adresi.',
    'ipv6' => 'Geçersiz ipv6 adresi.',
    'mac' => 'Geçersiz mac adresi.',
    'domain' => 'Geçersiz domain.',
    'phone' => 'Geçersiz telefon numarası.',
    'dateFormat' => 'Geçersiz tarih formatı.',
    'creditCard' => 'Geçersiz kredi kartı numarası.',
    'tcNo' => 'Geçersiz TC kimlik numarası.',
];
```