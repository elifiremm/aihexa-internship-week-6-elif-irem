# Docker Compose Startup Notu

## Uygulamayı Başlatma

Projeyi Docker Compose ile başlatmak için:

docker compose up -d --build

Bu komut gerekli Docker image'ını oluşturur ve servisleri arka planda çalıştırır.

## Servisleri Kontrol Etme

docker compose ps

Bu komut çalışan servisleri ve healthcheck durumlarını gösterir.

Frontend servisinin durumu `healthy` olduğunda uygulama kullanıma hazırdır.

## Uygulamaya Erişim

Frontend uygulamasına tarayıcı üzerinden:

http://localhost:8080

adresinden erişilebilir.

## Uygulamayı Durdurma

docker compose down

Bu komut Compose tarafından oluşturulan container ve network yapılarını durdurur ve kaldırır.

## Healthcheck

Frontend servisi belirli aralıklarla kontrol edilir. Container'ın yalnızca çalışıyor olması yeterli değildir. Healthcheck sonucunun `healthy` olması uygulamanın isteklere başarılı şekilde cevap verdiğini gösterir.
