# Modül A — İki Tekerlekli Aktif Denge Robotu

**Sorumlu:** Sultan Ahmet Güzel (Kontrol ve Otomasyon Mühendisliği — KOM)  
**Hedef Ölçüt:** Ö1 (20 Puan — 4 adet 5 puanlık alt test)

---

## Klasör Yapısı

```text
Moduller/A-DengeRobotu/
├── README.md          # Bu dosya (Teknik kararlar, Ö1 kriterleri ve ilerleme takibi)
├── docs/              # Dinamik modelleme, kontrol blok diyagramları ve bağlantı şemaları
├── simulations/       # MATLAB / Simulink / Python kontrolcü ve sistem analiz dosyaları
├── cad/               # Robot şasi ve mekanik tasarım dosyaları
└── firmware/          # Mikrodenetleyici denge, hareket ve telemetri gömülü kodları
```

---

## Teknik Tasarım ve Donanım Seçimleri *(Ekip Üyesi Tarafından Doldurulacak)*

| Bileşen / Katman | Seçilen Çözüm | Açıklama ve Seçim Gerekçesi |
| :--- | :--- | :--- |
| **Kontrol Algoritması** | `[Belirlenecek: Örn. PID / LQR / Kayma Kipli]` | Denge ve konum kontrolü için seçilen yöntem |
| **Ana Mikrodenetleyici** | `[Belirlenecek: Örn. ESP32 / STM32]` | Kontrol döngüsü frekansı ve telemetri yönetimi |
| **IMU Sensörü** | `[Belirlenecek: Örn. MPU6050 / BNO055]` | Gövde eğim açısı ve açısal hız ölçümü |
| **Tahrik Motorları** | `[Belirlenecek: Redüktörlü DC + Enkoder]` | Tork, devir (RPM) ve enkoder çözünürlüğü |
| **Motor Sürücü** | `[Belirlenecek: Çift yönlü sürücü kartı]` | Akım kapasitesi ve PWM kontrol arayüzü |
| **Şasi ve Mekanik** | `[Belirlenecek: 3B Baskı / Pleksi / Alüminyum]` | Ağırlık merkezi yerleşimi ve tekerlek çapı |

---

## Değişmez Teknik ve Güvenlik Kuralları

* **Mekanik Düzen:** Robot puanlanan testlerde yalnızca iki aynı eksenli ana teker üzerinde dengelenir; üçüncü tekerlek, ayak veya yere temas eden pasif destek kullanılamaz.
* **Pasif Kararlılık Ön Testi:** Motorlar enerjisizken robot +10&deg; ve -10&deg; gövde eğiminden ayrı ayrı bırakıldığında 5 s içinde dik konuma geri dönmemeli veya pasif olarak dik kalmamalıdır.
* **Güç ve Güvenlik:** Ana besleme en fazla 24 V DC olmalıdır. Erişilebilir fiziksel acil güç kesme anahtarı, BMS korumalı batarya ve motor hattında sigorta/akım koruması zorunludur.
* **Hazır Kit Yasağı:** Hazır çalışan kendini dengeleyen robot kiti ana platform olarak kullanılamaz.

---

## Ö1 Kabul Kriterleri ve Geliştirme Kontrol Listesi

### 1. Mekanik ve Gömülü Altyapı
* [ ] Robot şasisinin üretimi ve iki ana tekerlek/motor montajının tamamlanması
* [ ] Pasif kararlılık ön testinin (&plusmn;10&deg; bırakıldığında 5 s içinde devrilme) doğrulanması
* [ ] Modül D (Mobil Uygulama) kamerası için gövdede 8 &times; 8 cm düz ArUco işaretçi alanı ayrılması

### 2. Denge ve Temel Hareket Testleri (Ö1 — 20 Puan)
* [ ] **Alt Test 1 (5 Puan):** Harici destek olmadan 30 s boyunca kesintisiz statik denge
* [ ] **Alt Test 2 (5 Puan):** Yaklaşık 1 m ileri hareket edip hedef noktada 3 s dengede durma
* [ ] **Alt Test 3 (5 Puan):** Yaklaşık 1 m geri hareket edip başlangıç noktasında 3 s dengede durma
* [ ] **Alt Test 4 (5 Puan):** +10&deg; ve -10&deg; başlangıç eğimlerinin ikisinden de 10 s içinde toparlanma

### 3. Modüller Arası Entegrasyon
* [ ] Modül B'den (Eldiven) gelen 5 komutun (`FORWARD`, `BACKWARD`, `LEFT`, `RIGHT`, `STOP`) &le; 2 s içinde uygulanması
* [ ] Modül C (Şarj İstasyonu) 30 cm yarıçaplı ve 90&deg; yaklaşma bölgesinde IR sensörlerle otonom hizalanma ve dock'a yerleşme
* [ ] Modül D'ye (Telefon) &ge; 1 Hz sıklığında `pitch_deg`, `charging`, `battery_pct` ve `last_cmd` verilerinin JSON formatında gönderilmesi
