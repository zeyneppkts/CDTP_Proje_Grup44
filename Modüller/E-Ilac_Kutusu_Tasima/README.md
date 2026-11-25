# Modül E — İlaç Kutusu Taşıma ve Bütünleşik Görev Entegrasyonu

**Sorumlu:** Tüm Ekip *(Entegrasyon Koordinasyonu: Sultan Ahmet Güzel — KOM)*  
**Hedef Ölçüt:** Ö5 (20 Puan — 4 adet 5 puanlık tam görev tekrarı)

---

## Klasör Yapısı

```text
Moduller/E-IlacKutusuTasima/
├── README.md          # Bu dosya (Yük taşıma kuralları, Ö5 senaryosu ve prova kayıtları)
├── docs/              # Bütünleşik görev akış diyagramı ve final test kontrol listesi
├── carrier_cad/       # İlaç kutusu taşıyıcı hazne CAD dosyaları
└── test_logs/         # Dönem sonu 4x bütünleşik görev prova özetleri
```

---

## Yük ve Taşıyıcı Tasarım Kuralları *(Ekip Tarafından Doldurulacak)*

| Parametre | Teknik Şartname Kuralı | Ekip Tasarım Çözümü |
| :--- | :--- | :--- |
| **Görev Yükü** | Kapalı, kuru, kırılmaz standart bir ilaç (aspirin vb.) kutusu | `[Belirlenecek: Kutu boyutları ve ağırlığı]` |
| **Sabitleme Kısıtı** | Kutu görev boyunca düşmemeli; **ancak gövdeye vidalanmamalı/yapıştırılmamalı** | `[Belirlenecek: Hazne / kenarlıklı tabla tasarımı]` |
| **Konumlandırma** | Kamera görüşünü (ArUco) ve IR şarj sensörlerini kapatmamalı | `[Belirlenecek: Robot üzerindeki montaj yüksekliği]` |

---

## Bütünleşik Görev Senaryosu (Uçtan Uca Akış)

1. **Başlangıç:** Robot, taşıyıcı haznesinde standart ilaç kutusu ile dengede bekler; mobil uygulama (Modül D) kamera kaydını ve 1 Hz telemetri izlemesini başlatır.
2. **Hedefe Ulaşım:** Operatör, giyilebilir eldiven (Modül B) komutlarıyla robotu ev içi hedef noktaya yönlendirir.
3. **Teslimat Beklemesi:** Robot hedef noktada ilaç kutusunu düşürmeden 5 s boyunca aktif dengede bekler.
4. **Otonom Şarj:** Robot şarj istasyonunun 30 cm yarıçaplı / 90&deg; yaklaşma bölgesine girer; insan müdahalesi olmadan istasyonu bulur, yerleşir ve &le; 5 s içinde şarjı başlatır.
5. **Otomatik Raporlama:** Mobil uygulama tüm süreci doğrular ve değiştirilemez **Geçti / Kaldı** sonuç raporunu üretir.

---

## Ö5 Kabul Kriterleri ve Prova Kontrol Listesi (20 Puan)

Final değerlendirmesinde aynı bütünleşik görev birbirinden bağımsız **4 kez** uygulanır. Her eksiksiz görev 5 puan, kutunun düşmesi, robotun devrilmesi veya insan müdahalesi durumunda 0 puan alır:

* [ ] **Prova / Alt Test 1 (5 Puan):** Tam bütünleşik görev + otomatik mobil rapor kaydı
* [ ] **Prova / Alt Test 2 (5 Puan):** Tam bütünleşik görev + otomatik mobil rapor kaydı
* [ ] **Prova / Alt Test 3 (5 Puan):** Tam bütünleşik görev + otomatik mobil rapor kaydı
* [ ] **Prova / Alt Test 4 (5 Puan):** Tam bütünleşik görev + otomatik mobil rapor kaydı

> **Final Test Bütünlüğü Kuralı:** Final değerlendirmesi başladıktan sonra firmware, kontrol parametreleri, mekanik yapı veya kablolama değiştirilemez; yalnızca sistemi yeniden başlatma ve aynı tip batarya değişimi yapılabilir.
