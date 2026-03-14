# 📖 BÖLÜM 4: Paralel Çalışmayı Verimli Kullanma

## 🎯 Neden Paralelizasyon Önemli?

**Tek ajan = 5 dakika × 4 görev = 20 dakika**
**Paralel 4 ajan = 5 dakika × 1 tur = 5 dakika**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    SIRALI vs PARALEL ÇALIŞMA                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  SIRALI (YANLIŞ):                                                          │
│  Ajan 1: [██████████] Görev 1 (5 dk)                                       │
│  Ajan 1:          [██████████] Görev 2 (5 dk)                              │
│  Ajan 1:                    [██████████] Görev 3 (5 dk)                     │
│  Ajan 1:                              [██████████] Görev 4 (5 dk)           │
│  TOPLAM: 20 dakika                                                          │
│                                                                             │
│  PARALEL (DOĞRU):                                                           │
│  Ajan 1: [██████████] Görev 1 (5 dk)                                       │
│  Ajan 2: [██████████] Görev 2 (5 dk)                                        │
│  Ajan 3: [██████████] Görev 3 (5 dk)                                        │
│  Ajan 4: [██████████] Görev 4 (5 dk)                                        │
│  TOPLAM: 5 dakika                                                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Background Agents

### Temel Kavram

Background agents, ana oturumunuzu bloke etmeden arka planda çalışır. Siz başka işlere devam ederken, ajanlar görevlerini tamamlar.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    BACKGROUND AGENT AKIŞI                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   KULLANICI (Sisyphus)                                                      │
│       │                                                                     │
│       ├─→ task(explore, background=true) → task_id: "bg_001"               │
│       │     "Find authentication patterns"                                 │
│       │                                                                     │
│       ├─→ task(librarian, background=true) → task_id: "bg_002"            │
│       │     "Find JWT best practices"                                       │
│       │                                                                     │
│       ├─→ task(explore, background=true) → task_id: "bg_003"               │
│       │     "Find error handling patterns"                                  │
│       │                                                                     │
│       └─→ [ANA OTURUMDA DEVAM]                                              │
│           "Şimdi user model'i inceleyelim..."                              │
│                                                                             │
│   ───────────────────────────────────────────────────────                   │
│   ARKA PLANDA:                                                              │
│                                                                             │
│   bg_001 (Explore): Pattern buluyor...                                     │
│   bg_002 (Librarian): Dokümantasyon araştırüyor...                          │
│   bg_003 (Explore): Error handling araştırıyor...                           │
│                                                                             │
│   ───────────────────────────────────────────────────────                   │
│   SİSTEM BİLDİRİMİ:                                                         │
│                                                                             │
│   <system-reminder>                                                         │
│   Background task bg_001 completed                                          │
│   </system-reminder>                                                        │
│                                                                             │
│   <system-reminder>                                                         │
│   Background task bg_002 completed                                          │
│   </system-reminder>                                                        │
│                                                                             │
│   <system-reminder>                                                         │
│   Background task bg_003 completed                                          │
│   </system-reminder>                                                        │
│                                                                             │
│   KULLANICI:                                                                │
│   background_output(task_id="bg_001") → Sonuçları al                        │
│   background_output(task_id="bg_002") → Sonuçları al                        │
│   background_output(task_id="bg_003") → Sonuçları al                        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Pratik Kullanım

### Temel Syntax

```typescript
// ARKA PLANDA BAŞLAT
task(
  subagent_type: "explore",           // Ajan tipi
  load_skills: [],                    // Skill'ler
  prompt: "Arama tanımı",             // Ne yapılacak
  run_in_background: true             // ARKA PLANDA ÇALIŞTIR
)

// SONUÇLARI AL
background_output(task_id: "bg_abc123")
```

---

### Örnek 1: Kod Tabanı Araştırması

```typescript
// SENARYO: Authentication sistemini anla

// ADIM 1: Paralel aramaları başlat
task(
  subagent_type: "explore",
  load_skills: [],
  description: "Find auth patterns",
  prompt: `
    FIND: Authentication middleware patterns
    CONTEXT: src/middleware/, src/routes/
    FOCUS: How auth is implemented
    SKIP: Test files
    RETURN: File paths with pattern descriptions
  `,
  run_in_background: true
)

task(
  subagent_type: "explore",
  load_skills: [],
  description: "Find token handling",
  prompt: `
    FIND: JWT token generation and validation
    CONTEXT: src/auth/, src/services/
    FOCUS: Token flow
    RETURN: Key files and functions
  `,
  run_in_background: true
)

task(
  subagent_type: "librarian",
  load_skills: [],
  description: "Find JWT docs",
  prompt: `
    FIND: JWT best practices and security guidelines
    FOCUS: Token expiration, refresh strategies
    RETURN: Key recommendations
  `,
  run_in_background: true
)

// ADIM 2: Ana akışta devam et
"Şimdi user model yapısını inceleyelim..."
// [Siz çalışırken arka planda ajanlar devam ediyor]

// ADIM 3: Sistem bildirimleri geldikçe sonuçları topla
// background_output(task_id="bg_001")
// background_output(task_id="bg_002")
// background_output(task_id="bg_003")

// ADIM 4: Tüm sonuçları birleştir ve analiz et
```

---

### Örnek 2: Frontend + Backend Paralel

```typescript
// SENARYO: Yeni özellik ekleme

// ADIM 1: Frontend ve Backend'ı paralel başlat
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux"],
  description: "Design UI component",
  prompt: `
    TASK: Create responsive dashboard component
    CONTEXT: src/components/, using React + Tailwind
    MUST:
    - Match existing design system
    - Mobile-first responsive
    - Include loading states
    MUST NOT:
    - Add new dependencies
    - Break existing layouts
  `,
  run_in_background: true
)

task(
  category: "deep",
  load_skills: [],
  description: "Design API endpoint",
  prompt: `
    TASK: Design REST endpoint for dashboard data
    CONTEXT: src/api/, using Express
    MUST:
    - Use existing auth middleware
    - Return paginated results
    - Include error handling
    RETURN: Endpoint specification
  `,
  run_in_background: true
)

// ADIM 2: İki taraf için dokümantasyon ara
task(
  subagent_type: "librarian",
  load_skills: [],
  description: "Find API docs",
  prompt: "Find Express pagination best practices",
  run_in_background: true
)

// ADIM 3: Devam et, sonuçlar gelince birleştir
```

---

### Örnek 3: 5+ Ajan Paralel

```typescript
// SENARYO: Kapsamlı kod tabanı analizi

task(subagent_type="explore", run_in_background=true,
     description="Find auth patterns",
     prompt="Find all authentication implementations")

task(subagent_type="explore", run_in_background=true,
     description="Find database patterns",
     prompt="Find database connection and query patterns")

task(subagent_type="explore", run_in_background=true,
     description="Find error handling",
     prompt="Find error handling implementations")

task(subagent_type="librarian", run_in_background=true,
     description="Find security docs",
     prompt="Find OWASP security guidelines")

task(subagent_type="librarian", run_in_background=true,
     description="Find testing docs",
     prompt="Find Jest testing best practices")

// 5 AYNI ANDA ÇALIŞIYOR → Süre: max(her biri) = her birinin süresi
// SIRALI OLAYDI → Süre: sum(her biri) = 5 × süre
```

---

## 📊 Concurrency Limits (Eşzamanlılık Limitleri)

Konfigürasyonda `background_task` ayarları paralel çalışmayı kontrol eder:

```jsonc
"background_task": {
  // Toplam max paralel görev
  "defaultConcurrency": 10,
  
  // Sağlayıcı bazlı limitler
  "providerConcurrency": {
    "anthropic": 3,      // Claude API: max 3 paralel
    "openai": 5,         // GPT API: max 5 paralel
    "google": 10,        // Gemini: max 10 paralel
    "opencode": 15       // Ücretsiz: max 15 paralel
  },
  
  // Model bazlı limitler (öncelikli)
  "modelConcurrency": {
    "anthropic/claude-opus-4-6": 2,   // Pahalı model: düşük limit
    "opencode/gpt-5-nano": 20,        // Ucuz model: yüksek limit
    "opencode/glm-4.7-free": 30       // Ücretsiz: çok yüksek
  }
}
```

### Concurrency Mantığı

```
ÖRNEK SENARYO:
  Kullanıcı 6 Explore ajanı başlattı
  
ARka planda çalışanlar:
  Explore 1 → opencode/gpt-5-nano (model limit: 20)
  Explore 2 → opencode/gpt-5-nano (model limit: 20)
  Explore 3 → opencode/gpt-5-nano (model limit: 20)
  Explore 4 → opencode/gpt-5-nano (model limit: 20)
  Explore 5 → opencode/gpt-5-nano (model limit: 20)
  Explore 6 → opencode/gpt-5-nano (model limit: 20)
  
HEPSİ AYNI ANDA ÇALIŞIR (6 < 20 limit)
```

---

## ⚡ Optimal Paralelizasyon Stratejileri

### Strateji 1: Bağımsız Görevleri Paralel Yapın

```
❌ YANLIŞ (Bağımlı Görevleri Paralel Yapmaya Çalışmak):

  task(background, "Parse API response")
  task(background, "Process parsed data")  // Bağımlı!
  
✅ DOĞRU (Bağımsız Görevleri Paralel Yapın):

  task(background, "Find auth patterns")
  task(background, "Find error handling")
  task(background, "Find database patterns")
  // Hepsi bağımsız → paralel çalışır
```

### Strateji 2: Farklı Ajanları Kullanın

```typescript
// AYNI AJAN TIPI ARKA ARKAYA:
task(subagent_type="explore", background=true, ...)
task(subagent_type="explore", background=true, ...)
task(subagent_type="explore", background=true, ...)
// → Explore agent instances = paralel

// FARKLI AJAN TIPLERİ:
task(subagent_type="explore", background=true, ...)
task(subagent_type="librarian", background=true, ...)
task(subagent_type="oracle", background=true, ...)
// → Farklı uzmanlık alanları = daha verimli
```

### Strateji 3: Sonuçları Toplayın, Boş Beklemeyin

```typescript
// ❌ YANLIŞ: Bloke ederek bekle
const result1 = background_output(task_id="bg_001", block=true)
// [ANA AKIŞ DURDU]
const result2 = background_output(task_id="bg_002", block=true)
// [ANA AKIŞ DURDU]

// ✅ DOĞRU: Sistem bildirimi gelince topla
// [ANA AKIŞTA DEVAM ET]
// <system-reminder> → O zaman background_output çağır
```

---

## 🎮 İleri Düzey: Task Tool Parametreleri

### Tam Parametre Listesi

```typescript
task({
  // ZORUNLU:
  load_skills: ["skill-1", "skill-2"],  // Skill'ler
  prompt: "Görev tanımı",                 // Ne yapılacak
  run_in_background: true,               // Arka plan mı
  
  // OPSİYONEL:
  category: "visual-engineering",        // Kategori
  subagent_type: "explore",              // Direkt ajan
  description: "Kısa açıklama",          // Görev başlığı
  session_id: "ses_abc123",              // Devam eden session
  command: "/custom-command",            // Slash command
})
```

### Parametre Öncelikleri

```
ÖNCELİK SIRASI:

1. category VEYA subagent_type (BİRİNİ VERİN, İKİSİNİ VERMEYİN)
   └─ category: İş türünü belirt, sistem model seçer
   └─ subagent_type: Direkt ajan belirt

2. load_skills: Uzmanlık ekler
   └─ Her zaman verilmeli ([] boş bile olsa)

3. run_in_background: true/false
   └─ Paralel için true

4. prompt: Görev açıklaması
   └─ NE YAPILACAĞINI NET AÇIKLA

5. description: Kısa etiket
   └─ Task listesinde görünür
```

---

## 📈 Performance Optimizasyonu

### Model Maliyet Hesabı

```
ÖRNEK: 5 paralel Explore ajanı

MODEL: opencode/gpt-5-nano
COST: $0.001 per 1M tokens
TOKENS per task: ~10,000

5 tasks × 10,000 tokens × $0.001/1M = $0.00005

KARŞILAŞTIRMA:
  opencode/gpt-5-nano × 5 = $0.00005
  anthropic/claude-opus-4-6 × 5 = $1.50
  
FARK: 30,000× DAHA UCUZ
```

### Concurrency Tuning

```jsonc
// HIZ ÖNCELİKLİ:
"background_task": {
  "defaultConcurrency": 20,
  "modelConcurrency": {
    "opencode/gpt-5-nano": 30,
    "opencode/glm-4.7-free": 50
  }
}

// MALİYET ÖNCELİKLİ:
"background_task": {
  "defaultConcurrency": 5,
  "modelConcurrency": {
    "anthropic/claude-opus-4-6": 1,
    "google/gemini-3.1-pro-preview": 2
  }
}

// DENGeli:
"background_task": {
  "defaultConcurrency": 10,
  "providerConcurrency": {
    "anthropic": 3,
    "openai": 3,
    "google": 10,
    "opencode": 15
  }
}
```

---

## ⚠️ Yaygın Hatalar

### Hata 1: Background'ı Unutmak

```typescript
// ❌ YANLIŞ: block=false unuttun
task(subagent_type="explore", prompt="Find patterns")
// → ANA AKIŞTA BEKLER (background değil!)

// ✅ DOĞRU: run_in_background=true
task(subagent_type="explore", prompt="Find patterns", run_in_background=true)
// → ARKA PLANDA ÇALIŞIR
```

### Hata 2: Bağımlı Görevleri Yanlış Sıralama

```typescript
// ❌ YANLIŞ: Her şeyi paralel başlat
task(background, "Find patterns")  // Sonuç gerekiyor
task(background, "Implement based on patterns")  // Bağımlı!

// ✅ DOĞRU: Bağımlı görevleri serial yap
task(background, "Find patterns")
// [... diğer işler ...]
// Sistem bildirimi gelince:
const patterns = background_output(task_id)
task(category="deep", prompt=`Implement based on: ${patterns}`)
```

### Hata 3: Prompt Yetersizliği

```typescript
// ❌ YANLIŞ: Çok kısa prompt
task(subagent_type="explore", run_in_background=true,
     prompt="Find auth")
// → NE ARADIĞINI ANLAMAZ

// ✅ DOĞRU: Detaylı prompt
task(subagent_type="explore", run_in_background=true,
     prompt: `
       FIND: authentication middleware patterns
       CONTEXT: src/middleware/, src/routes/
       FOCUS: How JWT tokens are validated
       SKIP: Test files, mock data
       RETURN: File paths with key function names
     `)
```

---

## 📚 Pratik Checklist

```
PARALEL ÇALIŞMA MEDENİYETİ:

✅ Görevler bağımsız mı? → Paralel başlat
✅ 3+Explore/Librarian gerekiyor mu? → Background kullan
✅ Model concurrency limitini aşıyor mu? → Ayarları kontrol et
✅ Prompt yeterli detayda mı? → Context, focus, return format ekle
✅ Sonuçları toplayacak mekanizma var mı? → Sistem bildirimlerini izle

SIRALI ÇALIŞMA GEREKEN DURUMLAR:

❌ Bir görev diğerinin sonucuna bağımlı → Serial yap
❌ Kaynak yarışı var (dosya yazma) → Serial yap
❌ Context'de değişiklik gerekiyor → Serial yap
```