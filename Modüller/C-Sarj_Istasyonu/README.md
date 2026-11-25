# Modül C — Güç Mimarisi ve Otomatik Şarj İstasyonu

**Sorumlu:** Umut Ali Örs (Elektrik Mühendisliği — ELM)  
**Hedef Ölçüt:** Ö3 (20 Puan — 4 adet 5 puanlık alt test)

---

## Klasör Yapısı

```text
Moduller/C-SarjIstasyonu/
├── README.md          # Bu dosya (Teknik kararlar, Ö3 kriterleri ve ilerleme takibi)
├── docs/              # Güç bütçesi, koruma elemanları hesabı ve test ölçümleri
├── schematics/        # Batarya BMS, regülatör, şarj devresi ve IR sensör şemaları
└── cad_dock/          # Şarj istasyonu mekanik dock ve temas arayüzü tasarımları
```

---

## Teknik Tasarım ve Donanım Seçimleri *(Ekip Üyesi Tarafından Doldurulacak)*

| Bileşen / Katman | Seçilen Çözüm | Açıklama ve Seçim Gerekçesi |
| :--- | :--- | :--- |
| **Batarya Paketi** | `[Belirlenecek: Örn. 3S / 4S Li-Ion veya LiFePO4]` | Nominal voltaj (&le; 24 V DC) ve kapasite (mAh) |
| **Koruma ve BMS** | `[Belirlenecek: BMS kartı + Sigorta + Anahtar]` | Aşırı akım, kısa devre ve acil güç kesme mimarisi |
| **Şarj Devresi / IC** | `[Belirlenecek: CC/CV Şarj Kontrolcüsü]` | `CHG/STAT` donanımsal durum çıkışı özellikli |
| **Enerji Aktarım Arayüzü** | `[Belirlenecek: Yaylı Pogo Pin / Temas Pedi / Endüktif]` | Hizalama toleransı ve temas direnci çözümü |
| **İstasyon Algılama (IR)** | `[Belirlenecek: IR Verici/Alıcı Sensör Dizisi]` | 30 cm yarıçap ve 90&deg; yaklaşma bölgesi kapsaması |

---

## Değişmez Teknik ve Güvenlik Kuralları

* **Otonom Şarj:** Şarj aşamasında insan tarafından kablo takılması, anahtar çevrilmesi veya robotun elle istasyona itilmesi/yerleştirilmesi kesinlikle yasaktır.
* **Motor Durdurma:** Robot istasyona yerleşip stabil hale geldikten sonra tahrik motorları durdurulmalıdır.
* **Donanımsal Şarj Göstergesi:** Robot veya istasyon üzerinde görünür bir şarj göstergesi bulunmalıdır. Bu gösterge **doğrudan şarj devresinin donanımsal `CHG/STAT` çıkışı** tarafından sürülmeli; mikrodenetleyici yazılımı tarafından bağımsız olarak yakılmamalıdır.
* **Telemetri Aktarımı:** Şarj durumu (`charging: true/false`) ve batarya doluluk oranı (`battery_pct`) robot ana kontrolcüsü üzerinden Modül D'ye iletilmelidir.

---

## Ö3 Kabul Kriterleri ve Geliştirme Kontrol Listesi

### 1. Güç Dağıtımı ve İstasyon Donanımı
* [ ] Robot içi güç dağıtım kartı, voltaj regülatörleri, BMS ve sigorta korumasının tamamlanması
* [ ] Şarj istasyonu mekanik dock yapısının ve robot karşıt temas yüzeyinin üretilmesi
* [ ] Donanımsal `CHG/STAT` durum göstergesinin (LED / pil seviye göstergesi) devresinin kurulması
* [ ] Şarj durum bilgisinin robot kontrolcüsüne (Modül A) dijital/analog sinyal olarak bağlanması

### 2. Otomatik Şarj Doğrulama Testleri (Ö3 — 20 Puan)
* [ ] **Alt Test 1 (5 Puan):** Şarj istasyonu merkezli 30 cm yarıçaplı ve 90&deg; yaklaşma bölgesinde robotun insan müdahalesi olmadan istasyonu algılayıp yönelmesi
* [ ] **Alt Test 2 (5 Puan):** Yaklaşma sırasında yön ve hızını otomatik ayarlayarak şarj konumuna hatasız yerleşmesi
* [ ] **Alt Test 3 (5 Puan):** Doğru yerleşmeden en geç 5 s içinde şarjın otomatik başlaması ve donanımsal göstergenin yanması
* [ ] **Alt Test 4 (5 Puan):** Robot istasyondan çekildiğinde göstergenin en geç 2 s içinde sönmesi; tekrar doğru konuma getirildiğinde en geç 5 s içinde yeniden yanması
