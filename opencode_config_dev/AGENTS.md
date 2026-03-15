# OpenCode Konfigürasyon Geliştirme Kuralları

## 📋 Özet

Bu dosya, `oh-my-opencode.json` konfigürasyon dosyasında değişiklik yapılırken izlenmesi gereken kuralları içerir.

---

## 🔄 Konfigürasyon Değişikliği Workflow

### Kural: asla doğrudan `oh-my-opencode.json` dosyasını düzenleme

**Tetikleyici**: Kullanıcı `oh-my-opencode.json` dosyasında değişiklik istediğinde:

```
┌─────────────────────────────────────────────────────────┐
│  ADIM 1: Geliştirme                                      │
│  ↓                                                       │
│  oh-my-opencode-optimized.jsonc dosyasında değişiklik   │
│  ↓                                                       │
│  ADIM 2: Test                                           │
│  ↓                                                       │
│  opencode debug config ile test et                       │
│  ↓                                                       │
│  ADIM 3: Aktarım                                        │
│  ↓                                                       │
│  Başarılı → oh-my-opencode.json'a aktar                 │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Dosya Yapısı

| Dosya | Rol | Format |
|-------|-----|--------|
| `oh-my-opencode-optimized.jsonc` | Geliştirme & Test | JSONC (yorum destekli) |
| `oh-my-opencode.json` | Production | Standard JSON |
| `WORKFLOW.md` | Dokümantasyon | Markdown |

---

## ⚡ Otomatik İşlem Sırası

1. **Tespit**: `oh-my-opencode.json` değişikliği istendiğinde
2. **Yönlendirme**: Değişikliği `oh-my-opencode-optimized.jsonc`'ye yap
3. **Test**: `opencode debug config` ile doğrula
4. **Aktarım**: Başarılı olursa `oh-my-opencode.json`'a aktar

---

## 🧪 Test Komutları

```bash
# Test için
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
opencode debug config
```

---

## 🚨 Önemli Notlar

- **ASLA** doğrudan `oh-my-opencode.json` dosyasında değişiklik YAPMA
- **HER ZAMAN** `oh-my-opencode-optimized.jsonc`'yi kullan
- **TESTİ** atlamadan yap
- **ŞÜPHE** durumunda WORKFLOW.md'yi kontrol et

---

## 🔗 İlgili Dosyalar

- [oh-my-opencode-optimized.jsonc](./oh-my-opencode-optimized.jsonc)
- [oh-my-opencode.json](./oh-my-opencode.json)
- [WORKFLOW.md](./WORKFLOW.md)
