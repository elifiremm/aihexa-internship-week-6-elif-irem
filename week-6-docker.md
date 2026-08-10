# Docker, Dockerfile ve Reproducible Build

## Docker Nedir?

Docker, uygulamaların ihtiyaç duyduğu çalışma ortamı ve bağımlılıklarla birlikte paketlenmesini sağlayan bir container teknolojisidir. Bir uygulamanın geliştiricinin bilgisayarında çalışıp başka bir bilgisayarda çalışmaması gibi ortam farklılıklarından kaynaklanan sorunları azaltmayı amaçlar.

Docker sayesinde uygulama ve uygulamanın çalışması için gerekli ortam birlikte hazırlanabilir. Böylece geliştirilen uygulama farklı bilgisayarlarda ve sunucularda daha tutarlı şekilde çalıştırılabilir.

## Docker Image Nedir?

Docker Image, bir uygulamanın çalıştırılması için gerekli dosyaları, bağımlılıkları ve yapılandırmaları içeren hazır bir şablondur.

Image doğrudan çalışan uygulama değildir. Container oluşturmak için kullanılan temel yapıdır.

Örneğin bir Spring Boot uygulamasının JAR dosyası ve Java çalışma ortamı bir Docker Image içerisinde bulunabilir.

## Docker Container Nedir?

Container, Docker Image'ın çalışan halidir.

Image bir şablon olarak düşünülebilirken container bu şablondan oluşturulmuş çalışan uygulamadır. Aynı image kullanılarak birden fazla container oluşturulabilir.

Temel süreç şu şekildedir:

Dockerfile → Docker Image → Docker Container

## Dockerfile Nedir?

Dockerfile, Docker Image oluşturulurken Docker'ın hangi işlemleri gerçekleştireceğini belirleyen talimat dosyasıdır.

Dockerfile içerisinde genellikle:

- Hangi base image'ın kullanılacağı
- Uygulama dosyalarının nereye kopyalanacağı
- Hangi çalışma dizininin kullanılacağı
- Gerekli bağımlılıkların nasıl hazırlanacağı
- Uygulamanın hangi komutla başlatılacağı

belirtilir.

Örneğin Spring Boot uygulamasında Dockerfile, Java ortamının hazırlanmasını ve oluşturulan JAR dosyasının container içerisinde çalıştırılmasını sağlayabilir.

## Reproducible Build Nedir?

Reproducible build, bir projenin aynı kaynak kod ve aynı yapılandırmalar kullanıldığında farklı ortamlarda tekrar oluşturulabilmesini ifade eder.

Amaç, geliştiricinin bilgisayarında çalışan uygulamanın test veya production ortamında da mümkün olduğunca aynı şekilde oluşturulmasını sağlamaktır.

Docker bu konuda önemlidir çünkü uygulamanın çalışacağı ortamı da tanımlamamıza yardımcı olur.

## Multi-Stage Build Nedir?

Multi-stage build, Dockerfile içerisinde birden fazla build aşaması kullanılmasıdır.

İlk aşamada uygulamanın derlenmesi ve paketlenmesi gerçekleştirilebilir. Sonraki aşamada ise yalnızca uygulamanın çalışması için gerekli dosyalar alınarak daha küçük bir final image oluşturulabilir.

Örneğin Spring Boot projesinde ilk aşamada Maven ve JDK kullanılarak proje build edilebilir. İkinci aşamada yalnızca Java çalışma ortamı ve oluşturulan JAR dosyası kullanılabilir.

Bu yöntem gereksiz build araçlarının final image içerisinde bulunmasını önler.

## Multi-Stage Build Neden Kullanılır?

Multi-stage build kullanmanın başlıca avantajları şunlardır:

- Docker image boyutunu azaltabilir.
- Gereksiz build araçlarının final image içerisinde bulunmasını engeller.
- Daha düzenli Dockerfile hazırlanmasını sağlar.
- Uygulamanın dağıtım sürecini kolaylaştırır.
- Saldırı yüzeyini azaltmaya yardımcı olabilir.

## Base Image Nedir?

Base image, oluşturacağımız Docker Image'ın başlangıç noktasıdır.

Örneğin Java uygulaması çalıştırırken Java çalışma ortamını içeren bir image kullanılabilir. Dockerfile içerisinde `FROM` komutu ile base image belirlenir.

Base image seçerken kullanılan yazılımın sürümüne, image'ın kaynağına, güncelliğine ve içerisinde bulunan gereksiz paketlere dikkat edilmelidir.

## Base Image Seçimi Güvenliği Nasıl Etkiler?

Base image, oluşturulan container'ın temelini oluşturduğu için güvenlik açısından önemlidir.

Eski veya güvenilir olmayan bir base image kullanılması güvenlik açıklarına neden olabilir. Ayrıca gereğinden fazla yazılım içeren büyük image'lar daha geniş bir saldırı yüzeyi oluşturabilir.

Bu nedenle:

- Güvenilir kaynaklardan image kullanılmalıdır.
- Güncel ve desteklenen sürümler tercih edilmelidir.
- Gereksiz paketlerden kaçınılmalıdır.
- Mümkün olduğunda daha küçük runtime image'ları tercih edilmelidir.
- Kullanılan image'ların güvenlik güncellemeleri takip edilmelidir.

## Docker Layer Nedir?

Dockerfile içerisindeki bazı komutlar image oluşturulurken katmanlar meydana getirir. Bu katmanlara layer adı verilir.

Docker, daha önce oluşturulmuş ve değişmemiş katmanları tekrar kullanabilir. Bu yapı build işlemlerinin daha hızlı gerçekleştirilmesine yardımcı olur.

## Docker Cache Nedir?

Docker cache, daha önce oluşturulan uygun layer'ların sonraki build işlemlerinde tekrar kullanılmasını sağlar.

Örneğin uygulamanın bağımlılıkları değişmemişse Docker ilgili katmanı yeniden oluşturmak yerine önbellekten kullanabilir.

Bu nedenle Dockerfile içerisindeki komutların sıralaması build performansını etkileyebilir.

## Layer Cache Nasıl Optimize Edilir?

Dockerfile hazırlanırken sık değişmeyen işlemleri, sık değişen kaynak kodlardan önce gerçekleştirmek cache kullanımını iyileştirebilir.

Özellikle bağımlılık dosyaları ile uygulama kaynak kodlarının doğru sırayla kopyalanması önemlidir.

Amaç, küçük bir kod değişikliğinde bütün image'ın gereksiz yere baştan oluşturulmasını önlemektir.

# CI ve CD

## CI Nedir?

CI, Continuous Integration yani Sürekli Entegrasyon anlamına gelir.

Geliştiricilerin yaptığı kod değişikliklerinin ortak projeye düzenli olarak dahil edilmesini ve bu değişikliklerin otomatik build ve test işlemlerinden geçirilmesini amaçlar.

Örneğin GitHub'a yeni bir commit gönderildiğinde sistem otomatik olarak projeyi build edip testleri çalıştırabilir.

## CD Nedir?

CD kavramı Continuous Delivery veya Continuous Deployment anlamında kullanılabilir.

Continuous Delivery'de uygulama otomatik olarak build ve test edilir ve dağıtıma hazır hale getirilir. Production ortamına gönderme işlemi manuel onay gerektirebilir.

Continuous Deployment'da ise gerekli kontrollerden başarıyla geçen değişiklikler otomatik olarak production ortamına kadar dağıtılabilir.

## Continuous Delivery ve Continuous Deployment Arasındaki Fark

Continuous Delivery'de yazılım dağıtıma hazır hale getirilir ancak production'a geçişte manuel bir onay bulunabilir.

Continuous Deployment'da ise testleri ve gerekli kontrolleri başarıyla geçen değişiklikler otomatik olarak production ortamına aktarılabilir.

# Git Cherry-Pick

## Cherry-Pick Nedir?

`git cherry-pick`, başka bir branch üzerinde bulunan belirli bir commit'i mevcut branch'e uygulamak için kullanılan Git komutudur.

Normal bir merge işleminde bir branch'teki değişikliklerin bütünü birleştirilebilirken cherry-pick ile ihtiyaç duyulan belirli commit seçilebilir.

Örnek:

git cherry-pick <commit-id>

Bu komut belirtilen commit'in yaptığı değişiklikleri mevcut branch üzerine uygular.

Cherry-pick özellikle başka bir branch'teki tek bir hata düzeltmesini veya belirli bir özelliği almak gerektiğinde kullanılabilir.

# Java Lambda Expression

## Lambda Expression Nedir?

Lambda Expression, Java'da özellikle fonksiyonel arayüzlerle birlikte daha kısa ve okunabilir kod yazılmasını sağlayan bir yapıdır.

Java 8 ile dile eklenmiştir.

Genel yapısı:

(parametreler) -> işlem

Örneğin:

(a, b) -> a + b

Lambda ifadeleri özellikle koleksiyon işlemleri, Stream API ve fonksiyonel programlama yaklaşımında kullanılabilir.

# AIHEXA Projesinde Nerede Kullanılabilir?

Docker, AIHEXA bünyesinde geliştirilen backend uygulamalarının farklı geliştirme, test ve sunucu ortamlarında daha tutarlı biçimde çalıştırılmasına yardımcı olabilir.

Spring Boot uygulaması Docker Image haline getirilerek gerekli Java ortamıyla birlikte paketlenebilir. Böylece uygulamanın farklı sistemlerde kurulması ve çalıştırılması kolaylaştırılabilir.

CI/CD süreçleri ise GitHub repository üzerinde yapılan değişikliklerden sonra projenin otomatik olarak build ve test edilmesi için kullanılabilir. İlerleyen aşamalarda Docker ve CI/CD birlikte kullanılarak yazılımın geliştirme ve dağıtım süreçleri daha düzenli hale getirilebilir.
