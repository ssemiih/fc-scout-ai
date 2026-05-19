# ⚡ FC Scout AI — Player Similarity Engine

> EA FC 26 oyuncuları arasında **pozisyona özel ağırlıklı benzerlik analizi** yapan scouting aracı.  
> 14,412 oyuncu · Saf HTML/CSS/JS · Sıfır bağımlılık · GitHub Pages'de çalışır

**[🔴 Canlı Demo](https://YOUR_USERNAME.github.io/fc-scout-ai)**

---

![Preview](https://i.imgur.com/placeholder.png)

---

## ✨ Özellikler

| Özellik | Detay |
|--------|-------|
| 🧮 Pozisyona özel ağırlıklandırma | ST, CB, CAM gibi her mevki için farklı özellik ağırlıkları |
| 🎯 Çok katmanlı benzerlik skoru | Euclidean mesafe + lig tier cezası + ayak bonusu + play style skoru |
| 🎨 Benzerlik renk kodlaması | Mükemmel (yeşil) → Çok İyi → İyi → Orta → Düşük (kırmızı) |
| 🔍 Anlık autocomplete | Yazarken oyuncu önerileri |
| 🏳️ Milliyet filtresi | 155+ ülke, bayrak ikonları ile |
| 🏆 Lig filtresi | 45 lig |
| 📍 Pozisyon filtresi | Aynı mevki veya spesifik pozisyon |
| 👶 Yaş filtresi | Min/max yaş, U23 hızlı filtresi |
| 🎴 EA FC kart görselleri | Resmi EA kart fotoğrafları |
| 📊 Detaylı stat barları | Karta tıklayınca 14 özellik gösterir |
| 🎮 Play style gösterimi | Gold/Silver stil etiketleri |

---

## 🧠 Algoritma

### Temel Mantık

Her oyuncu için **pozisyonuna göre belirlenmiş ağırlıklı özellik vektörü** oluşturulur. Benzerlik skoru şu bileşenlerden oluşur:

```
final_score = base_score - tier_penalty + foot_bonus - wf_penalty - sm_penalty + style_bonus
```

### 1. Özellik Grupları ve Ağırlıkları

```
Box Threat       → Finishing, Positioning, Heading Accuracy, Penalties
Distance Threat  → Long Shots, Shot Power, Volleys
Playmaking       → Short Passing, Vision, Curve
Service          → Crossing, Long Passing, Free Kick Accuracy
Ball Carry       → Dribbling, Ball Control
Agility          → Agility, Balance, Reactions, Composure
Ball Winning     → Standing Tackle, Sliding Tackle, Interceptions
Def Awareness    → Def Awareness
Speed            → Acceleration, Sprint Speed
Physical         → Strength, Jumping, Aggression, Stamina
```

### 2. Pozisyona Özel Ağırlıklar (Örnek)

| Mevki | En Önemli Grup | Ağırlık |
|-------|---------------|---------|
| ST    | Box Threat    | 35%     |
| CB    | Def Awareness + Ball Winning | 29% + 29% |
| LW/RW | Speed + Ball Carry | 25% + 20% |
| CDM   | Ball Winning + Physical | 25% + 25% |
| CAM   | Playmaking + Distance Threat | 30% + 18% |

### 3. Mesafe Hesabı

```
scaled_features = StandardScaler(features)  # Z-score normalization
weighted_vector = scaled * position_weights
distance        = euclidean(target_vector, candidate_vector)
base_score      = 100 × exp(−distance² × 15)
```

### 4. Bağlam Cezaları ve Bonusları

```
tier_penalty = |league_tier_target - league_tier_candidate| × 6.0
foot_bonus   = +2.0 (aynı ayak) | -2.0 (farklı ayak)
wf_penalty   = |weak_foot_diff| × 1.5
sm_penalty   = |skill_moves_diff| × 1.5
style_bonus  = Paylaşılan play style başına +1.0 / +1.5 / +2.5
                (Gold+Gold: +2.5, Silver+Silver: +1.5, Mixed: +1.0)
```

---

## 🚀 GitHub Pages'e Kurulum (5 Dakika)

### 1. Repoyu oluştur

```bash
# GitHub'da yeni bir public repo oluştur: fc-scout-ai
# Sonra local'e klonla:
git clone https://github.com/YOUR_USERNAME/fc-scout-ai.git
cd fc-scout-ai
```

### 2. Dosyaları kopyala

```
fc-scout-ai/
├── index.html          ← Ana sayfa
├── data/
│   └── players.json    ← Oyuncu verisi (9MB)
└── README.md
```

### 3. Push et

```bash
git add .
git commit -m "🚀 FC Scout AI initial commit"
git push origin main
```

### 4. GitHub Pages'i aç

1. Repo → **Settings** → **Pages**
2. Source: `Deploy from a branch`
3. Branch: `main` → `/ (root)` → **Save**
4. ~2 dakika sonra: `https://YOUR_USERNAME.github.io/fc-scout-ai` 🎉

---

## 📁 Proje Yapısı

```
fc-scout-ai/
├── index.html          # Tek dosya uygulama (HTML + CSS + JS)
├── data/
│   └── players.json    # EA FC 26 oyuncu verisi (compact format)
└── README.md
```

**Bağımlılık yok.** Sadece tarayıcı gerekli.

---

## 🛠️ Lokal Geliştirme

```bash
# Herhangi bir HTTP server ile çalıştır (CORS için gerekli)
python3 -m http.server 8080
# Sonra: http://localhost:8080
```

> ⚠️ `index.html`'i direkt dosya olarak açarsan (`file://`) `data/players.json` yüklenemez. Bir HTTP server kullan.

---

## 📊 Veri Formatı

`data/players.json` compact key formatı:

| Key | Alan           |
|-----|---------------|
| `n` | Name           |
| `pos` | Position     |
| `ov` | OVR            |
| `pa/sh/ps/dr/df/ph` | PAC/SHO/PAS/DRI/DEF/PHY |
| `nat` | Nation        |
| `lea` | League        |
| `tm` | Team           |
| `age` | Age           |
| `pf` | Preferred Foot |
| `wf` | Weak Foot      |
| `sm` | Skill Moves    |
| `sty` | Play Styles   |
| `card` | Card image URL |

---

## 🔧 Özelleştirme

### Pozisyon ağırlıklarını değiştirmek

`index.html` içinde `posWeights` objesini düzenle:

```javascript
const posWeights = {
  ST: {
    "Box Threat": 0.35,   // Gol koklama ağırlığını artır/azalt
    "Speed": 0.12,
    // ...
  }
};
```

### Benzerlik hassasiyetini ayarlamak

```javascript
const SHARP = 15.0;  // Büyüttükçe sadece çok benzer oyuncular çıkar
                     // Küçülttükçe daha geniş sonuçlar gelir
```

---

## 📝 Lisans

MIT — İstediğin gibi kullan, fork'la, geliştir.

---

*EA FC 26 verileri eğitim/hobi amaçlı kullanılmıştır. FC Scout AI, EA Sports ile resmi bir bağlantısı olmayan bağımsız bir projedir.*
