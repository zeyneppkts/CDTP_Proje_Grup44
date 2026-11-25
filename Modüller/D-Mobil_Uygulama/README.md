# Modül D — Mobil İzleme ve Değerlendirme Uygulaması

**Sorumlu:** Zeynep Pektaş (Bilgisayar Mühendisliği)  
**Hedef Ölçüt:** Ö4 (20 Puan — 4 adet 5 puanlık alt test)

---

## Klasör Yapısı

```text
Moduller/D-MobilUygulama/
├── README.md                  # Bu dosya (Ö4 kabul kriterleri ve ilerleme takibi)
├── docs/
│   ├── telemetry_schema.json  # Robot ile ortak JSON veri sözleşmesi
│   └── aruco_markers/         # Basılıp robota yapıştırılacak 4x4 ArUco işaretçileri
├── prototype_pc/              # Python + OpenCV masaüstü takip ve telemetri simülatörü
├── mobile_app/                # Native Android (Kotlin + Jetpack Compose + CameraX + OpenCV)
└── sample_reports/            # SHA-256 imzalı örnek test raporu çıktıları
```

---

## Ö4 Kabul Kriterleri ve Geliştirme Kontrol Listesi

### 1. Kablosuz Telemetri (5 Puan)
* [ ] Robot SoftAP ağına WebSocket/UDP istemcisi ile bağlanma
* [ ] En az 1 Hz sıklığında `pitch_deg`, `charging` ve `battery_pct` verilerini canlı gösterme
* [ ] Bağlantı kesintisinin 2 s sınırını aşması durumunda görsel uyarı ve log kaydı üretme

### 2. Kamera Tabanlı Görsel Takip (5 Puan)
* [ ] CameraX ile sabit telefon kamerasından canlı görüntü akışı alma
* [ ] OpenCV ArUco (`DICT_4X4_50`) tespiti ile piksel-santimetre kalibrasyonu yapma
* [ ] Robot hareket durumunu (*hareket ediyor*, *dönüyor*, *statik denge*, *toparlanma*) canlı sınıflandırma
* [ ] Hareket yönünü (*ileri*, *geri*, *sağ*, *sol*) anlık ekranda gösterme

### 3. Otomatik Görev Değerlendirme (5 Puan)
* [ ] Arayüzden 4 görev modunu seçebilme (*Statik Denge*, *Robot Yönlendirmesi*, *Şarj Etme*, *Bütünleşik Görev*)
* [ ] "Testi Başlat" / "Testi Bitir" zaman damgası ve otomatik bitiş koşulu tespiti
* [ ] Süre (&plusmn;2 s tolerans), mesafe (0.8–1.2 m), yön ve şarj sinyaline göre otomatik **Geçti / Kaldı** kararı

### 4. Değiştirilemez Raporlama (5 Puan)
* [ ] Test süresince eşzamanlı MP4 video kaydı alma
* [ ] Test sonunda JSON/PDF sonuç raporu oluşturma ve `SHA-256` özetini `Android Keystore` ile mühürleme
* [ ] Rapor ekranında bütünlük doğrulama (elle müdahale edilmediğinin kanıtı) gösterme

---

## GitHub Deposu Hızlı Kurulum Adımları

Yeni GitHub reponuzu oluşturduktan sonra yerel bilgisayarınızda tüm ekip klasörlerini tek seferde oluşturmak için aşağıdaki komutları çalıştırabilirsiniz:

```bash
git clone <REPO_URL>
cd <REPO_KLASORU>

mkdir -p Moduller/A-DengeRobotu
mkdir -p Moduller/B-GiyilebilirEldiven
mkdir -p Moduller/C-SarjIstasyonu
mkdir -p Moduller/D-MobilUygulama/{docs/aruco_markers,prototype_pc,mobile_app,sample_reports}
mkdir -p Moduller/E-IlacKutusuTasima
mkdir -p ProjeTanimSunum

touch Moduller/A-DengeRobotu/README.md
touch Moduller/B-GiyilebilirEldiven/README.md
touch Moduller/C-SarjIstasyonu/README.md
touch Moduller/E-IlacKutusuTasima/README.md

git add .
git commit -m "[Genel] Proje klasor yapisi ve ortak arayuz dokumanlari olusturuldu"
git push origin main
```
