# Web Frontend Sürekli Serisi #6

## HTML5 İleri Form Senaryoları

Bugün HTML5 formlarında kullanılan ileri seviye form özellikleri araştırıldı. Formların yalnızca kullanıcıdan veri almak için değil, veriyi düzenli, anlaşılır ve kullanıcı dostu şekilde toplamak için tasarlanması gerektiği öğrenildi.

## Fieldset ve Legend

`fieldset`, bir form içerisindeki birbiriyle ilişkili alanları gruplandırmak için kullanılır.

`legend` ise oluşturulan bu grubun başlığını belirtir.

Örneğin ad, soyad ve e-posta alanları "Kişisel Bilgiler" başlığı altında gruplanabilir. Böylece özellikle uzun formların daha düzenli ve anlaşılır olması sağlanır.

## Autocomplete

`autocomplete`, tarayıcının daha önce girilmiş kullanıcı bilgilerini hatırlayarak form alanlarını otomatik doldurmasına yardımcı olan HTML özelliğidir.

Örneğin ad, e-posta, telefon ve adres gibi bilgilerde kullanılabilir. Kullanıcının formu daha hızlı doldurmasını sağlayarak kullanıcı deneyimini iyileştirir.

## Multiple

`multiple` özelliği, uygun form alanlarında kullanıcının birden fazla değer seçmesine veya girmesine izin verir.

Örneğin bir dosya yükleme alanında `multiple` kullanıldığında kullanıcı aynı anda birden fazla dosya seçebilir.

## File Input

`input type="file"` kullanıcının bilgisayarından bir dosya seçerek forma eklemesini sağlar.

Örneğin bir eğitim başvuru sisteminde kullanıcıdan CV, sertifika veya gerekli bir belge alınması için kullanılabilir.

Birden fazla dosyanın seçilebilmesi için `multiple` özelliği ile birlikte kullanılabilir.

## Kullanıcı Deneyimi Kuralları

Form hazırlanırken kullanıcı deneyiminin iyi olması için:

- Form alanlarının ne istediği açıkça belirtilmelidir.
- İlgili alanlar gruplandırılmalıdır.
- Zorunlu ve isteğe bağlı alanlar kullanıcı tarafından anlaşılabilmelidir.
- Gereksiz form alanlarından kaçınılmalıdır.
- Hata mesajları açık ve anlaşılır olmalıdır.
- Dosya yükleme alanında kabul edilen dosya türleri belirtilmelidir.
- Form mobil cihazlarda da kullanılabilir olmalıdır.
- Gönder butonu kullanıcı tarafından kolayca fark edilebilmelidir.

## Örnek Form Senaryosu

AIHEXA eğitimlerine başvurmak isteyen kullanıcılar için bir başvuru formu düşünülebilir.

Form içerisinde kişisel bilgiler `fieldset` ile gruplandırılabilir. Ad, soyad, e-posta ve telefon bilgileri alınabilir. Kullanıcı katılmak istediği eğitimi seçebilir ve CV veya sertifika gibi belgelerini file input kullanarak sisteme yükleyebilir.

`autocomplete` özelliği sayesinde kullanıcının bazı kişisel bilgileri daha hızlı doldurması sağlanabilir. `multiple` özelliği kullanılarak birden fazla belge yüklenmesine izin verilebilir.

Bu özellikler sayesinde daha düzenli ve kullanıcı dostu bir form oluşturulabilir.

## AIHEXA Projesinde Kullanımı

HTML5'in gelişmiş form özellikleri AIHEXA'nın eğitim başvurusu, iletişim, kayıt ve belge yükleme gibi kullanıcıdan veri alınan bölümlerinde kullanılabilir.

Doğru tasarlanmış formlar kullanıcının işlemlerini daha kolay tamamlamasını sağlarken frontend tarafında daha düzenli bir kullanıcı deneyimi oluşturulmasına yardımcı olur.
