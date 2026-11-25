# Modül B — Giyilebilir Komut Arayüzü (Akıllı Eldiven)

**Sorumlular:**  
* Hanifi Emirhan Ağır (Elektronik ve Haberleşme Mühendisliği — EHM): *Eldiven elektroniği, güç katı ve kablosuz haberleşme*  
* Doğa Nur Yıldız (Biyomedikal Mühendisliği — BME): *IMU/EMG sensör seçimi, sinyal işleme, kullanıcı kalibrasyonu ve ergonomi*  

**Hedef Ölçüt:** Ö2 (20 Puan — 4 adet 5 puanlık alt test)

---

## Klasör Yapısı

```text
Moduller/B-GiyilebilirEldiven/
├── README.md          # Bu dosya (Teknik kararlar, Ö2 kriterleri ve ilerleme takibi)
├── docs/              # Devre şemaları, ergonomi tasarımı ve jest/sinyal haritası
├── hardware/          # PCB tasarım dosyaları ve sensör yerleşim şemaları
├── firmware/          # Eldiven mikrodenetleyici ve kablosuz veri iletim kodları
└── calibration/       # Kullanıcı kalibrasyon algoritmaları ve sinyal eşik testleri
```

---

## Teknik Tasarım ve Donanım Seçimleri *(Ekip Üyeleri Tarafından Doldurulacak)*

| Bileşen / Katman | Seçilen Çözüm | Sorumlu | Açıklama ve Seçim Gerekçesi |
| :--- | :--- | :---: | :--- |
| **Algılama Yöntemi** | `[Belirlenecek: IMU / EMG / Esnek Sensör]` | BME | El/kol hareketi veya kas aktivitesi ölçüm yöntemi |
| **Sensör Modelleri** | `[Belirlenecek: Örn. MPU6050 / MyoWare]` | BME / EHM | Sensör yerleşimi ve örnekleme frekansı |
| **Eldiven MCU** | `[Belirlenecek: Örn. ESP32-C3 / Nano]` | EHM | Düşük güç tüketimi ve boyut uyumluluğu |
| **Kablosuz Protokol** | `[Belirlenecek: Örn. ESP-NOW / BLE]` | EHM | Gecikme süresi (&le; 2 s) ve bağlantı kararlılığı |
| **Besleme ve Güvenlik** | `[Belirlenecek: LiPo Pil + Koruma Devresi]` | EHM / BME | Galvanik izolasyon ve kullanıcı güvenliği |

---

## Komut Eşleme Tablosu *(Doldurulacak)*

| Hedef Komut | Sistem Kodu | Operatör El/Kol Hareketi veya Kas Aktivitesi | Eşik / Kalibrasyon Notu |
| :--- | :--- | :--- | :--- |
| **İleri Git** | `FORWARD` | `[Belirlenecek: Örn. El bileği aşağı eğim]` | Yaklaşık 0.5 m ilerleme |
| **Geri Git** | `BACKWARD` | `[Belirlenecek: Örn. El bileği yukarı eğim]` | Yaklaşık 0.5 m geri gitme |
| **Sola Dön** | `LEFT` | `[Belirlenecek: Örn. Bilek sola yatırma]` | Yaklaşık 90&deg; sola dönüş |
| **Sağa Dön** | `RIGHT` | `[Belirlenecek: Örn. Bilek sağa yatırma]` | Yaklaşık 90&deg; sağa dönüş |
| **Dur** | `STOP` | `[Belirlenecek: Örn. Nötr / Düz el konumu]` | Tüm hareket komutlarından sonra aktif |

---

## Değişmez Teknik ve Güvenlik Kuralları

* **Kablosuz Zorunluluğu:** Kabul testinde robot üzerindeki fiziksel düğmeler veya akıllı telefon üzerinden manuel sürüş komutu kullanılamaz.
* **Biyosinyal Güvenliği:** İnsan üzerinde elektriksel biyosinyal (EMG vb.) ölçülecekse yalnızca bataryalı ve şebekeden galvanik olarak ayrılmış düzenekler kullanılmalıdır.
* **Parametre Kilidi:** Test öncesinde kullanıcı kalibrasyonu yapılabilir; ancak final testi başladıktan sonra eşik değerleri veya algoritma değiştirilemez.

---

## Ö2 Kabul Kriterleri ve Geliştirme Kontrol Listesi

### 1. Donanım, Ergonomi ve Kalibrasyon
* [ ] Sensörlerin eldiven üzerine ergonomik ve tekrarlanabilir biçimde yerleştirilmesi
* [ ] Elektriksel izolasyon ve batarya güvenlik kontrollerinin tamamlanması
* [ ] Kullanıcı kalibrasyon rutinlerinin geliştirilmesi ve yanlış tetiklemelerin (false positive) filtrelenmesi
* [ ] Komut paketinin zaman damgasıyla (`last_cmd`, `cmd_ts_ms`) Modül A'ya aktarılması

### 2. Kablosuz Komut Testleri (Ö2 — 20 Puan)
* [ ] **Alt Test 1 (5 Puan):** `FORWARD` (&approx; 0.5 m) + `STOP` komutlarına &le; 2 s içinde dengeli tepki
* [ ] **Alt Test 2 (5 Puan):** `BACKWARD` (&approx; 0.5 m) + `STOP` komutlarına &le; 2 s içinde dengeli tepki
* [ ] **Alt Test 3 (5 Puan):** `LEFT` (&approx; 90&deg;) + `STOP` komutlarına &le; 2 s içinde dengeli tepki
* [ ] **Alt Test 4 (5 Puan):** `RIGHT` (&approx; 90&deg;) + `STOP` komutlarına &le; 2 s içinde dengeli tepki
