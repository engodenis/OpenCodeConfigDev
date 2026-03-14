#📖 BÖLÜM 1: Konfigürasyon Detayları

## 🎯 Giriş

Bu bölüm, `oh-my-opencode.jsonc` konfigürasyon dosyasının her bir bölümünü detaylıca açıklar. Kendi konfigürasyonunuzu özelleştirmek için bu rehberi kullanabilirsiniz.

---

## 📁 Konfigürasyon Dosya Yapısı

```
~/.config/opencode/
├── opencode.json              # OpenCode temel yapılandırması
└── oh-my-opencode.jsonc      # oh-my-openagent eklenti yapılandırması

# VEYA proje seviyesi:
your-project/
├── .opencode/
│   └── oh-my-opencode.jsonc  # Proje bazlı yapılandırma
```

---

## 📋 Ana Yapı

```jsonc
{
  "$schema": "...",           // JSON şema URL'si
  "agents": { ... },          // Ajan yapılandırmaları
  "categories": { ... },      // Kategori tanımları
  "background_task": { ... }, // Paralel çalışma ayarları
  "sisyphus_agent": { ... },  // Orkestrasyon ayarları
  "experimental": { ... },   // Deneysel özellikler
  "disabled_hooks": [ ... ], // Devre dışı hook'lar
  "runtime_fallback": { ... },// Model yedekleme
  "hashline_edit": true,     // Hash anchoring
  "disabled_mcps": [ ... ]   // Devre dışı MCP'ler
}
```

---

## 🤖 BÖLÜM 1:Ajan Yapılandırmaları

### Temel Yapı

```jsonc
"agents": {
  "<ajan-adı>": {
    "model": "sağlayıcı/model-adı",    // Zorunlu
    "variant": "high",                  // Opsiyonel: low/medium/high/xhigh/max
    "temperature": 0.1,                 // Opsiyonel:0.0-2.0
    "prompt_append": "...",            // Opsiyonel: Sistem istemine eklenecek
    "fallback_models": ["..."],        // Opsiyonel: Yedek modeller
    "permission": { ... },             // Opsiyonel: Araç izinleri
    "thinking": { ... },               // Opsiyonel: Extended thinking
    "tools": { ... }                   // Opsiyonel: Araç kısıtlamaları
  }
}
```

### Ajan Listesi ve Varsayılanları

| Ajan | Varsayılan Model | Görev |
|------|------------------|-------|
| **sisyphus** | Gemini 3.1 Pro | Ana orkestratör |
| **hephaestus** | GPT-5.3 Codex | Otonom derin işçi |
| **prometheus** | Gemini 3.1 Pro | Stratejik planlayıcı |
| **oracle** | Gemini 3.1 Pro | Mimari danışman |
| **librarian** | GLM 4.7 Free | Dokümantasyon arama |
| **explore** | GPT-5 Nano | Pattern keşfi |
| **atlas** | Gemini 3.1 Pro | Todo orkestratörü |
| **metis** | Gemini 3.1 Pro | Boşluk analizi |
| **momus** | Gemini 3.1 Pro | Plan revizyonu |
| **multimodal-looker** | Gemini 3 Flash | Görsel analiz |

### Örnek: Sisyphus Yapılandırması

```jsonc
"sisyphus": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "high",
  "temperature": 0.1,
  
  // Ultrawork modu için özel model
  "ultrawork": {
    "model": "google/gemini-3.1-pro-preview",
    "variant": "max"
  },
  
  // Sistem istemine ekleme
  "prompt_append": "You are the conductor of an AI development team..."
}
```

### Örnek: Oracle Yapılandırması

```jsonc
"oracle": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "high",
  
  // Salt okunur ajan - yazma araçları engelli"permission": {
    "edit": "deny",
    "bash": "deny",
    "write": "deny"
  },
  
  "prompt_append": "You are a read-only architecture consultant..."
}
```

### Örnek: Extended Thinking

```jsonc
"ultrabrain": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "high",
  "temperature": 0.1,
  
  // Derin düşünme modu
  "thinking": {
    "type": "enabled",
    "budgetTokens": 16000
  }
}
```

---

## 🏷️ BÖLÜM 2: Kategori Tanımları

### Kategori Nedir?

Kategori iş türüne göre otomatik model seçimi sağlar. Sisyphus model adı değil, kategori seçer.

### Temel Yapı

```jsonc
"categories": {
  "<kategori-adı>": {
    "model": "sağlayıcı/model-adı",
    "variant": "high",
    "temperature": 0.5,
    "prompt_append": "...",
    "description": "Kategori açıklaması"
  }
}
```

### Built-in Kategoriler

| Kategori | Varsayılan Model | Ne Zaman Kullanılır? |
|----------|------------------|----------------------|
| `visual-engineering` | Gemini 3.1 Pro | Frontend, UI/UX, tasarım |
| `ultrabrain` | Gemini 3.1 Pro | Derin düşünme, mimari |
| `deep` | Gemini 3.1 Pro | Otonom problem çözme |
| `artistry` | Gemini 3.1 Pro | Yaratıcı yaklaşımlar |
| `quick` | Gemini 3 Flash |Basit işler, typo düzeltme |
| `unspecified-low` | Gemini 3 Flash | Genel düşük çaba |
| `unspecified-high` | Gemini 3.1 Pro | Genel yüksek çaba |
| `writing` | Gemini 3 Flash | Dokümantasyon, yazı |

### Örnek: Visual-Engineering

```jsonc
"visual-engineering": {
  "model": "google/gemini-3.1-pro-preview",
  "variant": "high",
  "temperature": 0.7,  // Yaratıcılık için yüksek sıcaklık
  "prompt_append": "You are a designer-turned-developer..."
}
```

### Örnek: Özel Kategori

```jsonc
"git": {
  "model": "opencode/gpt-5-nano",
  "temperature": 0.1,
  "description": "All git operations",
  "prompt_append": "Atomic commits, clear messages, safe operations..."
}
```

---

## ⚡ BÖLÜM 3: Background Task Ayarları

### Paralel Çalışma Kontrolü

```jsonc
"background_task": {
  // Toplam max paralel görev
  "defaultConcurrency": 10,
  
  // Görev zaman aşımı (milisaniye)
  "staleTimeoutMs": 180000,  // 3 dakika
  
  // Sağlayıcı bazlı limitler
  "providerConcurrency": {
    "anthropic": 3,    // Claude API: max 3 paralel
    "openai": 5,       // GPT API: max 5 paralel
    "google": 10,      // Gemini: max 10 paralel
    "opencode": 15     // Ücretsiz: max 15 paralel
  },
  
  // Model bazlı limitler (sağlayıcıyı ezer)
  "modelConcurrency": {
    "anthropic/claude-opus-4-6": 2,// Pahalı model: düşük limit
    "opencode/gpt-5-nano": 20,     // Ucuz model: yüksek limit
    "opencode/glm-4.7-free": 30     // Ücretsiz: çok yüksek
  }
}
```

### Öncelik Sırası

```
modelConcurrency > providerConcurrency > defaultConcurrency
```

---

## 🔧 BÖLÜM 4: Sisyphus Agent Ayarları

### Orkestrasyon Kontrolü

```jsonc
"sisyphus_agent": {
  "disabled": false,// Sisyphus aktif
  "default_builder_enabled": false,  // OpenCode-Builder kapalı
  "planner_enabled": true,           // Prometheus aktif
  "replace_plan": true// Varsayılan plan ajanını değiştir
}
```

---

## 🧪 BÖLÜM 5: Deneysel Özellikler

### Ayarlar

```jsonc
"experimental": {
  // Bağlam penceresi yönetimi
  "truncate_all_tool_outputs": false,
  "aggressive_truncation": true,
  "auto_resume": true,
  "disable_omo_env": false,
  "task_system": true,
  
  // Dinamik bağlam budaması
  "dynamic_context_pruning": {
    "enabled": true,
    "notification": "detailed",
    "turn_protection": {
      "enabled": true,
      "turns": 3// Son 3 tur korunur
    },
    "protected_tools": [
      "task",
      "todowrite",
      "session_read",
      "session_write",
      "session_search"
    ],
    "strategies": {
      "deduplication": { "enabled": true },
      "supersede_writes": { "enabled": true, "aggressive": false },
      "purge_errors": { "enabled": true, "turns": 5 }
    }
  }
}
```

---

## 🪝 BÖLÜM 6: Hook Yapılandırması

### Devre Dışı Bırakma

```jsonc
"disabled_hooks": [
  // Kullanmak istemediğiniz hook'lar
  // "comment-checker",// AI yorum kontrolünü kapat
  // "startup-toast",         // Başlangıç bildirimini kapat
  // "gpt-permission-continuation"  // GPT otomatik devamı kapat
]
```

### Built-in Hook'lar

| Hook | Açıklama |
|------|----------|
| `gpt-permission-continuation` | GPT oturumlarını otomatik devam ettir |
| `todo-continuation-enforcer` | Todo'ları tamamlamaya zorla |
| `context-window-monitor` | Token kullanımını izle |
| `comment-checker` | AI yorumlarını temizle |
| `keyword-detector` | `ulw`, `search` anahtar kelimelerini algıla |
| `ralph-loop` | Self-referential loop |

---

## 🔄 BÖLÜM 7: Runtime Fallback

### API Hatası Durumunda Model Değiştirme

```jsonc
"runtime_fallback": {
  "enabled": true,
  
  // Hangi hata kodlarında yedek model kullan
  "retry_on_errors": [400, 429, 503, 529],
  
  // Max yedekleme denemesi
  "max_fallback_attempts": 3,
  
  // Başarısız modeli bekleme süresi
  "cooldown_seconds": 60,
  
  // Zaman aşımı
  "timeout_seconds": 30,
  
  // Model değişiminde bildirim
  "notify_on_fallback": true
}
```

---

## 📝 BÖLÜM 8: Hashline Edit

### Güvenli Düzenleme

```jsonc
"hashline_edit": true
```

**Açıklama:** Hash anchoring ile güvenli düzenleme. Edit tool'u `LINE#ID` formatı içerir ve dosya değişmediyse düzenlemeyi reddeder.

```
10#ABC| function hello() {
// ABC hash'i dosya değişmediyse düzenleme geçerli
```

---

## 🔌 BÖLÜM 9: MCP Ayarları

### Built-in MCP'ler

| MCP | Sağlayıcı | Ne Yapar? |
|-----|-----------|-----------|
| `websearch` | Exa AI | Web araması |
| `context7` | Context7 | Kütüphane dokümantasyonu |
| `grep_app` | GitHub | Kod araması |

### Devre Dışı Bırakma

```jsonc
"disabled_mcps": [
  // "websearch",    // Web aramasını kapat
  // "context7",     // Dokümantasyon aramasını kapat
  // "grep_app"      // GitHub aramasını kapat
]
```

---

## 🎨 BÖLÜM 10: Ek Yapılandırmalar

### Browser Automation

```jsonc
"browser_automation_engine": {
  "provider": "playwright"  // veya "agent-browser"
}
```

### Tmux Entegrasyonu

```jsonc
"tmux": {
  "enabled": false,  // Arka plan ajanları panellerde görmek için true
  "layout": "main-vertical",
  "main_pane_size": 60
}
```

### Git Master

```jsonc
"git_master": {
  "commit_footer": true,          // Commit'lere metadata ekle
  "include_co_authored_by": true  // Co-author bilgisi ekle
}
```

### Comment Checker

```jsonc
"comment_checker": {
  "custom_prompt": "Review these comments for AI slop..."
}
```

---

## 📊 Hızlı Referans Tablosu

### En Sık Kullanılan Ayarlar

| Ayar | Tür | Açıklama |
|------|-----|----------|
| `agents.<agent>.model` | string | Ajan için model |
| `agents.<agent>.variant` | string | Model varyasyonu |
| `agents.<agent>.temperature` | number | Yaratıcılık seviyesi |
| `categories.<cat>.model` | string | Kategori için model |
| `background_task.defaultConcurrency` | number | Max paralel görev |
| `runtime_fallback.enabled` | boolean | Yedekleme aktif mi |
| `experimental.aggressive_truncation` | boolean | Agresif budama |
| `hashline_edit` | boolean | Hash anchoring |

---

## 🔗 İlgili Dosyalar

- **Konfigürasyon dosyası:** `oh-my-opencode-optimized.jsonc`
- **Ajan kullanım rehberi:** `docs/02-Agent-Usage-Guide.md`
- **Kategori sistemi:** `docs/03-Category-System-Guide.md`
- **Paralel çalışma:** `docs/04-Parallel-Execution-Guide.md`