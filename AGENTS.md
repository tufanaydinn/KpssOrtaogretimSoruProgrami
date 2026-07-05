# KPSS Ortaöğretim Soru Programı — Yapay Zekâ Çalışma Talimatları

Bu depo **KPSS Ortaöğretim (GY-GK)** çalışma aracıdır. Bu dosya, depoda çalışan **her yapay zekâ asistanı için bağlayıcıdır**. Soru eklemeden veya değişiklik yapmadan önce bu dosyanın tamamını uygula.

> **ÖNEMLİ — SEVİYE:** Bu depo **ORTAÖĞRETİM (lise)** düzeyi içindir. Sorular, Lisans deposundakinden **belirgin biçimde daha kolay** olmalıdır: kısa paragraflar, temel işlemler, doğrudan bilgi. Lisans deposundaki (`KpssSoruProgrami`) soruların aynısı ya da benzeri buraya kopyalanmaz — burada her şey sıfırdan, ortaöğretim seviyesine göre yazılır.

## Proje Yapısı

- Tarayıcıda çalışan, kurulumsuz, internetsiz uygulama: `index.html` + `js/app.js` + `css/style.css`
- Sorular: `sorular/*.js` dosyalarında, `window.KPSS_BANK["<ders>"]` dizisine push edilir
- `sorular/manifest.js`: programın yükleyeceği dosyaların listesi — burada olmayan dosya görünmez
- Kullanıcı verileri (netler, yanlış defteri) tarayıcı localStorage'ındadır; depoda veri YOKTUR
- Yerel çalıştırmada (KPSS-Baslat.bat → localhost:8347, `sunucu.js`) ilerleme verisi `veri/ilerleme.json` dosyasına da yansıtılır (git'e girmez, kişiseldir)

## Soru Ekleme Prosedürü (sırayla, adım atlamadan)

1. `git pull` — önce varsa uzaktaki değişiklikleri al.
2. Dosya adı: `<ders>-<sıra>.js` (örn. `turkce-1.js`). Soru id'leri: `<önek>-<numara>` (örn. `tur-001`).
3. **KOPYA KONTROLÜ (zorunlu):** Yeni soru yazmadan önce o dersin depodaki TÜM soru dosyalarını oku. Aynı bilgiyi, aynı paragrafı, aynı kurguyu soran soru üretme.
4. Formata birebir uy (aşağıda). Tam 5 şık, tek savunulabilir doğru cevap, `dogru` alanı 0-4 indeks (0=A … 4=E), `aciklama` zorunlu.
5. `konu` alanına aşağıdaki listeden BİREBİR bir ad yaz — serbest konu adı uydurma (konu analizi bozulur).
6. Dosya adını `sorular/manifest.js` listesinin **sonuna** ekle.
7. **`node dogrula.js` çalıştır** — hata (❌) ve benzerlik uyarısı (⚠️) sıfır olana kadar düzelt.
8. Commit + `git push`. Push reddedilirse: `git pull --rebase` sonra tekrar push.

## Soru Formatı

```js
window.KPSS_BANK = window.KPSS_BANK || {};
(window.KPSS_BANK["turkce"] = window.KPSS_BANK["turkce"] || []).push(
{ id: "tur-001", konu: "Paragraf", soru: "Soru metni?", secenekler: ["A", "B", "C", "D", "E"], dogru: 2, aciklama: "Gerekçe + kritik çeldirici neden yanlış." }
);
```

- String içindeki çift tırnaklar `\"` ile kaçırılır; her soru tek satırda yazılır; görsel/şekil gerektiren soru eklenmez (her şey metinle ifade edilmeli).

## Ders Anahtarları, id Önekleri ve Geçerli Konu Adları

| Ders anahtarı | id öneki | Geçerli `konu` adları |
|---|---|---|
| `turkce` | `tur` | Paragraf, Cümlede Anlam, Sözcükte Anlam, Dil Bilgisi, Yazım Kuralları, Noktalama, Anlatım Bozukluğu, Sözel Mantık |
| `matematik` | `mat` | Temel Kavramlar ve Sayılar, Denklem ve Oran-Orantı, Problemler, Küme-Fonksiyon-İşlem, Permütasyon-Kombinasyon-Olasılık, Sayısal Mantık, Geometri |
| `tarih` | `tar` | İslamiyet Öncesi Türk Tarihi, İlk Türk-İslam Devletleri, Osmanlı Kuruluş ve Yükselme, Osmanlı Kültür ve Medeniyeti, Osmanlı Dağılma ve Islahatlar, Millî Mücadele Hazırlık Dönemi, Kurtuluş Savaşı, Atatürk İlke ve İnkılapları, Atatürk Dönemi İç ve Dış Politika, Çağdaş Türk ve Dünya Tarihi |
| `cografya` | `cog` | Coğrafi Konum, İklim ve Bitki Örtüsü, Yer Şekilleri, Nüfus ve Yerleşme, Tarım ve Hayvancılık, Madenler, Enerji ve Sanayi, Ulaşım, Ticaret ve Turizm, Bölgeler ve Projeler |
| `vatandaslik` | `vat` | Hukukun Temel Kavramları, Anayasa ve Genel Esaslar, Temel Hak ve Ödevler, Yasama, Yürütme, Yargı ve İdare |
| `guncel` | `gun` | Uluslararası Gündem, Türkiye Gündemi, Bilim ve Teknoloji, Spor, Kültür-Sanat, Ödüller |

Gerçek sınav dağılımı (Ortaöğretim GY-GK, 120 soru): Türkçe 30, Matematik 30, Tarih 27, Coğrafya 18, Vatandaşlık 9, Güncel 6.

## Kalite Standartları (sınav başarısı buna bağlı)

- **Seviye: ORTAÖĞRETİM (lise) düzeyi.** Sorular Lisans'a göre daha kolay olmalı: kısa ve sade paragraflar, temel matematik işlemleri, doğrudan ve tek adımlı bilgi soruları. Karışım ~%40 kolay, %45 orta, %15 zor. Ne aşırı kolay ne de Lisans düzeyinde zor — ölçüt, ortaöğretim (lise mezunu) adayının seviyesidir.
- **Faktüel doğruluk kritik:** Emin olunmayan bilgi soruya çevrilmez. Tarih/coğrafya bilgileri kesin doğru olmalı; şüphede web'den doğrula.
- **Vatandaşlık:** Yalnızca 2017 anayasa değişikliği SONRASI sistem (600 milletvekili, Cumhurbaşkanlığı Hükûmet Sistemi, CB kararnamesi, HSK 13 üye, AYM 15 üye).
- **Matematik:** Her soruyu yazdıktan sonra baştan çöz; cevabın şıklarda tam bir kez geçtiğini ve `dogru` indeksinin onu gösterdiğini doğrula. Açıklamalar adım adım öğretici yazılır.
- **Güncel Bilgiler:** Yalnızca web'den doğrulanmış, son ~18 ayın olayları.
- ÖSYM üslubu; çeldiriciler makul; doğru cevabın şık konumu dengeli dağıtılır (her şıkka ~%20).
- Açıklama: doğru cevabın gerekçesi + en güçlü çeldiricinin neden yanlış olduğu (kavram/terim testinde 1 cümlelik tanım da eklenir).

## Sık Yapılan Hatalar

- `dogru` alanına harf yazmak → SAYI olmalı (0=A … 4=E)
- Son sorudan sonra virgül bırakmak → sözdizimi hatası, dosya hiç yüklenmez
- Dosyayı yazıp `manifest.js`'e eklememek → program soruyu görmez
- `node dogrula.js` çalıştırmadan push etmek → bozuk dosya gider
