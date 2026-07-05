# 🤝 Soru Ekleme Rehberi (Ortaöğretim)

Bu depoya soru eklerken **ortaöğretim (lise) seviyesini** koru: kısa paragraflar, temel işlemler, doğrudan bilgi. Sorular Lisans deposundakinden daha kolay olmalıdır.

## Altın Kurallar

1. Dosya adı: `<ders>-<sıra>.js` (örn. `turkce-1.js`). Soru id'leri: `<önek>-<numara>` (örn. `tur-001`).
2. Dosyanı ekledikten sonra adını `sorular/manifest.js` listesinin **sonuna** ekle.
3. Göndermeden önce proje klasöründe `node dogrula.js` çalıştır (🎉 = tamam; ❌/⚠️ = düzelt).

## Soru Dosyası Formatı

```js
window.KPSS_BANK = window.KPSS_BANK || {};
(window.KPSS_BANK["turkce"] = window.KPSS_BANK["turkce"] || []).push(
{ id: "tur-001", konu: "Paragraf", soru: "Soru metni?", secenekler: ["A şıkkı", "B şıkkı", "C şıkkı", "D şıkkı", "E şıkkı"], dogru: 2, aciklama: "Neden C olduğunun açıklaması." }
);
```

- `dogru`: doğru şıkkın sırası — **0=A, 1=B, 2=C, 3=D, 4=E**
- Tam **5 şık** olmalı; sorular arasına **virgül** konur (son sorudan sonra virgül yok)
- Metin içinde çift tırnak kullanılacaksa `\"` yazılır

## Ders Anahtarları

| Ders | anahtar | id öneki |
|---|---|---|
| Türkçe | `turkce` | `tur-001` |
| Matematik | `matematik` | `mat-001` |
| Tarih | `tarih` | `tar-001` |
| Coğrafya | `cografya` | `cog-001` |
| Vatandaşlık | `vatandaslik` | `vat-001` |
| Güncel Bilgiler | `guncel` | `gun-001` |

Ayrıntılı kurallar için **CLAUDE.md / AGENTS.md** dosyalarına bak. Yapay zekâ asistanı kullanıyorsan bu dosyaları otomatik okur: ortaöğretim seviyesinde, kopya üretmeden soru yazar, `node dogrula.js` ile kontrol eder.
