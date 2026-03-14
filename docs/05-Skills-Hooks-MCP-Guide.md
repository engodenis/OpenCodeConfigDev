# 📖 BÖLÜM 5: Skills, Hooks ve MCP

## 🎯 Giriş

Bu üç bileşen, oh-my-openagent'in en güçlü özellikleridir:

| Bileşen | Ne Yapar? | Örnek |
|---------|-----------|-------|
| **Skills** | Uzmanlık alanı + MCP araçları enjekte eder | `git-master`.atomic commits, style detection |
| **Hooks** | Yaşam döngüsü olaylarına müdahale eder | `keyword-detector` →`ulw` algılar |
| **MCP** | Dış dünyaya bağlantı sağlar | `websearch`, `context7`, `grep_app` |

---

## 🛠️ SKILLS (Beceriler)

### Skill Nedir?

Skill, bir ajana **uzmanlık bilgisi** ve **MCP araçları** enjekte eden bir mekanizmadır.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SKILL YAPISI                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   SKILL = UZMANLIK + MCP SUNUCULARI                                         │
│                                                                             │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │  git-master Skill                                                    │  │
│   │                                                                      │  │
│   │  UZMANLIK:                                                           │  │
│   │  • Atomic commits: 3+ dosya = 2+ commit                              │  │
│   │  • Style detection: Son 30 commit analizi                          │  │
│   │  • Rebase surgery: History rewriting                                │  │
│   │                                                                      │  │
│   │  MCP SUNUCULARI: (yok - yerel araçlar)                               │  │
│   │  • git tool kullanımı                                                │  │
│   │                                                                      │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │  playwright Skill                                                    │  │
│   │                                                                      │  │
│   │  UZMANLIK:                                                           │  │
│   │  • Browser automation expertise                                      │  │
│   │  • Screenshot capabilities                                          │  │
│   │  • Form filling, navigation                                          │  │
│   │                                                                      │  │
│   │  MCP SUNUCULARI:                                                     │  │
│   │  npx @playwright/mcp@latest                                         │  │
│   │                                                                      │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Built-in Skills

| Skill | Trigger | Ne Yapar? |
|-------|---------|-----------|
| `git-master` | commit, rebase, "who wrote" | Atomic commits, style detection, rebase surgery |
| `playwright` | Browser tasks, testing | Browser automation via MCP |
| `playwright-cli` | Browser on CLI | Playwright CLI integration |
| `agent-browser` | Browser automation | Agent-browser CLI |
| `dev-browser` | Stateful browser | Persistent page state |
| `frontend-ui-ux` | UI/UX tasks | Design-first thinking |

---

### Skill Kullanımı

#### Temel Syntax

```typescript
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux", "playwright"],  // Skill'ler yükle
  prompt: "Create responsive dashboard"
)
```

#### Örnek 1: Git Master

```
# Tetikleyiciler:
"commit these changes"
"rebase onto main"
"who wrote this code?"
"when was X added?"

# Ne yapar:
git-master:
  1. Son 30 commit'i analiz eder
  2. Commit stilini belirler (Korean? English? Semantic?)
  3. Atomic commits uygular:
     - 3+ dosya = 2+ commit
     - 5+ dosya = 3+ commit
     - 10+ dosya = 5+ commit
  4. Mesaj formatını korur
```

#### Örnek 2: Frontend UI/UX

```typescript
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux", "playwright"],
  prompt: `
    Create a pricing table with:
    - Animated hover effects
    - Dark mode support
    - Mobile responsive
  `
)

// Skill ne enjekte eder:
// frontend-ui-ux:
//   - Design Process: Purpose, Tone, Constraints
//   - Aesthetic Direction: Choose extreme
//   - Typography: Distinctive fonts
//   - Color: Cohesive palettes
//   - Motion: High-impact animations
//
// playwright:
//   - Browser automation MCP
//   - Screenshot verification
//   - Live testing
```

---

### Custom Skill Oluşturma

#### Dosya Yapısı

```
.opencode/
└── skills/
    └── my-skill/
        └── SKILL.md
```

#### SKILL.mdFormat

```markdown
---
name: my-skill
description: My custom skill forspecific domain
mcp:
  my-mcp:
    command: npx
    args: ["-y", "my-mcp-server"]
---

# My Skill Prompt

You are an expert in [DOMAIN].Follow these principles:

1. PRINCIPLE 1
2. PRINCIPLE 2
3. PRINCIPLE 3

When working on [DOMAIN] tasks:
- DO: ...
- DON'T: ...
```

#### Örnek: Kubernetes Skill

```markdown
---
name: kubernetes-expert
description: Kubernetes deployment and management expertise
mcp:
  kubernetes-mcp:
    command: kubectl-mcp-server
---

# Kubernetes Expert Skill

You are a Kubernetes expert. Follow these principles:

## Deployment Best Practices
- Use Deployments for stateless apps
- Use StatefulSets for stateful apps
- Always set resource limits
- Use ConfigMaps for config, Secrets for sensitive

## Label Convention
- app: application name
- version: semantic version
- environment: dev/staging/prod

## When Debugging
1. Check pod status: kubectl get pods
2. Check logs: kubectl logs <pod>
3. Check events: kubectl describe pod <pod>
4. Check resource usage: kubectl top pods

## DO
- Use namespace isolation
- Implement health checks
- Use HorizontalPodAutoscaler

## DON'T
- Hardcode values in manifests
- Run pods as root
- Ignore security contexts
```

#### Kullanım

```typescript
task(
  category: "deep",
  load_skills: ["kubernetes-expert"],
  prompt: "Debug why my deployment is stuck in CrashLoopBackOff"
)
```

---

## 🪝 HOOKS (Kancalar)

### Hook Nedir?

Hook, yaşamdöngüsü olaylarına müdahale eden fonksiyonlardır.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         HOOK OLAYLARI                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   KULLANICI MESAJI                                                          │
│       │                                                                     │
│       ▼                                                                     │
│   ┌─────────────────────────────────────────────────────────────────────┐ │
│   │  MESSAGE HOOKS                                                        │ │
│   │  • keyword-detector → "ulw" algılar                                  │ │
│   │  • auto-slash-command → Komutları otomatik çalıştır                  │ │
│   └─────────────────────────────────────────────────────────────────────┘ │
│       │                                                                     │
│       ▼                                                                     │
│   ┌─────────────────────────────────────────────────────────────────────┐ │
│   │  PARAMS HOOKS                                                         │ │
│   │  • think-mode → "think deeply" algılar                               │ │
│   └─────────────────────────────────────────────────────────────────────┘ │
│       │                                                                     │
│       ▼                                                                     │
│   ┌─────────────────────────────────────────────────────────────────────┐ │
│   │  PRE-TOOL-USE HOOKS                                                   │ │
│   │  • directory-agents-injector → AGENTS.md enjekte eder               │ │
│   │  • rules-injector →Kuralları enjekte eder                           │ │
│   └─────────────────────────────────────────────────────────────────────┘ │
│       │                                                                     │
│       ▼TOOL ÇALIŞIR                                            │
│       │                                                                     │
│       ▼                                                                     │
│   ┌─────────────────────────────────────────────────────────────────────┐ │
│   │  POST-TOOL-USE HOOKS                                                  │ │
│   │  • comment-checker → AI yorumlarını temizler                          │ │
│   │  • background-notification → Task tamamlandı bildirimi               │ │
│   └─────────────────────────────────────────────────────────────────────┘ │
│       │                                                                     │
│       ▼                                                                     │
│   AJAN YANITI                                                              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Built-in Hooks

| Hook | Olay | Ne Yapar? |
|------|------|-----------|
| `keyword-detector` | Message | `ulw`, `search`, `analyze` algılar |
| `think-mode` | Params | "think deeply" → extended thinking |
| `directory-agents-injector` | PreToolUse | AGENTS.md enjekte eder |
| `comment-checker` | PostToolUse | AI yorumlarını temizler |
| `background-notification` | Event | Task tamamlandı bildirimi |
| `todo-continuation-enforcer` | System | Todo'yu takip eder |
| `ralph-loop` | Event | Self-referential loop |
| `start-work` | Message | `/start-work`command handler |
| `gpt-permission-continuation` | Event | GPT otomatik devam |

---

### Hook Devre Dışı Bırakma

```jsonc
"disabled_hooks": [
  "comment-checker",      // AI yorum kontrolünü kapat
  "startup-toast",        // Başlangıç bildirimini kapat
  "gpt-permission-continuation"  // GPT otomatik devamı kapat
]
```

---

### Critical Hook: Todo Continuation Enforcer

Bu hook, ajanın görevi yarıda bırakmasını engeller:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    TODO CONTINUATION ENFORCER                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   AJAN: "Bitti!"                                                            │
│       │                                                                     │
│       ▼                                                                     │
│   ┌─────────────────────────────────────────────────────────────────────┐ │
│   │  HOOK: Todo Continuation Enforcer                                    │ │
│   │                                                                       │ │
│   │  Check: Tüm todo'lar tamam mı?                                        │ │
│   │                                                                       │ │
│   │  ┌─ → EVET → İzin ver, yanıt gönder                                  │ │
│   │  │                                                                     │ │
│   │  └─ → HAYIR → ENGESİ KOY                                              │ │
│   │                                                                       │ │
│   │       [SYSTEM REMINDER - TODO CONTINUATION]                           │ │
│   │       You have incomplete todos!                                     │ │
│   │       Complete ALL before responding:                                 │ │
│   │       - [ ] Implement user service                                     │ │
│   │       - [ ] Add validation                                            │ │
│   │       - [ ] Write tests                                               │ │
│   │                                                                       │ │
│   │       DO NOT respond until all todos are marked completed.           │ │
│   │                                                                       │ │
│   └─────────────────────────────────────────────────────────────────────┘ │
│       │                                                                     │
│       ▼                                                                     │
│   AJAN: "Devam ediyorum..."                                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔌 MCP (Model Context Protocol)

### MCP Nedir?

MCP, ajanların dış dünyaya bağlanmasını sağlayan bir protokoldür.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         MCP MİMARİSİ                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │                      OCODE CLIENT                          │  │
│   │                                                                      │  │
│   │   ┌─────────────────────────────────────────────────────────────┐   │  │
│   │   │                    MCP MANAGER                                │   │  │
│   │   │                                                              │   │  │
│   │   │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │   │  │
│   │   │  │ websearch   │ │ context7    │ │ grep_app    │           │   │  │
│   │   │  │ (Exa AI)    │ │ (Docs)      │ │ (GitHub)    │           │   │  │
│   │   │  └─────────────┘ └─────────────┘ └─────────────┘           │   │  │
│   │   │                                                              │   │  │
│   │   │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │   │  │
│   │   │  │ playwright  │ │ filesystem  │ │ custom      │           │   │  │
│   │   │  │ (Browser)    │ │ (Files)     │ │ (Your MCP)  │           │   │  │
│   │   │  └─────────────┘ └─────────────┘ └─────────────┘           │   │  │
│   │   │                                                              │   │  │
│   │   └─────────────────────────────────────────────────────────────┘   │  │
│   │                                                                      │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │                      EXTERNAL WORLD                                   │  │
│   │                                                                      │  │
│   │   Internet ←→ GitHub ←→ Docs ←→ Browser ←→ FileSystem               │  │
│   │                                                                      │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Built-in MCP'ler

| MCP | Sağlayıcı | Ne Yapar? |
|-----|-----------|-----------|
| `websearch` | Exa AI | Web araması |
| `context7` | Context7 | Librarydokümantasyonu |
| `grep_app` | GitHub | Kod araması |

### MCP Kullanımı

```typescript
// Web Araması
websearch_web_search_exa({
  query: "React 19 new features",
  numResults: 5
})

// Dokümantasyon Araması
context7_resolve-library-id({
  libraryName: "react",
  query: "How to use suspense"
})
context7_query-docs({
  libraryId: "/facebook/react",
  query: "React 19 use() hook"
})

// GitHub Kod Araması
grep_app_searchGitHub({
  query: "useEffect cleanup",
  language: ["TypeScript"]
})
```

---

### MCP Devre Dışı Bırakma

```jsonc
"disabled_mcps": [
  "websearch",    // Web aramasını kapat
  "context7",     // Dokümantasyon aramasını kapat
  "grep_app"      // GitHub aramasını kapat
]
```

---

### Skill-Embedded MCP

Skill'ler kendi MCP sunucularını taşıyabilir:

```typescript
// playwright skill yüklendiğinde:
load_skills: ["playwright"]

// Otomatik olarak MCP başlatılır:
// npx @playwright/mcp@latest

// Ve araçlar kullanılabilir olur:
// - playwright_navigate
// - playwright_screenshot
// - playwright_click
// - ...
```

**Avantaj:**
- Context bloat olmaz
- Skill ile scoped
- İş bitince temizlenir

---

## 📚 Pratik Örnekler

### Örnek 1: FrontendProjesi

```typescript
// SKILL + CATEGORY + MCP kombinasyonu

task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux", "playwright"],
  prompt: `
    Create a responsive pricing page with:
    - Dark mode toggle
    - Animated cards on hover
    - Mobile-first design
    
    After implementation, verify with screenshot.
  `
)

// Ne olur:
// 1. Category: visual-engineering → Gemini 3.1 Pro
// 2. Skill: frontend-ui-ux → Design principles enjekte
// 3. Skill: playwright → Browser MCP başlat
// 4. Ajan çalışır, sonrascreenshot alır
```

### Örnek 2: Git Projesi

```
# Komut:
"Commit these changes with proper atomic commits"

# Ne olur:
# 1. keyword-detector hook: "commit" algılar
# 2. git-master skill yüklenir
# 3. Style detection: Son 30 commit analiz
# 4. Atomic commits uygulanır
# 5. git_master tool kullanılır
```

### Örnek 3: Dokümantasyon Projesi

```typescript
// Paralel dokümantasyon araması

task(
  subagent_type: "librarian",
  load_skills: [],
  run_in_background: true,
  prompt: "Find React 19 documentation about Server Components"
)

task(
  subagent_type: "librarian",
  load_skills: [],
  run_in_background: true,
  prompt: "Find Next.js App Router best practices"
)

// Librarian MCP context7 kullanarak dokümantasyon arar
```

---

## ⚠️ Yaygın Hatalar

### Hata 1: Skill Yüklemeyi Unutmak

```typescript
// ❌ YANLIŞ:
task(
  category: "visual-engineering",
  load_skills: [],
// Skill yok!
  prompt: "Create beautiful UI"
)
// Design guidanceeksik

// ✅ DOĞRU:
task(
  category: "visual-engineering",
  load_skills: ["frontend-ui-ux"],
  prompt: "Create beautiful UI"
)
```

### Hata 2: MCP' Manuel Başlatmaya Çalışmak

```typescript
// ❌ YANLIŞ: MCP' manuel başlat
// Skill zaten yapıyor

// ✅ DOĞRU: Skill kullan, MCP otomatik başlatılır
task(
  load_skills: ["playwright"],
  prompt: "Take screenshot of the page"
)
```

### Hata 3: Hook'ları Anlamamak

```
//SORUN: Ajan "ulw" yazdıyorum ama ultrawork aktif olmuyor

// NEDEN: keyword-detector hook'u devredışı

// ÇÖZÜM:
"disabled_hooks": []  // keyword-detector'ü kaldır
// VEYA
// Hook doğru çalışıyor mu kontrol et
```