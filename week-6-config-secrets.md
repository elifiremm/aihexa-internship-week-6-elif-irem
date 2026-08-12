# Secret ve Config Yönetimi

## 12-Factor Config Prensibi Nedir?

12-Factor yaklaşımında uygulama yapılandırmalarının doğrudan kaynak kod içerisinde tutulmaması önerilir. Veritabanı bağlantı bilgileri, API anahtarları, port bilgileri veya ortam ayarları gibi değerler koddan ayrılmalıdır.

Bu sayede aynı uygulama farklı ortamlarda farklı ayarlarla çalıştırılabilir.

Örneğin:

* Development ortamı
* Test ortamı
* Production ortamı

aynı kaynak kodu kullanabilir ancak farklı environment variable değerlerine sahip olabilir.

## Secret Nedir?

Secret, uygulamanın çalışması için gerekli olan ancak herkes tarafından görülmemesi gereken hassas bilgidir.

Secret örnekleri:

* Veritabanı şifresi
* API anahtarı
* JWT secret key
* SMTP şifresi
* Cloud erişim anahtarı
* Özel servis token'ları

Bu tür bilgiler GitHub repository içerisine doğrudan yazılmamalıdır.

## Secret Neden Repository İçerisinde Tutulmamalıdır?

Bir secret Git repository içine commit edildiğinde daha sonra dosyadan silinse bile eski commit geçmişinde kalabilir.

Bu durum güvenlik riski oluşturur.

Bu nedenle secret değerleri:

* Kaynak kod içerisine yazılmamalıdır.
* GitHub repository içerisine gönderilmemelidir.
* Docker image içerisine doğrudan gömülmemelidir.
* Environment variable veya güvenli secret yönetim sistemleri üzerinden verilmelidir.

## Secret Rotation Nedir?

Secret rotation, kullanılan şifre, token veya API anahtarlarının belirli aralıklarla değiştirilmesidir.

Bir secret'ın ele geçirilmesi veya yanlışlıkla paylaşılması durumunda eski secret devre dışı bırakılarak yeni bir değer oluşturulur.

Örneğin bir API anahtarının GitHub'a yanlışlıkla yüklenmesi durumunda yalnızca dosyadan silmek yeterli değildir. İlgili API anahtarı iptal edilmeli ve yeni bir anahtar oluşturulmalıdır.

## Development, Test ve Production Ortamları

### Development

Yazılımcının geliştirme yaptığı ortamdır.

Hata ayıklama ve test işlemleri daha rahat yapılabilir.

### Test

Uygulamanın production ortamına gönderilmeden önce test edildiği ortamdır.

Gerçek kullanıcı verileri yerine test verileri kullanılabilir.

### Production

Uygulamanın gerçek kullanıcılar tarafından kullanıldığı canlı ortamdır.

Production ortamındaki güvenlik ve yapılandırma kuralları daha sıkı olmalıdır.

## Dev, Test ve Prod Config Neden Ayrılır?

Her ortamın ihtiyaçları farklıdır.

Örneğin development ortamında:

* Local database kullanılabilir.
* Debug logları açık olabilir.

Production ortamında ise:

* Gerçek database kullanılır.
* Güvenlik ayarları daha sıkıdır.
* Debug bilgileri kullanıcıya gösterilmez.

Bu nedenle config değerlerinin ortama göre ayrılması gerekir.

## Environment Variable Nedir?

Environment variable, uygulama dışından verilen yapılandırma değeridir.

Örneğin:

```text
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=aihexa
```

Uygulama bu değerleri çalışma sırasında okuyabilir.

Böylece config değerleri kaynak koddan ayrılmış olur.

## .env Dosyası Nedir?

`.env`, environment variable değerlerinin yerel geliştirme ortamında saklanması için kullanılan dosyalardan biridir.

Örnek:

```text
APP_ENV=development
APP_PORT=8080
DB_HOST=localhost
DB_USERNAME=aihexa
DB_PASSWORD=secret-password
```

Gerçek `.env` dosyasında secret bulunabileceği için GitHub'a gönderilmemelidir.

## .env.example Nedir?

`.env.example`, projede hangi environment variable'ların gerektiğini göstermek için oluşturulan örnek dosyadır.

Gerçek secret değerleri içermez.

Örnek:

```text
APP_ENV=development
APP_PORT=8080
DB_HOST=localhost
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

Yeni bir geliştirici repository'yi indirdiğinde bu dosyayı inceleyerek hangi config değerlerini oluşturması gerektiğini anlayabilir.

## .gitignore Neden Kullanılır?

`.gitignore`, Git tarafından takip edilmemesi gereken dosyaları belirtir.

Örneğin:

```text
.env
```

satırı eklenirse `.env` dosyası Git tarafından repository'ye gönderilmez.

Secret içeren dosyaların `.gitignore` içerisinde tanımlanması güvenlik açısından önemlidir.

## Config Envanteri Nedir?

Config envanteri, uygulamada kullanılan tüm yapılandırma değerlerinin listesidir.

Örnek bir config envanteri:

| Config      | Açıklama                   | Secret mı? | Ortama Göre Değişir mi? |
| ----------- | -------------------------- | ---------- | ----------------------- |
| APP_PORT    | Uygulama portu             | Hayır      | Evet                    |
| APP_ENV     | Çalışma ortamı             | Hayır      | Evet                    |
| DB_HOST     | Database adresi            | Hayır      | Evet                    |
| DB_USERNAME | Database kullanıcı adı     | Bazen      | Evet                    |
| DB_PASSWORD | Database şifresi           | Evet       | Evet                    |
| API_KEY     | Harici servis API anahtarı | Evet       | Evet                    |

Bu tablo hangi değerlerin korunması gerektiğini anlamayı kolaylaştırır.

# Cache

## Cache Nedir?

Cache, sık kullanılan verilerin geçici olarak daha hızlı erişilebilen bir alanda tutulmasıdır.

Amaç aynı veriyi tekrar tekrar üretmek veya uzak bir kaynaktan tekrar almak yerine daha hızlı şekilde kullanmaktır.

## Cache Performansı Nasıl Artırır?

Örneğin bir uygulama aynı veriyi veritabanından sürekli çekiyorsa bu veri belirli bir süre cache içerisinde tutulabilir.

Sonraki isteklerde database'e tekrar gitmek yerine cache içerisindeki veri kullanılabilir.

Bu yöntem:

* Response süresini azaltabilir.
* Database yükünü azaltabilir.
* Sistem performansını artırabilir.

Ancak cache içerisindeki verilerin güncelliği de doğru şekilde yönetilmelidir.

# Git Squash

## Squash Nedir?

Squash, birden fazla Git commit'ini tek bir commit haline getirme işlemidir.

Örneğin bir özellik geliştirilirken şu commitler yapılmış olabilir:

```text
Add form
Fix form
Fix validation
Update form
```

Squash yapıldığında bunlar tek bir anlamlı commit haline getirilebilir:

```text
Add user registration form
```

Bu yöntem commit geçmişinin daha düzenli ve okunabilir olmasını sağlar.

# Güvenli Dosya Paylaşımı

Dijital ortamda dosya paylaşırken kişisel ve hassas bilgilerin korunması önemlidir.

Dosya paylaşırken:

* Public link kullanırken dikkat edilmelidir.
* Gereksiz erişim izinleri verilmemelidir.
* Restricted link kullanılabilir.
* Paylaşım süresi sınırlandırılabilir.
* Kişisel veya hassas veriler paylaşılmadan önce kontrol edilmelidir.

Özellikle kurum içi belgelerde herkesin erişebileceği public bağlantılar yerine yetkili kullanıcıların erişebildiği bağlantılar tercih edilmelidir.

# Java Stream API

## Stream API Nedir?

Java Stream API, koleksiyonlar üzerinde filtreleme, dönüştürme ve sıralama gibi işlemleri daha okunabilir şekilde gerçekleştirmek için kullanılan bir yapıdır.

Örneğin bir öğrenci listesinde yalnızca aktif öğrencileri almak için `filter` kullanılabilir.

## filter

Belirli bir koşula uyan elemanları seçmek için kullanılır.

## map

Bir veriyi başka bir biçime dönüştürmek için kullanılır.

## sorted

Elemanları belirli bir kurala göre sıralamak için kullanılır.

## collect

Stream sonucunu tekrar bir koleksiyon haline getirmek için kullanılabilir.

Stream API genellikle Lambda Expression ve Functional Interface yapılarıyla birlikte kullanılır.

# Bootstrap 5 Form Validation

Bootstrap Validation, formlarda kullanıcıya başarılı veya hatalı girişler hakkında görsel geri bildirim vermek için kullanılabilir.

Örneğin zorunlu bir alan boş bırakıldığında kullanıcıya hata mesajı gösterilebilir.

Bu yapı kullanıcı deneyimini geliştirir ve hatalı form gönderimlerinin azaltılmasına yardımcı olur.

# Bootstrap Toast

Toast, kullanıcıya kısa süreli bilgilendirme mesajı göstermek için kullanılan Bootstrap bileşenidir.

Örneğin:

* Kayıt başarıyla oluşturuldu.
* Dosya başarıyla yüklendi.
* İşlem sırasında hata oluştu.

gibi mesajlar toast ile gösterilebilir.

# Bootstrap Dropdown

Dropdown, kullanıcıya bir buton veya bağlantı üzerinden açılan seçenek listesi sunar.

Örneğin profil menüsünde:

* Profil
* Ayarlar
* Çıkış Yap

seçenekleri dropdown içerisinde gösterilebilir.

# AIHEXA Projesinde Kullanımı

AIHEXA projelerinde database bağlantı bilgileri, API anahtarları ve diğer hassas bilgiler source code içerisinde tutulmamalıdır.

Development, test ve production ortamları için farklı config değerleri kullanılabilir. Gerçek secret değerleri `.env` veya uygun secret yönetim mekanizmalarında saklanırken `.env.example` dosyası yalnızca gerekli değişkenlerin isimlerini göstermek için repository içerisinde tutulabilir.

Bu yaklaşım hem güvenliği artırır hem de uygulamanın farklı ortamlarda daha kolay çalıştırılmasını sağlar.
