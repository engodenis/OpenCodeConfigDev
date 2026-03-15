# Plan: OpenCode Go Provider Model Dağılımı Optimizasyonu

## 🔍 Problem Tanımı

- **Sorun**: `google/gemini-3.1-pro-preview` modeli yoğun kullanımdan dolayı "gemini is way too hot" hatası veriyor
- **Etki**: Sisyphus ve diğer yüksek iş yükü agent'lar çalışamıyor
- **Çözüm**: OpenCode Go provider modellerine geçiş (Kimi K2.5, MiniMax M2.5, GLM-5)

## 📊 OpenCode Go Model Karşılaştırması

| Model | Input ($/M) | Output ($/M) | Aylık İstek | Kullanım Senaryosu |
|-------|-------------|--------------|-------------|-------------------|
| **Kimi K2.5** | $0.60 | $3.00 | 9,250 | Orta-yüksek iş yükü (best value) |
| **MiniMax M2.5** | $0.30 | $1.20 | 150,000 | Yüksek hacim, küçük işler |
| **GLM-5** | $1.00 | $3.20 | 5,750 | Yüksek kalite gereken işler |

## ✅ Değişiklik Kararları

### 1. AGENTLAR

| Agent | Mevcut Model | Yeni Model | Gerekçe |
|-------|-------------|------------|---------|
| **sisyphus** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Orchestration için güçlü reasoning, 3x daha ucuz |
| **sisyphus.ultrawork** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Yüksek token, K2.5 yeterli |
| **oracle** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Salt okunur analiz için ideal |
| **prometheus** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Planlama için güçlü mantık |
| **metis** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Gap analizi için reasoning |
| **momus** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Plan review için tutarlılık |
| **atlas** | `google/gemini-3.1-pro-preview` | `opencode-go/minimax-m2.5` | Hızlı exec, en ucuz |
| **hephaestus** | `openai/gpt-5.3-codex` | `opencode-go/glm-5` | Yüksek kalite gereken otonom iş |
| **librarian** | `opencode/glm-4.7-free` | `opencode/glm-4.7-free` | Değişiklik yok - zaten ücretsiz |
| **explore** | `opencode/gpt-5-nano` | `opencode/gpt-5-nano` | Değişiklik yok - zaten en ucuz |
| **multimodal-looker** | `google/gemini-3-flash-preview` | `google/gemini-3-flash-preview` | Değişiklik yok - vizyon gerekli |

### 2. KATEGORİLER

| Category | Mevcut Model | Yeni Model | Gerekçe |
|----------|-------------|------------|---------|
| **visual-engineering** | `google/gemini-3.1-pro-preview` | `opencode-go/glm-5` | UI/UX için en yüksek kalite |
| **ultrabrain** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Derin düşünme için güçlü reasoning |
| **deep** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Otonom çalışma |
| **artistry** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Yaratıcı çözümler |
| **unspecified-high** | `google/gemini-3.1-pro-preview` | `opencode-go/kimi-k2.5` | Genel yüksek çaba |
| **quick** | `google/gemini-3-flash-preview` | `opencode-go/minimax-m2.5` | Küçük işler, en yüksek hacim |
| **unspecified-low** | `google/gemini-3-flash-preview` | `opencode-go/minimax-m2.5` | Düşük çaba |
| **writing** | `google/gemini-3-flash-preview` | `opencode-go/minimax-m2.5` | Dokümantasyon |
| **git** | `opencode/gpt-5-nano` | `opencode/gpt-5-nano` | Değişiklik yok |

## 📈 Beklenen Faydalar

1. **Maliyet**: ~60-70% tasarruf (Gemini Pro → OpenCode Go)
2. **Performans**: "Too hot" hataları sona erecek
3. **Kapasite**: Aylık 150K istek (MiniMax ile)
4. **Güvenilirlik**: 3 farklı provider, yedekleme imkanı

## ⚠️ Dikkat Edilecek Noktalar

1. **Vizyon İşleri**: `multimodal-looker` için Gemini Flash kalmalı (görsel analiz gerekli)
2. **Hephaestus**: GLM-5 Codex'in yeteneklerini tam karşılayamayabilir, test edilmeli
3. **Test**: Her değişiklik sonrası `opencode debug config` çalıştırılmalı

## 🔄 Uygulama Adımları

### Adım 1: Geliştirme Dosyasını Güncelle
- Dosya: `opencode_config_dev/oh-my-opencode-optimized.jsonc`
- Tüm model değişikliklerini uygula

### Adım 2: Test
```bash
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
opencode debug config
```

### Adım 3: Production Aktarımı
- Başarılı test sonrası: `opencode_config_dev/oh-my-opencode.json`'a aktar

## 📝 Notlar

- Model ID formatı: `opencode-go/<model-id>` (örn: `opencode-go/kimi-k2.5`)
- OpenCode Go abonelik: Aylık $10, limits: 5h/$12, weekly/$30, monthly/$60
- Kullanım konsoldan takip edilebilir: https://opencode.ai/auth

---

**Plan Oluşturulma**: 2026-03-15  
**Status**: Hazır - Uygulamaya Hazır
