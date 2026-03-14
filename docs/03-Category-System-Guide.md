# 📖 BÖLÜM 3: Kategori Sistemini Model Seçimi İçin Kullanma

## 🎯 Temel Kavram

Kategori sistemi, Sisyphus'un **ne tür bir iş** yaptığını bilip, **hangi modelin** en uygun olduğunu otomatik seçmesini sağlar.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    KATEGORİ SİSTEMİ AKIŞI                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   Sisyphus: "Bu iş bir frontend UI değişikliği"                        │
│                    │                                                    │
│                    ▼                                                    │
│   ┌─────────────────────────────────────────────────────────────────┐ │
│   │              CATEGORY: visual-engineering                        │ │
│   │                                                                  │ │
│   │   ANLAM: Frontend, UI/UX, tasarım, CSS, animasyon              │ │
│   │                                                                  │ │
│   │   OTOMATİK SEÇİM:                                               │ │
│   │   → Model: Gemini 3.1 Pro (high variant)                       │ │
│   │   → Temperature: 0.7 (yaratıcı)                                │ │
│   │   → Prompt: "Designer-turned-developer..."                      │ │
│   │                                                                  │ │
│   └─────────────────────────────────────────────────────────────────┘ │
│                    │                                                    │
│                    ▼                                                    │
│   Sisyphus-Junior (Visual uzmanı) çalışır                           │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Kategori Türleri ve Ne Zaman Kullanılır

### 1. VISUAL-ENGINEERING

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3.1 Pro (high) |
| **Temperature** | 0.7 (yaratıcı) |
| **Ne Zaman** | Frontend, UI/UX, CSS, tasarım, animasyon |

#### Kullanım Örnekleri

```
# Otomatik kategori algılama
"Add a responsive navigation bar with smooth animations"
→ Sisyphus: visual-engineering algıladı
→ Gemini 3.1 Pro çağrılır

"Redesign the login form to match modern design trends"
→ Sisyphus: visual-engineering algıladı

# Manuel kategori belirtme
task(
  category="visual-engineering",
  load_skills=["frontend-ui-ux", "playwright"],
  prompt="Create a dashboard with charts and responsive layout"
)
```

#### Neden Gemini?

```
GEMINI VISUAL ENGINEERING'DE NEDEN EN İYİ:

1. VİZYON KAPASİTESİ: Görsel içeriği anlar
2. TASARIM KAVRAMI: UI/UX prensiplerini bilir
3. YARATICILIK: Düşük temperature değil, orta-yüksek
4. KODGEN: Frontend kod üretimi güçlü

KARŞILAŞTIRMA:
┌─────────────────┬─────────────────┬─────────────────┐
│                 │ Gemini 3.1 Pro  │ Claude Opus     │
├─────────────────┼─────────────────┼─────────────────┤
│ Visual design   │ ⭐⭐⭐⭐⭐       │ ⭐⭐⭐          │
│ CSS skills      │ ⭐⭐⭐⭐⭐       │ ⭐⭐⭐⭐         │
│ Animation       │ ⭐⭐⭐⭐⭐       │ ⭐⭐⭐          │
│ Creativity      │ ⭐⭐⭐⭐        │ ⭐⭐⭐⭐         │
└─────────────────┴─────────────────┴─────────────────┘
```

---

### 2. ULTRABRAIN

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | GPT-5.4 (xhigh) veya Gemini 3.1 Pro (high) |
| **Temperature** | 0.1 (tutarlı) |
| **Ne Zaman** | Derin mantıksal akıl yürütme, mimari kararlar |

#### Kullanım Örnekleri

```
"Design a distributed caching strategy for our microservices"
→ Sisyphus: ultrabrain algıladı
→ GPT-5.4 / Gemini 3.1 Pro çağrılır

"Analyze the trade-offs between SQL and NoSQL for this use case"
→ Sisyphus: ultrabrain algıladı

"Refactor this algorithm to achieve O(n log n) instead of O(n²)"
→ Sisyphus: ultrabrain algıladı

task(
  category="ultrabrain",
  prompt="Design a rate limiting system that handles 100k requests/sec"
)
```

#### Extended Thinking

```jsonc
// Konfigürasyonda ultrabrain için extended thinking
"ultrabrain": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "high",
  "temperature": 0.1,
  
  // DERİN DÜŞÜNME MODU
  "thinking": {
    "type": "enabled",
    "budgetTokens": 16000  // Düşünme için ayrılan token
  }
}
```

---

### 3. DEEP

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | GPT-5.3 Codex (medium) |
| **Temperature** | 0.2 |
| **Ne Zaman** | Otonom problem çözme, derin araştırma |

#### Kullanım Örnekleri

```
"Fix the race condition in our concurrent queue implementation"
→ Sisyphus: deep algıladı
→ GPT-5.3 Codex veya Gemini çağrılır

"Research all edge cases for this authentication flow and fix them"
→ Sisyphus: deep algıladı

task(
  category="deep",
  prompt="Investigate the memory leak in our Node.js application and propose fixes"
)
```

---

### 4. ARTISTRY

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3.1 Pro (high) |
| **Temperature** | 0.8 (çok yaratıcı) |
| **Ne Zaman** | Yaratıcı çözümler, özgün fikirler |

#### Kullanım Örnekleri

```
"Create an unconventional error message system that surprises users"
→ Sisyphus: artistry algıladı

"Design a unique micro-interaction pattern no one has seen before"
→ Sisyphus: artistry algıladı

task(
  category="artistry",
  prompt="Design an easter egg feature that delights power users"
)
```

---

### 5. QUICK

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3 Flash / Haiku |
| **Temperature** | 0.2 |
| **Ne Zaman** | Basit işler, tek dosya değişikliği |

#### Kullanım Örnekleri

```
"Fix the typo in the README"
→ Sisyphus: quick algıladı
→ Flash çağrılır (hızlı ve ucuz)

"Change the button text from Submit to Send"
→ Sisyphus: quick algıladı

"Add a console.log to debug this function"
→ Sisyphus: quick algıladı

task(
  category="quick",
  prompt="Change the color of the header from blue to green"
)
```

---

### 6. UNSPECIFIED-LOW

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3 Flash |
| **Temperature** | 0.3 |
| **Ne Zaman** | Kategoriye uymayan işler, düşük çaba |

#### Kullanım Örnekleri

```
# Sisyphus kategori belirleyemediğinde ve düşük çaba olarak
// Normal kullanımda manuel belirtmek nadir
```

---

### 7. UNSPECIFIED-HIGH

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3.1 Pro (medium) |
| **Temperature** | 0.2 |
| **Ne Zaman** | Kategoriye uymayan işler, yüksek çaba |

#### Önemli Not

```
⚠️ DİKKAT:

Unspecified-high için öNERİLEN:
→ Claude Opus 4.6 (max) veya Gemini 3.1 Pro (high)

Sizin konfigürasyonunuzda:
→ Gemini 3 Flash kullanılıyor

NEDEN SORUN?: Yüksek çaba gerektiren işler için Flash yetersiz kalabilir

ÇÖZÜM:
"unspecified-high": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "medium"
}
```

---

### 8. WRITING

| Özellik | Değer |
|---------|-------|
| **Varsayılan Model** | Gemini 3 Flash |
| **Temperature** | 0.5 (akıcı) |
| **Ne Zaman** | Dokümantasyon, README, yazı |

#### Kullanım Örnekleri

```
"Write comprehensive API documentation for our endpoints"
→ Sisyphus: writing algıladı

"Create a README for this library"
→ Sisyphus: writing algıladı

task(
  category="writing",
  prompt="Write a changelog summarizing these commits"
)
```

---

## 🔧 Kategori + Skill Kombinasyonları

En güçlü kullanım, kategori ve skill'leri birleştirmektir:

### Örnek 1: UI Uzmanı

```typescript
task(
  category: "visual-engineering",      // → Gemini 3.1 Pro
  load_skills: ["frontend-ui-ux", "playwright"],  // → UI expertise + browser testing
  prompt: "Create a responsive dashboard with charts",
)
```

**Sonuç:**
- Model: Gemini 3.1 Pro (visual uzmanı)
- Skill: frontend-ui-ux (design-first thinking)
- Skill: playwright (browser verification)
- Çıktı: Estetik UI + otomatik test

---

### Örnek 2: Git Uzmanı

```typescript
task(
  category: "quick",
  load_skills: ["git-master"],
  prompt: "Commit these changes with atomic commits",
)
```

**Sonuç:**
- Model: Gemini 3 Flash (hızlı)
- Skill: git-master (atomic commits, style detection)
- Çıktı: Temiz commit history

---

### Örnek 3: Derin Mimari Analizi

```typescript
task(
  category: "ultrabrain",
  load_skills: [],
  prompt: "Design a plugin system architecture for our application",
)
```

**Sonuç:**
- Model: Gemini 3.1 Pro + extended thinking
- Skill: Yok (saf akıl yürütme)
- Çıktı: Detaylı mimari plan

---

## ⚠️ Yaygın Hatalar

### Hata 1: Ağır Modeli Basit İş için Kullanma

```
❌ YANLIŞ:
task(
  category: "ultrabrain",  // GPT-5.4 xhigh = PAHALI
  prompt: "Change button color"
)
// Pahalı model basit iş için harcanıyor

✅ DOĞRU:
// Direkt yazın, Sisyphus quick'e delege eder
"Change button color to blue"
```

### Hata 2: Visual İş için Yanlış Kategori

```
❌ YANLIŞ:
task(
  category: "deep",  // GPT-5.3 Codex = backend odaklı
  prompt: "Design a beautiful login page"
)
// Visual iş için backend model

✅ DOĞRU:
task(
  category: "visual-engineering",
  prompt: "Design a beautiful login page"
)
```

### Hata 3: Skill Eklemeden Kategori Kullanma

```
❌ YANLIŞ:
task(
  category: "visual-engineering",
  load_skills: [],  // Boş!
  prompt: "Add dark mode support"
)
// Skill olmadan design guidance eksik

✅ DOĞRU:
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux"],
  prompt: "Add dark mode support"
)
```

---

## 📚 Kategori Karar Verme Akışı

```
İŞ TÜRÜ NE?
    │
    ├─ FRONTEND/UI/UX/ANİMASYON?
    │   └─ visual-engineering
    │
    ├─ ALGORİTMA/MİMARİ/DİP MANTIĞA SAHIP KARAR?
    │   └─ ultrabrain
    │
    ├─ DERİN ARAŞTIRMA/DEBUGGING?
    │   └─ deep
    │
    ├─ YARATICI/ÖZGÜN FİKİR?
    │   └─ artistry
    │
    ├─ BASİT DEĞİŞİKLİK (TEK DOSYA)?
    │   └─ quick
    │
    ├─ DOKÜMANTASYON/YAZI?
    │   └─ writing
    │
    ├─ GENEL İŞ, DÜŞÜK ÇABA?
    │   └─ unspecified-low
    │
    └─ GENEL İŞ, YÜKSEK ÇABA?
        └─ unspecified-high
```

---

## 🎯 Pratik Egzersiz

### Soru 1: Hangi kategori?

```
"Add authentication to the /settings route following the same pattern as /notes"
```

<details>
<summary>Cevap</summary>

```
KATEGORİ: quick veya unspecified-low

NEDEN?: 
- Pattern takip etme var (mevcut desen kopyalama)
- Basit route ekleme
- Derin akıl yürütme gerektirmiyor

AKIŞ:
Sisyphus: "Bu quick bir iş"
→ Explore: Pattern ara (@notes route)
→ Sisyphus-Junior: Aynısını uygula
→ Doğrula
```

</details>

---

### Soru 2: Hangi kategori?

```
"Design a real-time notification system that scales to 1M concurrent users"
```

<details>
<summary>Cevap</summary>

```
KATEGORİ: ultrabrain

NEDEN?:
- Derin mimari kararlar
- Scalability trade-off'ları
- Performance optimization
- Yüksek çaba

AKIŞ:
Sisyphus: "Bu deep architecture work"
→ Gemini 3.1 Pro + extended thinking
→ Akıl yürüt → Propose solution
→ Oracle: Review
```

</details>

---

### Soru 3: Hangi kategori + skill?

```
"Create an animated pricing table with hover effects and dark mode"
```

<details>
<summary>Cevap</summary>

```
KATEGORİ: visual-engineering
SKILLS: ["frontend-ui-ux", "playwright"]

NEDEN?:
- Frontend work (Gemini)
- Design expertise needed (frontend-ui-ux)
- Browser testing (playwright)

task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux", "playwright"],
  prompt: "Create an animated pricing table with hover effects and dark mode"
)
```

</details>