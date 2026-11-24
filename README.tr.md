> 🚧 **Durum: aktif geliştirme.** Yazarın kullanımında çalışır; arayüzler değişebilir.

[English](README.md)

# KarıncaAI — Lightpanda görev kılavuzu

Lightpanda'nın ne olduğunu, hangi görevlerde kullanılabileceğini ve Chrome/auto yolunun ne zaman korunması gerektiğini anlatan Türkçe başlangıç rehberi.

## İçerik

- `index.html`: yayın sayfası
- `assets/b2-signature.png`: KarıncaAI B2 imzası
- `assets/benchmark-vps.png`: KarıncaAI VPS benchmark grafiği
- `assets/social-preview.png`: X/link paylaşımı için 1200×628 sosyal önizleme kartı
- `benchmark-report.md`: yöntem, sonuç ve sınırlar
- `benchmark-data.json`: ham benchmark verisi

## Test durumu

Ölçümler 30 Ağustos 2026 tarihlidir; Lightpanda nightly ve Chrome 146 sürümleriyle yapılmıştır. Aşağıdaki sayfa denetimleri yayın sırasında yapıldı, CI ile yeniden koşulmaz.

- 320 / 390 / 1440 px: PASS
- JavaScript kapalı statik karar yolu: PASS
- Klavye Enter ve sonuç odağı: PASS
- Lightpanda/Chrome yerel VPS A/B: 20/20 + 20/20 başarı
- Gerçek internet sitesi ve görsel özellik uyumluluğu: kapsam dışı

Bu depo ve site otomatik X yayını yapmaz.

## Lisans

Yazılı rehber ve ölçüm raporu CC-BY-4.0 (`LICENSE`); kod (sayfa betiği ve varsa kod parçaları) Apache-2.0 (`LICENSE-APACHE-2.0`). Telif (c) 2026 Kemal (KarıncaAI). Güvenlik: `SECURITY.md`.
