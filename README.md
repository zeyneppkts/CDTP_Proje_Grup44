# İki Tekerlekli Ev İçi Hasta Destek Robotu (YTÜ EEF ÇDTP)

Yıldız Teknik Üniversitesi · Elektrik-Elektronik Fakültesi · Çok Disiplinli Tasarım Projesi (2026-2027 Güz)  
**Danışman:** Doç. Dr. Serkan Kurt (EHM)

---

## Proje Özeti

Bu proje, evde bakım gören veya hareket kabiliyeti kısıtlı bireylere önceden hazırlanmış kapalı bir ilaç kutusunu ev içinde ulaştıran **aktif dengeli iki tekerlekli bir destek robotu** geliştirmeyi amaçlar.

### Dört Adımda Sistem Akışı
1. **Aktif Denge (Modül A):** Robot, harici destek veya üçüncü tekerlek olmadan iki ana tekerlek üzerinde dengede kalır.
2. **Giyilebilir Kontrol (Modül B):** Operatör, eldiven üzerindeki sensörlerle ürettiği 5 komutu (*ileri, geri, sağa dön, sola dön, dur*) kablosuz olarak robota iletir.
3. **Otonom Şarj (Modül C):** Görev sonunda robot, 30 cm yarıçaplı ve 90&deg; yaklaşma bölgesinden itibaren istasyonu IR sensörlerle algılayıp insan müdahalesi olmadan şarjı başlatır.
4. **Görsel İzleme ve Raporlama (Modül D):** Sabit konumlandırılmış akıllı telefon uygulaması, kamera görüntüleri ve 1 Hz telemetri verileriyle görevi ölçer ve değiştirilemez **Geçti / Kaldı** raporu üretir.

---

## Sistem Mimarisi

```text
Kullanıcı ──(el/kol hareketi)──► Eldiven [B] ──(kablosuz komut)──► Denge Robotu [A+E] ◄──(IR)──► Şarj İstasyonu [C]
                                                                       ▲       │
                                                       kamera takibi   │       │ 1-2 Hz telemetri (şarj + eğim)
                                                                       │       ▼
                                                                Akıllı Telefon [D] ──► Sonuç Raporu (Geçti/Kaldı)
```

---

## Ekip ve Görev Dağılımı

| Ad, Soyad | Bölüm | Sorumlu Modül | Ana Sorumluluk Alanı | Klasör |
| :--- | :--- | :--- | :--- | :--- |
| **Sultan Ahmet Güzel** | KOM | Modül A & E | Robot dinamiği, aktif denge kontrolü, hareket ve istasyona otonom yaklaşma | [`Moduller/A-DengeRobotu/`](Moduller/A-DengeRobotu/) |
| **Hanifi Emirhan Ağır** | EHM | Modül B | Eldiven mikrodenetleyici/sensör elektroniği ve kablosuz haberleşme | [`Moduller/B-GiyilebilirEldiven/`](Moduller/B-GiyilebilirEldiven/) |
| **Doğa Nur Yıldız** | BME | Modül B | IMU/EMG sensör seçimi, kullanıcı kalibrasyonu, sinyal işleme ve ergonomi | [`Moduller/B-GiyilebilirEldiven/`](Moduller/B-GiyilebilirEldiven/) |
| **Umut Ali Örs** | ELM | Modül C | Batarya/BMS güç mimarisi, koruma elemanları ve otomatik şarj istasyonu | [`Moduller/C-SarjIstasyonu/`](Moduller/C-SarjIstasyonu/) |
| **Zeynep Pektaş** | BLM | Modül D | Android mobil izleme uygulaması, kamera tabanlı takip ve otomatik raporlama | [`Moduller/D-MobilUygulama/`](Moduller/D-MobilUygulama/) |

---

## Ortak Değerlendirme Kriterleri (5 &times; 20 = 100 Puan)

Her alt test ikili olarak değerlendirilir (**Geçti** = 5 puan, **Kaldı** = 0 puan):

| Kod | Ölçüt | Puan | Minimum Kabul Şartları Özeti |
| :--- | :--- | :---: | :--- |
| **Ö1** | Denge ve Temel Hareket | 20 | 30 s statik denge; 1 m ileri/geri gidip 3 s durma; &plusmn;10&deg; başlangıç eğiminden 10 s içinde toparlanma. |
| **Ö2** | Giyilebilir Eldivenle Komut | 20 | İleri/geri (0.5 m), sağ/sol (90&deg;) ve dur komutlarına 2 s içinde dengeli tepki. |
| **Ö3** | Otomatik Şarj İstasyonu | 20 | 30 cm ve 90&deg; bölgeden otonom yerleşme; 5 s içinde otomatik şarj; donanımsal şarj göstergesi (2 s sönme / 5 s yanma). |
| **Ö4** | Mobil Görsel Değerlendirme | 20 | 1 Hz telemetri (< 2 s kopma); kamera ile hareket/yön tespiti; otomatik değerlendirme ve değiştirilemez kayıt. |
| **Ö5** | Bütünleşik Görev | 20 | İlaç kutusunu düşürmeden taşıma, 5 s bekleme, otonom şarj ve raporlama döngüsünün 4 kez eksiksiz tekrarı. |

### Değişmez Teknik Kurallar
* Robot yalnızca **iki ana teker** üzerinde dengelenir; pasif olarak dik duramaz (&plusmn;10&deg; pasif kararlılık ön testi zorunludur).
* Ana besleme en fazla 24 V DC olmalıdır. Lityum bataryada BMS, motor hattında sigorta/akım koruması ve erişilebilir fiziksel güç kesme anahtarı zorunludur.
* Final değerlendirmesi başladıktan sonra firmware, kontrol parametresi, mekanik yapı veya kablolama değiştirilemez.

---

## Modüller Arası Arayüz Sözleşmesi

Modüller arası veri formatı bu tabloda tek kaynak olarak tutulur. Değişiklik yapılmadan önce ilgili modül sorumlularına bilgi verilir.

| Bağlantı | Aktarılan Veri | Sıklık | Format / Protokol |
| :--- | :--- | :--- | :--- |
| **B &rarr; A** | Komutlar: `FORWARD`, `BACKWARD`, `LEFT`, `RIGHT`, `STOP` | Olay bazlı (&le; 2 s) | ESP-NOW / BLE |
| **C &rarr; A** | Donanımsal şarj sinyali (`CHG/STAT`) | Sürekli | Dijital lojik sinyal (3.3 V) |
| **A &rarr; D** | Eğim açısı (&deg;), şarj durumu, pil (%), son komut | &ge; 1 Hz (hedef 2 Hz) | Wi-Fi SoftAP — WebSocket/UDP JSON |
| **C &harr; A** | İstasyon hizalama ve yaklaşma sinyali | Sürekli | IR alıcı-verici sensör dizisi |

### Ortak Telemetri Paketi (`A -> D`)
```json
{
  "ts_ms": 1732531200000,
  "pitch_deg": 1.4,
  "charging": false,
  "battery_pct": 86,
  "last_cmd": "FORWARD",
  "cmd_ts_ms": 1732531199200
}
```

---

## Dönem İçi İlerleme Takibi (14 Hafta)

- [x] **Hafta 1–2:** Ekip organizasyonu, senaryo, rol paylaşımı ve sistem mimarisi
- [ ] **Hafta 2–4:** Parça seçimi, mekanik/güç tasarımı, eldiven ve mobil uygulama iskeleti
- [ ] **Hafta 4–7:** Robot alt sistemleri ve ilk aktif denge; şarj istasyonu ve eldiven prototipleri
- [ ] **Hafta 7–10:** Hareket kontrolü, giyilebilir komutlar, mobil görsel takip ve ölçüm
- [ ] **Hafta 10–12:** İlaç kutusu taşıma, şarj istasyonu yerleşimi, bütünleşik görev entegrasyonu
- [ ] **Hafta 12–13:** Ortak test protokolüyle prova, hata giderme ve sonuç kaydı
- [ ] **Final Haftası:** Proje Pazarı canlı 5 &times; 20 değerlendirme ve ekip sunumu

---

## GitHub Çalışma Kuralları

1. **Klasör Sahipliği:** Her ekip üyesi yalnızca kendi modül klasörü (`Moduller/<ModulAdi>/`) altında geliştirme yapar.
2. **Branch Düzeni:** Doğrudan `main` dalına push yapılmaz. Özellik dalları `<modül-kodu>/<kısa-açıklama>` biçiminde açılır (ör. `D/kamera-aruco-takip`, `A/pid-denge`, `B/eldiven-imu`, `C/ir-dock`).
3. **Commit Mesajları:** Mesaj başında ilgili modül kodu belirtilir: `[D] 1 Hz telemetri dinleyici eklendi`.
4. **Büyük Dosyalar:** Ham test videoları, derleme çıktıları (`.apk`, `.bin`) ve büyük 3B CAD dosyaları repoya yüklenmez; `.gitignore` ile hariç tutulur.
5. **İlerleme Takibi:** Her modül sorumlusu kendi klasöründeki `README.md` içinde yer alan görev listesini (checkbox) haftalık olarak günceller.
