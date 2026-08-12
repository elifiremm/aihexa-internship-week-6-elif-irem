# Docker Compose, Servis Ağı ve Healthcheck

## Docker Compose Nedir?

Docker Compose, birden fazla container'dan oluşan uygulamaları tek bir yapılandırma dosyası üzerinden yönetmek için kullanılır. Örneğin bir projede frontend, backend ve database ayrı container'larda çalışabilir. Her container'ı ayrı ayrı başlatmak yerine Compose ile bu servisler birlikte tanımlanabilir ve çalıştırılabilir.

Compose yapılandırmasında servislerin hangi image'ı kullanacağı, hangi portların açılacağı, environment değişkenleri, volume'lar, network yapısı ve healthcheck gibi ayarlar belirtilebilir.

Örneğin AIHEXA projesinde şu yapı kullanılabilir:

* Frontend → Kullanıcının gördüğü web arayüzü
* Backend → API ve iş mantığı
* Database → Verilerin saklandığı veritabanı

Bu servisler Docker Compose kullanılarak birlikte çalıştırılabilir.

## Container Networking Nasıl Çalışır?

Docker container'ları birbirinden izole ortamlarda çalışır. Ancak bir uygulamada container'ların çoğu zaman birbirleriyle iletişim kurması gerekir.

Docker Compose kullanıldığında servisler için varsayılan olarak ortak bir network oluşturulur. Aynı network içerisindeki servisler birbirlerine servis isimleri üzerinden ulaşabilir.

Örneğin `backend` isimli bir servis ile `database` isimli bir servis bulunuyorsa backend, database servisine servis adını kullanarak erişebilir.

Bu yapı frontend, backend ve database gibi birbirine bağlı sistemlerin container ortamında haberleşmesini kolaylaştırır.

## Healthcheck Nedir?

Healthcheck, çalışan bir container içerisindeki uygulamanın gerçekten sağlıklı olup olmadığını kontrol etmek için kullanılır.

Bir container'ın çalışıyor görünmesi, içerisindeki uygulamanın düzgün şekilde hizmet verdiği anlamına gelmeyebilir. Örneğin container başlamış olabilir ancak web sunucusu henüz hazır olmayabilir veya uygulama bir hata nedeniyle isteklere cevap veremiyor olabilir.

Healthcheck belirli aralıklarla uygulamayı kontrol ederek container'ın durumunun `healthy` veya `unhealthy` olarak değerlendirilmesini sağlar.

## Healthcheck ile Process Running Arasındaki Fark

`Process running`, container içerisindeki ana işlemin hâlâ çalıştığını ifade eder.

Healthcheck ise uygulamanın beklenen hizmeti gerçekten sağlayıp sağlamadığını kontrol eder.

Örneğin Nginx işlemi çalışıyor olabilir fakat web uygulaması doğru şekilde cevap vermiyor olabilir. Bu durumda container çalışıyor görünmesine rağmen uygulama sağlıklı olmayabilir.

Bu nedenle production ortamlarında yalnızca container'ın çalışmasına bakmak yerine uygulamanın sağlık durumunun da kontrol edilmesi önemlidir.

## depends_on Nedir?

Docker Compose içerisindeki `depends_on`, servislerin başlatılma sırasını düzenlemek için kullanılabilir.

Örneğin backend servisi database servisine bağlıysa bu ilişki Compose dosyasında belirtilebilir.

Ancak yalnızca bir servisin başlatılmış olması, o servisin tamamen hazır olduğu anlamına gelmez. Database container'ı çalışmaya başlamış olsa bile veritabanının bağlantı kabul edecek duruma gelmesi birkaç saniye sürebilir.

Bu nedenle servislerin hazır olup olmadığını kontrol etmek için healthcheck mekanizmasının kullanılması önemlidir.

## Container ve Image Arasındaki Fark

Docker image, bir uygulamanın çalıştırılması için gerekli dosyaları ve yapılandırmaları içeren hazır bir şablondur.

Container ise bu image'ın çalışan örneğidir.

Kısaca:

**Image → Uygulamanın çalıştırılabilir şablonu**

**Container → Image kullanılarak oluşturulmuş çalışan ortam**

Aynı image kullanılarak birden fazla container oluşturulabilir.

## Git Merge Stratejileri

### Merge Commit

İki branch'in geçmişini koruyarak birleştirilmesini sağlar. Birleştirme sırasında yeni bir merge commit oluşabilir.

### Squash Merge

Bir branch içerisindeki birden fazla commit tek bir commit haline getirilerek hedef branch'e eklenir. Commit geçmişinin daha sade tutulmasını sağlar.

### Rebase Merge

Bir branch'teki commit'lerin başka bir branch'in sonuna yeniden uygulanmasını sağlar. Böylece daha doğrusal bir Git geçmişi elde edilebilir.

Her yöntemin amacı kod değişikliklerini birleştirmektir ancak commit geçmişini yönetme şekilleri farklıdır.

## KVKK Farkındalığı

KVKK, kişisel verilerin korunmasına yönelik kuralları düzenleyen kanundur. Yazılım geliştirirken kullanıcıların ad, soyad, telefon, e-posta gibi kişisel bilgilerinin gereksiz şekilde toplanmaması ve yetkisiz kişiler tarafından erişilememesi önemlidir.

Bir uygulamada yalnızca gerçekten ihtiyaç duyulan kişisel veriler alınmalı, kullanıcı hangi verilerin neden toplandığı konusunda bilgilendirilmeli ve veriler güvenli şekilde saklanmalıdır.

AIHEXA gibi eğitim ve başvuru sistemlerinde kullanıcı bilgilerinin bulunduğu formlar geliştirildiği için kişisel veri güvenliği özellikle dikkate alınmalıdır.

## Java Functional Interface

Functional Interface, yalnızca bir adet abstract metoda sahip Java interface'idir. Lambda expression kullanımıyla birlikte sık kullanılır.

Java'da yaygın kullanılan functional interface'lerden bazıları şunlardır:

### Predicate

Bir değeri kontrol eder ve `true` veya `false` sonucu üretir.

Örneğin bir öğrencinin yaşının 18'den büyük olup olmadığını kontrol etmek için kullanılabilir.

### Function

Bir değer alır, üzerinde işlem gerçekleştirir ve başka bir değer döndürür.

Örneğin bir kullanıcının adını alıp büyük harfe çevirmek için kullanılabilir.

### Consumer

Bir değer alır ve üzerinde işlem gerçekleştirir ancak sonuç döndürmez.

Örneğin kullanıcı bilgisini ekrana yazdırmak için kullanılabilir.

### Supplier

Herhangi bir parametre almadan bir değer üretir ve döndürür.

Örneğin rastgele bir değer veya varsayılan kullanıcı bilgisi üretmek için kullanılabilir.

## Sonuç

Bugünkü çalışmada Docker Compose ile birden fazla servisin birlikte yönetilmesi, container'ların network üzerinden haberleşmesi ve healthcheck ile uygulama sağlığının kontrol edilmesi incelendi. Ayrıca container-image farkı, Git merge stratejileri, KVKK farkındalığı ve Java Functional Interface kavramları araştırıldı.

Bu yapıların özellikle frontend, backend ve database servislerinden oluşan gerçek projelerin geliştirme ve deployment süreçlerinde önemli olduğu görüldü.
