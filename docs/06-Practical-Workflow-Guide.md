# 📖 BÖLÜM 6: Pratik Senaryo - Baştan Sona İş Akışı

## 🎯 Senaryo: Authentication Sistemi Yeniden Yazma

Bu bölümde, öğrendiğimiz tüm kavramları bir gerçek dünya senaryosunda birleştireceğiz.

**Senaryo:** Mevcut session-based authentication sistemini JWT-based sisteme geçirmemiz gerekiyor.

---

## 📋 Adım Adım İş Akışı

### ADIM 1: Intent Gate ve Sınıflandırma

```
KULLANICI GİRDİSİ:
"Convert our session-based authentication to JWT with refresh tokens"

SİSYPHUS INTENT GATE:
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                            INTENT ANALYSIS:                     │
│   │                                                                     │
│   │  Girdi: "Convert session-auth to JWT"                                 │
│   │  Tür: IMPLEMENTATION                                                  │   │  Kapsam: Authentication sistem               ││  Karmaşıklık: ORTA-YÜKSEK                                       │
│   │                                                                       │
│   │  AKSİYON:                                                              │
│   │  ├─ Araştırma gerekiyor → Explore                                     │
│   │  ├─ Dokümantasyon gerekiyor → Librarian                              │   │  ├─ Planlama gerekiyor → Prometheus
│   │  └─ Uygulama → Category delegation                                   │   │
│   │  EYLEM: ultrawork moduaktif et                                        │
│   │                                                                       │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### ADIM 2: Paralel Araştırma Başlatma

```typescript
// Sisyphus otomatik başlatır:

task(
  subagent_type: "explore",
  load_skills: [],
  run_in_background: true,
  description: "Find current auth implementation",
  prompt: `
    FIND: Authentication middleware, session handling, user login
    CONTEXT: src/middleware/, src/routes/, src/services/
    FOCUS: How sessions are currently managed
    SKIP: Test files, mock data
    RETURN: Key files, functions, and data flow
  `
)

task(
  subagent_type: "explore",
  load_skills: [],
  run_in_background: true,
  description: "Find token handling patterns",
  prompt: `
    FIND: Any existing JWT or token implementations
    CONTEXT: src/
    FOCUS: Token generation, validation, refresh patterns
    RETURN: Files and functions if found, or note absence
  `
)

task(
  subagent_type: "librarian",
  load_skills: [],
  run_in_background: true,
  description: "Find JWT best practices",
  prompt: `
    FIND: JWT authentication best practices
    FOCUS: Token expiration, refresh strategies, security
    SKIP: Basic tutorials
    RETURN: Key recommendations and implementation patterns
  `
)

task(
  subagent_type: "librarian",
  load_skills: [],
  run_in_background: true,
  description: "Find refresh token patterns",
  prompt: `
    FIND: Refresh token implementation patterns
    FOCUS: Storage, rotation, revocation
    RETURN: Security recommendations
  `
)
```

**Süre:** 4 ajan paralel→ max(her birinin süresi)≈ 30 saniye

---

### ADIM 3: Sonuçların Toplanması

```
SİSTEM BİLDİRİMLERİ:
<system-reminder>Background task bg_001 completed</system-reminder>
<system-reminder>Background task bg_002 completed</system-reminder>
<system-reminder>Background task bg_003 completed</system-reminder>
<system-reminder>Background task bg_004 completed</system-reminder>

SISYPHUS:
// Sonuçları topla
const authImplementation = background_output(task_id="bg_001")
const tokenPatterns = background_output(task_id="bg_002")
const jwtBestPractices = background_output(task_id="bg_003")
const refreshTokenPatterns = background_output(task_id="bg_004")

// Wisdom accumulation aktifleştir
// Bulunan pattern'leri sonraki görevlere geçir
```

---

### ADIM 4: Planlama (Prometheus)

```
// Karmaşık iş → Planlama gerekli

@plan "Convert session authentication to JWT with refresh tokens"

PROMETHEUS MÜLAKAT MODU:
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  P: "Current auth implementation buldunuz mu?"                             │
│  K: [Explore sonuçlarından] Evet, session middleware ve user service      │
│                                                                             │
│  P: "JWT token süresi ne olmalı?"                                          │
│  K: 15 dakika access, 7 gün refresh                                        │
│                                                                             │
│  P: "Token storage nerede olacak?"                                         │
│  K: httpOnly cookie (XSS protection)                                       │
│                                                                             │
│  P: "Refresh token rotation yapılacak mı?"                                  │
│  K: Evet, her refresh'te yeni refresh token                                │
│                                                                             │
│  P: "Token revocation nasıl çalışacak?"                                    │
│  K: Redis blacklist                                                        │
│                                                                             │
│  [METIS: Boşluk analizi]                                                    │
│  "Kaçırılan: Token blacklist temizleme, concurrent session limit,         │
│   Token theftdetection"│
 │
│  [MOMUS: Plan revizyonu]                                                   │
│  "Plan kalitesi: 92% - Dosya referansları doğrulanmış,                    │
│   Acceptance criteria net"                                                 │
│                                                                             │
│  SONUÇ: .sisyphus/plans/jwt-auth-migration.md                              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### ADIM 5: Plan İçeriği

```markdown
# JWT Authentication Migration Plan

## Overview
Convert session-based authentication to JWT with refresh tokens.

## Tasks

### Phase 1: Infrastructure
- [ ] Create JWT service- Token generation
  - [ ] Access token generation (15 min)
  - [ ] Refresh token generation (7 day)
  - [ ] Token validation- [ ] Create token storage
  - [ ] httpOnly cookie configuration
  - [ ] Secure cookie settings

- [ ] Create Redis blacklist service
  - [ ] Token revocation endpoint
  - [ ] Blacklist cleanup job
  - [ ] TTL configuration

### Phase 2: Migration
- [ ] Update authentication middleware- [ ] Replace session check with JWT validation
  - [ ] Add token refresh logic
  - [ ] Handle token expiration- [ ] Update login endpoint
  - [ ] Return tokens in httpOnly cookie
  - [ ] Remove session creation- [ ] Update logout endpoint
  - [ ] Add token to blacklist
  - [ ] Clear cookies

### Phase 3: Security
- [ ] Add refresh token rotation
- [ ] Add concurrent session limit
- [ ] Add token theft detection
- [ ] Security tests

## Acceptance Criteria
- [ ] Users can login and receive JWT tokens
- [ ] Access tokens expire in15 minutes
- [ ] Refresh tokens work for 7 days
- [ ] Refresh token rotation implemented
- [ ] Token revocation via blacklist works
- [ ] All auth tests pass
- [ ] No session-related code remains
## Files to Modify
- src/middleware/auth.ts
- src/services/auth.service.ts
- src/routes/auth.routes.ts
- src/config/redis.ts (new)
```

---

### ADIM6: Plan Yürütme (Atlas)

```
KULLANICI: /start-work

ATLAS AKTİVASYONU:
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  Atlas: "Plan bulundu: jwt-auth-migration.md"                              │
│  Atlas: "8 görev, 3 faz"                                                   │
│  Atlas: "Faz 1: Infrastructure başlıyor..."                                │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  GÖREV 1: Create JWT service                                          │  │
│  │                                                                       │  │
│  │  Atlas → Category: deep                                              │  │
│  │                                                                       │  │
│  │  task(                                                               │  │
│  │    category: "deep",                                                │  │
│  │    load_skills: [],                                                  │  │
│  │    prompt: `                                                         │  │
│  │      TASK: Create JWT service with token generation                 │  │
│  │      CONTEXT: src/services/                                          │  │
│  │      MUST:                                                           │  │
│  │        - Generate access tokens (15 min expiry)                      │  │
│  │        - Generate refresh tokens (7 day expiry)                     │  │
│  │        - Validate tokens                            │  │
│  │        - Use RS256 algorithm                                         │  │
│  │      MUST NOT:                                                       │  │
│  │        - Use symmetric encryption                                    │  │
│  │        - Store tokens in database                                    │  │
│  │      RETURN: jwt.service.ts file                                     │  │
│  │    `                                                               │  │
│  │  )                                                                   │  │
│  │                                                                       │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│  Sisyphus-Junior (deep): [Çalışıyor...%10]│                         │"┌─────────────────────────────────────────────────────────────────────────────┘

  WISDOM ACCUMULATION:│                  Bulunan pattern'ler sonraki görevlere geçirilir│                  - Token format: RS256│                  - Expiry: 15min access, 7day refresh
│                  - Storage: httpOnly cookie│```

---

### ADIM 7: Kategori Bazlı Delegasyon

```typescript
// Atlas farklı görevler için farklı kategoriler kullanır:

// Görev 1: JWT Service (Backend logic) → deep
task(
  category: "deep",
  prompt: "Create JWT service..."
)

// Görev 2: Auth Middleware (Backend) → unspecified-high
task(
  category: "unspecified-high",
  prompt: "Update authentication middleware..."
)

// Görev 3: Frontend Token Handling (Frontend) → visual-engineering
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux"],
  prompt: "Add token refresh handling to frontend..."
)

// Görev 4: Tests (Quick tasks) → quick
task(
  category: "quick",
  prompt: "Add unit tests for JWT service..."
)
```

---

### ADIM 8: Doğrulama

```
HER GÖREV SONRASI:

Atlas: Görev 1 tamamlandı, doğrulanıyor...│                     lsp_diagnostics → Hata yok│
│                     Tests → Başarılı
│                     Code review → OK│                     │
│  Atlas: Görev 1 DOĞRULANDI
│  Wisdom güncellendi: JWT service pattern'leri kaydedildi
│
│  GEÇİCİ GÖREV:
│  ├─ [x] Create JWT service
│  ├─ [ ] Create token storage
│  └─ [ ] Create Redis blacklist...
│
│  Atlas: Görev 2 başlıyor...
```

---

## 📊 Tam İş Akışı Şeması

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    BAŞTAN SONA İŞ AKIŞI                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. KULLANICI GİRDİSİ                                                       │
│     "Convert auth to JWT"                                                  │
│       │                                                                     │
│       ▼                                                                     │
│  2. INTENT GATE (Sisyphus)                                                  │
│     Tür: Implementation│                     │
│     Karmaşıklık: Orta-Yüksek                                              │
│       │                                                                     │
│       ▼                                                                     │
│  3. PARALEL ARAŞTIRMA                                                       │
│     ├─ Explore: Auth implementation│                   │
│     ├─ Explore: Token patterns                                             │
│     ├─ Librarian: JWT best practices                                       │
│     └─ Librarian: Refresh token patterns                                    │
│       │                                                                     │
│       ▼                                                                     │
│  4. SONUÇLARI TOPLA│     Wisdom accumulation                               │
│       │                                                                     │
│       ▼                                                                     │
│  5. PLANLAMA (Prometheus)                                                   │
│     ├─ Mülakat: Gereksinimleri netleştir                                   │
│     ├─ Metis: Boşluk analizi                                               │
│     └─ Momus: Plan revizyonu                                               │
│       │                                                                     │
│       ▼                                                                     │
│  6. PLAN OLUŞTUR                                                            │
│     .sisyphus/plans/jwt-auth-migration.md                                   │
│       │                                                                     │
│       ▼                                                                     │
│  7. /start-work (Atlas)                                                     │
│     ├─ Faz 1: Infrastructure                                               │
│     │   ├─ Görev 1: deep category                                          │
│     │   ├─ Görev 2: unspecified-high category                             │
│     │   └─ Görev 3: deep category                                          │
│     ├─ Faz 2: Migration                                                     │
│     │   ├─ Görev 4: unspecified-high category                             │
│     │   └─ Görev 5: visual-engineering category                            │
│     └─ Faz 3: Security                                                      │
│         ├─ Görev 6: ultrabrain category                                    │
│         ├─ Görev 7: deep category                                          │
│         └─ Görev 8: quick category                                          │
│       │                                                                     │
│       ▼                                                                     │
│  8. DOĞRULAMA (Her görev sonrası)                                          │
│     ├─ lsp_diagnostics                                                      │
│     ├─ Tests                                                                │
│     └─ Code review                                                          │
│       │                                                                     │
│       ▼                                                                     │
│  9. BİTİRİ│                     %100 TAMAMLAMA GARANTİSİ │
│             Todo Enforcer                                                   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🎓 Öğrenilen Dersler

### 1. Intent Gate Kullanımı

```
KULLANICI GİRDİSİ               INTENT CLASSIFICATION         AKSİYON
─────────────────────────────────────────────────────────────────────────
"Fix the bug"                → Fix                   → Direct fix
"How does X work?"            → Research              → Explore/Librarian
"Add feature X"               → Implementation        → ultrawork
"Refactor X"                  → Implementation        → Plan + Execute
"Review this design"          → Evaluation            → Oracle
```

### 2. Paralelizasyon Stratejisi

```
BAĞIMSIZ GÖREVLER → PARALEL BAŞLAT
├─ Explore: Code patterns
├─ Explore: Config patterns
├─ Librarian: Docs│                     └─ Librarian: Best practices

BAĞIMLI GÖREVLER → SERIAL YAP
├─ Research (parallel)│                     ├─ Plan (serial, research'tan sonra)
└─ Execute (serial, plan'den sonra)
```

### 3. Kategori Seçimi

```
İŞ TÜRÜ                      KATEGORİ              MODEL
─────────────────────────────────────────────────────────
Frontend/UI                  visual-engineering    Gemini 3.1 Pro
Backend logic                deep                  GPT-5.3 Codex
Quick fix                    quick                 Gemini Flash
Architecture decision         ultrabrain            GPT-5.4 XHigh
Documentation                writing               Gemini Flash
```

### 4. Skill Kombinasyonu

```
GÖREV                        KATEGORİ              SKILLS
─────────────────────────────────────────────────────────────
Frontend implementation       visual-engineering    frontend-ui-ux, playwright
Git commits                  quick                 git-master
Browser testing              visual-engineering    playwright
Deep architecture            ultrabrain            (none - pure reasoning)
```

---

## 📚 Sonraki Adımlar

Bu eğitimde öğrendiklerinizi uygulamak için:

1. **Konfigürasyonunuzu uygulayın:**
   ```bash
   cp oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
   ```

2. **İlk projenizi başlatın:**
   ```bash
   cd your-project
   opencode
   /init-deep
   ```

3. **Basit bir görevle başlayın:**
   ```
   ulw add a simple health check endpoint
   ```

4. **Planlama modunu deneyin:**
   ```
   @plan "implement user authentication system"
   ```

5. **Paralel arama yapın:**
   ```typescript
   // Aynı anda birden fazla Explore başlatın
   task(subagent_type="explore", run_in_background=true, ...)
   task(subagent_type="librarian", run_in_background=true, ...)
   ```

---

## ✅ Eğitim Tamamlandı!

Tüm bölümler tamamlandı:

- [x] BÖLÜM 1: Konfigürasyon Dosyalarını Özelleştirme
- [x] BÖLÜM 2: Ajanları Doğru Durumlar İçin Kullanma
- [x] BÖLÜM 3: Kategori Sistemi ve Model Seçimi
- [x] BÖLÜM 4: Paralel Çalışma
- [x] BÖLÜM 5: Skills, Hooks ve MCP
- [x] BÖLÜM 6: Pratik Senaryo

**Sorularınız için:** Bu eğitim materyallerini referans alarak kendi projelerinizde uygulayabilirsiniz. Her bölüm ayrıntılı örnekler ve açıklamalar içeriyor.