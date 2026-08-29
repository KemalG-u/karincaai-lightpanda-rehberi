# KarıncaAI VPS testi — Lightpanda ve Chrome

**Tarih:** 30 Ağustos 2026  
**Durum:** Gerçek motor testi tamamlandı  
**Ham veri:** `results-vps-2026-08-30.json`  
**Telemetri:** `LIGHTPANDA_DISABLE_TELEMETRY=true`

## Sonuç

Aynı VPS'te, aynı 20 yerel JavaScript sayfasında iki motor da `20/20` görevi tamamladı.

| Ölçü | Lightpanda | Chrome | Fark |
|---|---:|---:|---:|
| Medyan gezinme + DOM hazır süresi | 4,992 ms | 25,129 ms | Chrome / Lightpanda: **5,034×** |
| p95 süre | 7,688 ms | 28,709 ms | — |
| Başlangıç toplam process-tree RSS | 24,176 MB | 1.000,809 MB | Chrome / Lightpanda: **41,397×** |
| Medyan tepe toplam RSS | 24,166 MB | 1.064,732 MB | — |
| Medyan ek RSS | ölçüm gürültüsü içinde | 63,924 MB | — |
| Görev başarısı | 20/20 | 20/20 | eşit |

## Sade yorum

Bu küçük, metin/DOM ağırlıklı ve sürekli açık motor testinde Lightpanda aynı görevi tamamladı; medyan navigasyon süresi Chrome'un yaklaşık beşte biri oldu. Lightpanda tek süreçte yaklaşık 24 MB düzeyinde kalırken izole Chrome örneği 12 süreç ve yaklaşık 1 GB başlangıç belleği kullandı.

Bu sonuç, “Lightpanda her sitede 5× hızlı ve 41× az RAM kullanır” anlamına gelmez. Yalnız aşağıdaki kontrollü test için geçerlidir.

*Teknik karşılığı: Persistent CDP engine benchmark; navigation-to-observable-DOM latency ve process-tree RSS karşılaştırması.*

## Test ortamı

- VPS: Ubuntu Linux, x86_64
- Toplam RAM: 15 GiB
- Google Chrome: 146.0.7680.164
- Lightpanda: `1.0.0-nightly.9002+1bd957d07`
- Lightpanda binary SHA-256: `bcf58eddf63892c1d735d53a14b7d54a21f3970ef5fd362dbe6fd6fc3789583b`
- Lightpanda portu: yalnız localhost `127.0.0.1:9223`
- Chrome portu: yalnız localhost `127.0.0.1:9224`
- Her iki motor: ayrı, sürekli açık CDP süreci
- Test sunucusu: yalnız localhost `127.0.0.1:8877`

## Görev

20 deterministik HTML sayfası üretildi. Her sayfa JavaScript ile 200 DOM öğesi oluşturdu ve tamamlandığında:

```html
<body data-ready="true">
```

işaretini ekledi.

Her iki motor aynı URL'lere yönlendirildi. Süre, şu 2 koşul birlikte sağlanınca durduruldu:

```text
data-ready == true
#items li sayısı == 200
```

Motor sırası her turda değiştirildi:

```text
tek turlar: Lightpanda → Chrome
çift turlar: Chrome → Lightpanda
```

Böylece sürekli ilk çalışan motor avantajı azaltıldı.

## Bellek ölçümü

Yalnız ana PID değil, ana süreç ve bütün alt süreçlerin RSS toplamı örneklendi. Örnekleme aralığı 5 ms idi.

Chrome başlangıçta 12 süreç kullanıyordu. Lightpanda tek süreçti.

Lightpanda'nın medyan tepe RSS değeri başlangıç değerinden 0,010 MB düşük görünüyor. Bu gerçek bir negatif bellek tüketimi değildir; işletim sistemi sayfalaması ve 5 ms örnekleme gürültüsüdür. Bu nedenle Lightpanda için “0 MB ek bellek” iddiası kullanılmayacak. Güvenilir ifade:

> Test boyunca Lightpanda toplam RSS değeri yaklaşık 24 MB düzeyinde kaldı.

## Sınırlar

1. Sayfalar yereldi; internet gecikmesi yoktu.
2. Sayfalar kontrollü ve basitti; gerçek sitelerin bütün JavaScript/API karmaşıklığını temsil etmiyor.
3. Motorlar sürekli açıktı; bu test cold-start binary süresi değildir.
4. Chrome 146 ve Lightpanda nightly sürümü karşılaştırıldı.
5. Screenshot, PDF, dosya yükleme, kalıcı profil, giriş akışı ve bot koruması sınanmadı.
6. Chrome'un yaklaşık 1 GB başlangıç RSS'i bu izole Chrome sürecinin process-tree toplamıdır; başka ortamlarda farklı olabilir.
7. Test, Lightpanda üreticisinin 933 sayfalık benchmarkını tekrar etmez.
8. Sonuç yalnız “tekrarlanan metin/DOM işi için kontrollü pilot mantıklı” hükmünü destekler.

## Karar

KarıncaAI'nin önceki hükmü artık güncellendi:

- Önceki: `Gerçek A/B testi yapılmadı.`
- Şimdi: `Yerel kontrollü A/B testi tamamlandı; 20/20 başarı, 5,034× medyan süre farkı ve yaklaşık 24 MB / 1.001 MB başlangıç process-tree RSS gözlendi.`

Yine de genel öneri değişmiyor:

- Metin/DOM ağırlıklı tekrarlı görev: Lightpanda pilotu mantıklı.
- Screenshot/PDF/dosya/kalıcı profil/tam uyum kritikse: Chrome/auto yolunu koru.
- Gerçek üretim kararı: kendi URL kümesi ve başarı ölçütüyle ayrıca sınanmalı.

## Yeniden üretme

- Sayfa üretici: `site/generate_pages.py`
- Cold-start denemesi: `run_benchmark.py` — VPS Chrome `--dump-dom` modunda VAAPI/shared-memory hatasıyla zaman aşımına uğradı; nihai veri olarak kullanılmadı.
- Başarılı yöntem: `run_cdp_benchmark.py`
- Ham sonuç: `results-vps-2026-08-30.json`
