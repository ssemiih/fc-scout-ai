# ⚡ FC Scout AI — Veri Odaklı Oyuncu Benzerlik Motoru

> EA FC 26 oyuncuları arasında **pozisyona özel ağırlıklı benzerlik analizi (Similarity Engine)** yapan, veri bilimi prensipleriyle çalışan scouting aracı.  
> 14,412 Oyuncu · Saf HTML/CSS/JS · Sıfır Bağımlılık · Tamamen İstemci Taraflı (Client-side)

**[🔴 Canlı Demo İçin Tıklayın](https://ssemiih.github.io/fc-scout-ai)**

---

---

## ✨ Öne Çıkan Özellikler

| Özellik | Detay |
|--------|-------|
| 🧮 **Ağırlıklı Vektör Analizi** | ST, CB, CAM gibi her mevki için farklı özellik ağırlıkları ile dinamik hesaplama. |
| 🎯 **Çok Katmanlı Skorlama** | Euclidean (Öklid) mesafesi + lig seviyesi cezası + zayıf ayak/yetenek cezaları + play style bonusları. |
| 🎨 **Veri Görselleştirme** | Benzerlik oranına göre renk kodlaması: Mükemmel (Yeşil) → Orta (Sarı) → Düşük (Kırmızı). |
| 🔍 **Anlık Arama (Autocomplete)** | Performanslı ve gecikmesiz oyuncu öneri sistemi. |
| 📊 **Detaylı Analiz Barları** | Karta tıklandığında 14 farklı oyuncu özelliğinin görselleştirilmesi. |
| 🎯 **Gelişmiş Filtreleme** | 155+ Ülke, 45 Lig, Spesifik Mevki ve Yaş (U23 vb.) filtreleri. |

---

## 🧠 Algoritma & Veri Bilimi Yaklaşımı

### Temel Mantık
Projeyi standart bir arama motorundan ayıran temel özellik; her oyuncu için **pozisyonuna göre belirlenmiş ağırlıklı bir özellik vektörü** oluşturulmasıdır. Nihai benzerlik skoru şu denkleme dayanır:

`final_score = base_score - tier_penalty + foot_bonus - wf_penalty - sm_penalty + style_bonus`

### 1. Boyut İndirgeme ve Grup Ağırlıkları
Oyuncuların onlarca farklı istatistiği, mantıksal kümelere ayrılmıştır:
* **Box Threat:** Finishing, Positioning, Heading Accuracy, Penalties
* **Distance Threat:** Long Shots, Shot Power, Volleys
* **Playmaking:** Short Passing, Vision, Curve
* **Agility:** Agility, Balance, Reactions, Composure
* *(ve diğer fiziksel/defansif gruplar...)*

### 2. Pozisyona Özel Ağırlıklandırma Matrisi (Örnek)

| Mevki | Birincil Öncelik | Ağırlık | İkincil Öncelik | Ağırlık |
|-------|---------------|---------|---------------|---------|
| **ST** | Box Threat | %35 | Speed | %12 |
| **CB** | Def Awareness | %29 | Ball Winning | %29 |
| **CAM** | Playmaking | %30 | Distance Threat | %18 |

### 3. Mesafe (Distance) ve Skor Hesabı
Veriler arasındaki varyansı dengelemek için Z-score normalizasyonu mantığı kullanılmış ve Öklid mesafesi üzerinden bir taban skor hesaplanmıştır:

`scaled_features = StandardScaler(features)`  
`weighted_vector = scaled_features * position_weights`  
`distance = euclidean(target_vector, candidate_vector)`  
`base_score = 100 * exp(-distance^2 * 15)`

### 4. Bağlam Cezaları ve Bonusları
Matematiksel benzerlik, futbolun gerçeklikleriyle (bağlam) harmanlanmıştır:
* **Lig Seviyesi (Tier) Cezası:** Alt lig ile üst lig oyuncusu arasındaki kalite farkı cezalandırılır.
* **Fiziksel Özellikler:** Ters ayak, yetenek hareketleri farklılıkları negatif çarpan olarak eklenir.
* **Oyun Stili (Play Styles):** Ortak altın/gümüş oyun stilleri skora bonus (+1.0 / +2.5) olarak yansır.

---

## 📁 Proje Mimarisi & Veri Formatı

**Bağımlılık Yoktur.** Sadece modern bir web tarayıcısı gereklidir. Veri çekimi performansını maksimize etmek için JSON dosyası `compact key` formatında küçültülmüştür.

```text
fc-scout-ai/
├── index.html          # Tek dosya uygulama (Core UI & Logic)
├── data/
│   └── players.json    # Sıkıştırılmış EA FC 26 oyuncu veriseti (~9MB)
└── README.md
```

---

## 🛠️ Kurulum & Lokal Geliştirme

Projeyi kendi bilgisayarınızda çalıştırmak ve CORS (Cross-Origin Resource Sharing) politikalarına takılmamak için basit bir HTTP sunucusu kullanmalısınız:

`python -m http.server 8080`

---

## 👨‍💻 Geliştirici

**Mahmut Semih Kiraz** *Computer Scientist & Data Analytics Enthusiast*
* [LinkedIn](https://www.linkedin.com/in/senin-linkedin-linkin-buraya) *(Bu parantezi silip kendi linkini yapıştır)*
* [GitHub](https://github.com/ssemiih)

*Futbol verileri ve makine öğrenimi tabanlı analiz projeleri geliştirmekteyim. Geri bildirimleriniz ve işbirlikleri için ulaşabilirsiniz.*

---

## 📝 Lisans
MIT License — İstediğiniz gibi kullanabilir, geliştirebilir ve kendi projelerinize entegre edebilirsiniz.

> *Not: EA FC 26 verileri eğitim, hobi ve veri bilimi pratikleri amacıyla kullanılmıştır. FC Scout AI, bağımsız bir analitik projesidir ve EA Sports ile resmi bir bağı bulunmamaktadır.*
