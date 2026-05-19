# ⚡ FC Scout AI — Data-Driven Player Similarity Engine

> A scouting tool for EA FC 26 players that performs **position-specific weighted similarity analysis (Similarity Engine)** using data science principles.  
> 14,412 Players · Pure HTML/CSS/JS · Zero Dependencies · Fully Client-Side

**[🔴 Click for Live Demo](https://ssemiih.github.io/fc-scout-ai)**

---

---

## ✨ Key Features

| Feature | Details |
|--------|-------|
| 🧮 **Weighted Vector Analysis** | Dynamic calculation with different attribute weights for each position (e.g., ST, CB, CAM). |
| 🎯 **Multi-Layered Scoring** | Euclidean distance + league tier penalty + weak foot/skill move penalties + play style bonuses. |
| 🎨 **Data Visualization** | Color-coded similarity rates: Perfect (Green) → Medium (Yellow) → Low (Red). |
| 🔍 **Instant Search (Autocomplete)** | Highly performant, zero-latency player recommendation system. |
| 📊 **Detailed Analysis Bars** | Visual representation of 14 different player attributes upon clicking a card. |
| 🎯 **Advanced Filtering** | 155+ Nations, 45 Leagues, Specific Positions, and Age (U23, etc.) filters. |

---

## 🧠 Algorithm & Data Science Approach

### Core Logic
What separates this project from a standard search engine is the creation of a **position-specific weighted feature vector** for each player. The final similarity score is based on the following equation:

`final_score = base_score - tier_penalty + foot_bonus - wf_penalty - sm_penalty + style_bonus`

### 1. Dimensionality Reduction & Group Weights
Dozens of different player statistics are categorized into logical clusters:
* **Box Threat:** Finishing, Positioning, Heading Accuracy, Penalties
* **Distance Threat:** Long Shots, Shot Power, Volleys
* **Playmaking:** Short Passing, Vision, Curve
* **Agility:** Agility, Balance, Reactions, Composure
* *(and other physical/defensive groups...)*

### 2. Position-Specific Weighting Matrix (Example)

| Position | Primary Focus | Weight | Secondary Focus | Weight |
|-------|---------------|---------|---------------|---------|
| **ST** | Box Threat | 35% | Speed | 12% |
| **CB** | Def Awareness | 29% | Ball Winning | 29% |
| **CAM** | Playmaking | 30% | Distance Threat | 18% |

### 3. Distance and Score Calculation
To balance the variance among the data, a Z-score normalization logic is applied, and a base score is calculated using the Euclidean distance:

`scaled_features = StandardScaler(features)`  
`weighted_vector = scaled_features * position_weights`  
`distance = euclidean(target_vector, candidate_vector)`  
`base_score = 100 * exp(-distance^2 * 15)`

### 4. Contextual Penalties and Bonuses
Mathematical similarity is blended with football realities (context):
* **League Tier Penalty:** Quality differences between lower and top-tier league players are penalized.
* **Physical Traits:** Differences in weak foot and skill moves are added as negative multipliers.
* **Play Styles:** Shared gold/silver play styles reflect as bonuses (+1.0 / +2.5) to the score.

---

## 📁 Project Architecture & Data Format

**Zero Dependencies.** Only a modern web browser is required. To maximize data fetching performance, the JSON file is minified using a `compact key` format.

```text
fc-scout-ai/
├── index.html          # Single-page application (Core UI & Logic)
├── data/
│   └── players.json    # Compressed EA FC 26 player dataset (~9MB)
└── README.md
```

---

## 🛠️ Local Development

To run the project locally and bypass browser CORS (Cross-Origin Resource Sharing) policies, use a simple HTTP server:

`python -m http.server 8080`

---

## 👨‍💻 Developer

**Mahmut Semih Kiraz** *Computer Scientist & Data Analytics Enthusiast*
* [LinkedIn](https://www.linkedin.com/in/YOUR_LINKEDIN_URL) *(Buraya kendi linkini eklemeyi unutma)*
* [GitHub](https://github.com/ssemiih)

*Developing data-driven and machine learning-based analysis projects in football analytics. Open for feedback and collaborations.*

---

## 📝 License
MIT License — Feel free to use, fork, and integrate into your own projects.

> *Note: EA FC 26 data is used for educational, hobby, and data science practice purposes only. FC Scout AI is an independent analytics project and has no official affiliation with EA Sports.*
