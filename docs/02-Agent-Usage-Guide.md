# 📖 BÖLÜM 2: Ajanları Doğru Durumlar İçin Kullanma

## 🎯 Giriş

oh-my-openagent'de her ajan farklı bir "kişiliğe" ve uzmanlık alanına sahiptir. Bir yazılım ekibinde farklı roller olduğu gibi (mimari, front-end geliştirici, test mühendisi, DevOps uzmanı), AI ekibinde de farklı ajanlar vardır.

Bu rehber, **hangi durumda hangi ajanı çağırmanız gerektiğini** ve **nasıl etkili kullanacağınızı** öğretecek.

---

## 📊 Ajan Karar Verme Akışı

```
ŞU AN NE YAPIYORSUNUZ?
         │
         ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         KARAR AĞACI                                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. KARMAŞIK İŞ Mİ?                                                         │
│     └─ EVET → Sisyphus (ultrawork) veya Prometheus (planlama)              │
│     └─ HAYIR → Aşağıdaki uzman ajanlara git                                 │
│                                                                             │
│  2. NE TÜR BİR İŞ?                                                          │
│     ├─ FRONTEND/UI/UX ────────────────→ visual-engineering kategorisi      │
│     ├─ MİMARİ KARAR ──────────────────→ Oracle                             │
│     ├─ DOKÜMANTASYON/DOC BULMA ────────→ Librarian                         │
│     ├─ KOD PATTERN BULMA ──────────────→ Explore                           │
│     ├─ BASİT DÜZELTME ─────────────────→ quick kategorisi                  │
│     ├─ GÖRSELLER (PDF/IMAGE) ──────────→ Multimodal-Looker                 │
│     └─ DERİN ARAŞTIRMA ────────────────→ Hephaestus                        │
│                                                                             │
│  3. PLANLAMA Mİ LAZIM?                                                      │
│     └─ EVET → Prometheus (mülakat modu)                                     │
│     └─ HAYIR → Doğrudan işe başla                                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🤖 AJAN DETAYLARI

### 1. SISYPHUS - Ana Orkestratör (Proje Yöneticisi)

| Özellik | Değer |
|---------|-------|
| **Roller** | Koordinasyon, delegasyon, %100 bitirme |
| **Model** | Gemini 3.1 Pro (konfigürasyona göre) |
| **Tarz** | İletişimci, talimat takipçisi, disiplinli |
| **Kısıtlama** | Yok - tüm araçlara erişimi var |

#### Ne Zaman Kullanılır?

| Durum | Komut |
|-------|-------|
| **Karmaşık iş (önemli)** | `ulw ` veya `ultrawork ` |
| **Normal iş** | Direkt yazın, Sisyphus varsayılan |
| **Planlama istiyorum** | `@plan "iş tanımı"` veya Tab→Prometheus |

#### Örnek Kullanım

```
# Ultrawork modu - sistem her şeyi halleder
ulw refactor the authentication module to use JWT instead of sessions

# Normal mod - Sisyphus direkt çalışır
Fix the bug where users can't login with special characters in password

# Intent Gate aktif - ne demek istediğinizi anlar
"This authentication system is broken, can you make it work?"
→ Sisyphus: "Araştırma mı istiyorsunuz? Düzeltme mi? Yeniden mi yazayım?"
```

#### Ultrawork Modu Flag'leri

```
ulw [ GöREV ]
    │
    ├─→ Intent Gate: Görevi sınıflandır
    ├─→ Explore/Librarian: Araştır
    ├─→ Delegasyon: Uzman ajanlara dağıt
    ├─→ Paralel çalıştır: Aynı anda birden fazla görev
    ├─→ Doğrulama: lsp_diagnostics, testler
    └─→ %100 BITIRENE KADAR DEVAM ET
```

---

### 2. HEPHAESTUS - Otonom Derin İşçi

| Özellik | Değer |
|---------|-------|
| **Roller** | Derin kodlama, mimari değişiklikler |
| **Model** | GPT-5.3 Codex (özel) |
| **Tarz** | Hedef ver → yap. Minimal rehberlik. |
| **Kısıtlama** | GPT gerektirir, Claude iyi çalışmaz |

#### Ne Zaman Kullanılır?

| Durum | Açıklama |
|-------|----------|
| **Derin mimari work** | "Refactor this monolith into microservices" |
| **Karmaşık debugging** | 15+ dosya içeren hata takibi |
| **Cross-domain synthesis** | Rust core ile TypeScript frontend entegrasyonu |
| **Otonom çalışma** | Detaylı talimat vermeden hedef ver |

#### Nasıl Çağrılır?

```
# Tab tuşu ile ajan değiştir
<TAB> → Hephaestus seç

# Veya direkt
@hephaestus build a plugin system for this application
```

#### Önemli: Neden GPT?

```
HEPHAESTUS İÇİN CLAUDA KULLANMA:

❌ YANLIŞ:
   Hephaestus: "Şimdi yapıyorum..."
   → Claude iletişimcidir, otonom değildir
   → Her adımda "şunu mu yapayım?" diye sorar
   → Verimsiz

✅ DOĞRU:
   Hephaestus: "Hedefi verdim, 3 saat sonra bitti"
   → GPT prensip odaklı çalışır
   → "Nasıl yaparım?" sorusunu kendisi yanıtlar
   → Verimli otonom çalışma
```

---

### 3. PROMETHEUS - Stratejik Planlayıcı

| Özellik | Değer |
|---------|-------|
| **Roller** | Mülakat, kapsam belirleme, plan yazma |
| **Model** | Gemini 3.1 Pro (veya Claude Opus) |
| **Tarz** | Sorgulayan, detaycı, belirsizlik giderici |
| **Kısıtlama** | READ-ONLY - sadece .sisyphus/plans/ yazabilir |

#### Ne Zaman Kullanılır?

| Durum | Açıklama |
|-------|----------|
| **Karmaşık proje** | "Build a REST API with authentication" |
| **Bulatıf kapsam** | Ne yapılacağı belirsiz |
| **Plan gerekli** | Kod yazmadan önce strateji |
| **Belirsizlik** | Gereksinimler net değil |

#### Mülakat Modu

```
Kullanıcı: @plan "Build user authentication system"

PROMETHEUS:
┌─────────────────────────────────────────────────────────────┐
│ "Mevcut kimlik doğrulama sisteminiz nasıl çalışıyor?"      │
│ Kullanıcı: [Açıklama]                                       │
│                                                             │
│ "Hangi kimlik doğrulama yöntemlerini desteklemeli?"        │
│ Kullanıcı: JWT, OAuth2, username/password                  │
│                                                             │
│ "Roller ve izinler nasıl yönetilecek?"                     │
│ Kullanıcı: Admin, Manager, User rolleri...                  │
│                                                             │
│ [METIS DEVREYE GİRER]                                       │
│ "Kaçırılan: Session management, password reset, 2FA..."    │
│                                                             │
│ [MOMUS DEVREYE GİRER]                                       │
│ "Plan kalitesi: 85% - dosya referansları eksik..."         │
└─────────────────────────────────────────────────────────────┘

SONUÇ: .sisyphus/plans/auth-system.md
```

#### Planı Yürütme

```
# Plan oluştuktan sonra
/start-work

# Atlas aktivasyonu
Atlas: "Plan bulundu: auth-system.md"
Atlas: "Görev 1: User model oluşturma..."
Atlas: "Görev 2: JWT service..."
...
```

---

### 4. ORACLE - Salt Okunur Danışman

| Özellik | Değer |
|---------|-------|
| **Roller** | Mimari danışman, code review, debugging |
| **Model** | Gemini 3.1 Pro (veya GPT-5.4) |
| **Tarz** | Derin akıl yürütme, detaylı analiz |
| **Kısıtlama** | Salt okunur - yazamaz, düzenleyemez |

#### Ne Zaman Kullanılır?

| Durum | Açıklama |
|-------|----------|
| **Mimari soru** | "Microservices mi monolith mi?" |
| **Güvenlik değeendirme** | "Bu auth sistemi güvenli mi?" |
| **Performans analizi** | "Neden bu query yavaş?" |
| **Code review** | "Bu PR'ı review et" |
| **3+ başarısız fix** | Aynı hata 3 kez çözülemedi |

#### Örnek Kullanım

```
@oracle should we use GraphQL or REST for our API?

Oracle:
┌─────────────────────────────────────────────────────────────┐
│ GRAPHQL vs REST ANALYSIS:                                   │
│                                                             │
│ PROS of GraphQL:                                            │
│ • Single endpoint, flexible queries                          │
│ • Less over-fetching                                        │
│ • Strong typing with schema                                 │
│                                                             │
│ PROS of REST:                                               │
│ • Simpler to implement                                      │
│ • Better caching (HTTP-level)                               │
│ • More tooling support                                      │
│                                                             │
│ RECOMMENDATION: REST for your case because:                  │
│ 1. Your data model is not deeply nested                     │
│ 2. Team familiarity is higher                               │
│ 3. Caching is critical for your mobile app                  │
│                                                             │
│ CONSIDER GraphQL if: Your frontend needs flexible queries   │
│ from many different views.                                  │
└─────────────────────────────────────────────────────────────┘
```

#### Kritik Kural

```
ORACLE ASLA KOD YAZMAZ:

❌ YANLIŞ:
   @oracle fix this bug for me
   → Oracle: "Bunu yapamam - salt okunurum"

✅ DOĞRU:
   @oracle analyze why this bug might be happening and suggest solutions
   → Oracle: "Root cause analysis: 3 olası neden..."
   → Sonra Sisyphus/Hephaestus çözümü uygular
```

---

### 5. LIBRARIAN - Dokümantasyon ve OSS Arama

| Özellik | Değer |
|---------|-------|
| **Roller** | Dokümantasyon bulma, OSS pattern arama |
| **Model** | GLM 4.7 Free (veya Gemini Flash) |
| **Tarz** | Hızlı, kapsamlı, evidence-based |
| **Kısıtlama** | Salt okunur |

#### Ne Zaman Kullanılır?

| Durum | Açıklama |
|-------|----------|
| **Library dokümantasyonu** | "How to use React 19 use() hook?" |
| **OSS implementation** | "How do other projects handle auth?" |
| **API reference** | "What are the params for useEffect?" |
| **Best practice** | "Best way to handle errors in Express?" |

#### Örnek Kullanım

```
@librarian find React 19 documentation about the new use() hook

Librarian:
┌─────────────────────────────────────────────────────────────┐
│ REACT 19 use() HOOK DOCUMENTATION:                           │
│                                                             │
│ Source: https://react.dev/reference/react/use               │
│                                                             │
│ DESCRIPTION:                                                │
│ The use() hook allows reading resources in render.          │
│ It can read Promises and Context.                           │
│                                                             │
│ USAGE EXAMPLE:                                               │
│ function Comments({ commentsPromise }) {                    │
│   const comments = use(commentsPromise);                    │
│   return comments.map(c => <Comment key={c.id} {...c} />);  │
│ }                                                            │
│                                                             │
│ KEY DIFFERENCES FROM use():                                  │
│ • Can be called conditionally (unlike hooks rules)          │
│ • Suspends until Promise resolves                           │
│ • Works with React Cache                                     │
│                                                             │
│ COMPARE WITH useMemo:                                        │
│ useMemo: Caches computed values                              │
│ use(): Reads resources from server                           │
└─────────────────────────────────────────────────────────────┘
```

#### Paralel Kullanım

```typescript
// BIRDEN FAZLA KAYNAK AYNI ANDA
task(subagent_type="librarian", run_in_background=true, 
     prompt="Find React 19 use() hook docs")

task(subagent_type="librarian", run_in_background=true,
     prompt="Find Express async error handling patterns")

task(subagent_type="librarian", run_in_background=true,
     prompt="Find PostgreSQL connection pooling best practices")

// Sistem tamamlandığında bildirir → sonuçları topla
```

---

### 6. EXPLORE - Kod Tabanı Pattern Keşfi

| Özellik | Değer |
|---------|-------|
| **Roller** | Pattern discovery, convention bulma |
| **Model** | GPT-5 Nano (en hızlı) |
| **Tarz** | Ultra hızlı, pattern odaklı |
| **Kısıtlama** | Salt okunur |

#### Ne Zaman Kullanılır?

| Durum | Açıklama |
|-------|----------|
| **Pattern bulma** | "How is error handling done here?" |
| **Convention keşfi** | "What's the coding style?" |
| **Implementation bulma** | "Where is authentication used?" |
| **Bileşen keşfi** | "What components exist?" |

#### Örnek Kullanım

```
@explore find authentication middleware usage patterns

Explore:
┌─────────────────────────────────────────────────────────────┐
│ AUTHENTICATION MIDDLEWARE PATTERNS FOUND:                   │
│                                                             │
│ FILE: src/middleware/auth.ts                                │
│ PATTERN: JWT verification with role-based access            │
│                                                             │
│ USAGE LOCATIONS:                                             │
│ • src/routes/api/users.ts:12 - User CRUD operations        │
│ • src/routes/api/admin.ts:8 - Admin panel routes            │
│ • src/routes/api/orders.ts:15 - Order processing           │
│                                                             │
│ CONVENTION:                                                  │
│ • Middleware name: authMiddleware                           │
│ • Throws 401 if invalid token                               │
│ • Adds req.user with user object                            │
│                                                             │
│ EXAMPLE CODE:                                                │
│ app.get('/protected', authMiddleware, (req, res) => {      │
│   // req.user is populated                                  │
│ });                                                          │
└─────────────────────────────────────────────────────────────┘
```

---

### 7. MULTIMODAL-LOOKER - Görsel Analiz Uzmanı

| Özellik | Değer |
|---------|-------|
| **Roller** | PDF, image, DIYagram analizi |
| **Model** | Gemini 3 Flash (vizyon-capable) |
| **Tarz** | Görsel understanding |
| **Kısıtlama** | Salt okunur - sadece look_at tool |

#### Ne Zaman Kullanım?

| Durum | Açıklama |
|-------|----------|
| **DIYagram okuma** | Architecture diagram, flowchart |
| **PDF analizi** | Technical documentation PDF |
| **Screenshot** | UI mockup, error screenshot |
| **Görsel debugging** | UI rendering issue |

#### Örnek Kullanım

```
look_at("docs/architecture.png", "Explain this architecture diagram")

Multimodal-Looker:
┌─────────────────────────────────────────────────────────────┐
│ ARCHITECTURE DIAGRAM ANALYSIS:                               │
│                                                             │
│ COMPONENTS IDENTIFIED:                                       │
│ • Frontend (React) - Port 3000                              │
│ • API Gateway (Express) - Port 8080                          │
│ • Auth Service (Node.js) - Port 3001                        │
│ • Database (PostgreSQL) - Port 5432                         │
│ • Cache (Redis) - Port 6379                                  │
│                                                             │
│ DATA FLOW:                                                   │
│ 1. Request → API Gateway                                     │
│ 2. Auth check → Auth Service                                 │
│ 3. Data fetch → Database/Cache                              │
│ 4. Response → Frontend                                       │
│                                                             │
│ POTENTIAL BOTTLENECK:                                        │
│ Direct DB connection from API Gateway (no connection pool)  │
└─────────────────────────────────────────────────────────────┘
```

---

### 8. METIS ve MOMUS - Plan Yönetimi

| Ajan | Görev | Ne Zaman |
|------|-------|----------|
| **Metis** | Boşluk analizi | Prometheus planlarken otomatik |
| **Momus** | Plan revizyonu | Plan yazıldıktan sonra otomatik |

Bu ajanlar manuel çağrılmaz. Prometheus ile otomatik çalışırlar:

```
Prometheus → [Sorular sor] → [Metis: Kaçırılanları bul] → [Momus: Plan doğrula]
```

---

## 🎯 PRATİK KULLANIM SENARYOLARI

### Senaryo 1: Yeni Özellik Ekleme

```
KULLANICI: "Add user profile page with avatar upload"

ADIMLAR:
1. Sisyphus: Intent Gate → "Uygulama"
2. Sisyphus: Explore → Pattern bul
3. Sisyphus: Delege et → visual-engineering category
4. Visual agent: UI implement et
5. Sisyphus: Doğrula → lsp_diagnostics
6. BİTİR
```

### Senaryo 2: Hata Düzeltme

```
KULLANICI: "Users report login fails with special characters"

ADIMLAR:
1. Sisyphus: Intent Gate → "Fix"
2. Explore: Pattern ara → auth middleware
3. Sisyphus: Analiz et → root cause
4. Sisyphus: Düzelt → quick category (basit fix ise)
   veya Hephaestus: Düzelt → (karmaşık ise)
5. Doğrula
```

### Senaryo 3: Mimari Değişiklik

```
KULLANICI: "Should we migrate to microservices?"

ADIMLAR:
1. Oracle: Mimariconsultation yap
2. Prometheus: Planla → @plan "microservice migration"
3. Kullanıcı: Soruları cevapla
4. Metis: Kaçırılanları bul
5. Momus: Planı doğrula
6. /start-work → Atlas yürütür
```

### Senaryo 4: Bilgi Toplama

```
KULLANICI: "How does authentication work in this codebase?"

ADIMLAR:
1. Intent Gate → "Araştırma"
2. Explore: Pattern analyze et
3. Librarian: Dokümantasyon ara
4. Sisyphus: Yanıtla → açıklama üret
```

---

## ⚠️ YAYGIN HATALAR

### Hata 1: Yanlış Ajan Seçimi

```
❌ YANLIŞ:
   @oracle fix this bug
   → Oracle salt okunur!

✅ DOĞRU:
   @sisyphus fix this bug
   → veya direkt sorun, Sisyphus varsayılan
```

### Hata 2: Basit İş İçin Ağır Ajan

```
❌ YANLIŞ:
   @hephaestus change the button color

✅ DOĞRU:
   Direkt yaz: "Change the button color to blue"
   → Sisyphus quick category'ye delege eder
```

### Hata 3: Paralellik Kullanmama

```
❌ YANLIŞ:
   @librarian find React docs
   [Bekle...]
   @explore find patterns
   [Bekle...]

✅ DOĞRU:
   task(subagent_type="librarian", run_in_background=true, ...)
   task(subagent_type="explore", run_in_background=true, ...)
   // Devam et, sistem bildirir
```

---

## 📚 ÖZET TABLOSU

| Ajan | Model | Ne Zaman | Salt Okunur? |
|------|-------|----------|-------------|
| **Sisyphus** | Gemini 3.1 Pro | Varsayılan, karmaşık işler | ❌ |
| **Hephaestus** | GPT-5.3 Codex | Derin mimari work, otonom | ❌ |
| **Prometheus** | Gemini 3.1 Pro | Planlama gerekli | ✅ (plans only) |
| **Oracle** | Gemini 3.1 Pro | Mimari danışman, review | ✅ |
| **Librarian** | GLM 4.7 Free | Dokümantasyon bulma | ✅ |
| **Explore** | GPT-5 Nano | Pattern keşfi | ✅ |
| **Atlas** | Gemini 3.1 Pro | Plan yürütme | ❌ |
| **Metis** | Gemini 3.1 Pro | Boşluk analizi | ✅ |
| **Momus** | Gemini 3.1 Pro | Plan revizyonu | ✅ |
| **Multimodal** | Gemini 3 Flash | Görsel analiz | ✅ |