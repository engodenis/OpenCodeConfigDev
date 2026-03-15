# Konfigürasyon Geliştirme Workflow Kuralı

## 📋 Kural

Bu repo'da konfigürasyon geliştirme için aşağıdaki workflow kuralları geçerlidir:

---

## 🔄 Geliştirme Süreci

### Adım 1: Geliştirme
- Tüm konfigürasyon değişiklikleri **`oh-my-opencode-optimized.jsonc`** dosyasında yapılır
- Bu dosya geliştirme ve test ortamıdır
- JSONC formatı sayesinde yorum satırları kullanılabilir

### Adım 2: Test
- Değişiklikler tamamlandıktan sonra konfigürasyon test edilir
- Test komutu:
  ```bash
  # Konfigürasyonu test ortamına kopyala
  cp oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
  
  # Test et
  opencode debug config
  ```
- Eğer test başarılı olursa bir sonraki adıma geçilir

### Adım 3: Aktarım
- Test başarılı olursa, **`oh-my-opencode-optimized.jsonc`** içindeki veriler **`oh-my-opencode.json`** dosyasına aktarılır
- Bu dosya production kullanım için tasarlanmıştır
- Standart JSON formatı kullanılır (yorum desteklemez)

---

## 📁 Dosya Yapısı

| Dosya | Amaç | Format |
|-------|------|--------|
| `oh-my-opencode-optimized.jsonc` | Geliştirme & Test | JSONC (yorum destekli) |
| `oh-my-opencode.json` | Production | Standard JSON |

---

## ✅ Test Kontrol Listesi

Her konfigürasyon değişikliğinde:

- [ ] JSON syntax geçerliliği kontrol edildi
- [ ] OpenCode config yükleme test edildi
- [ ] Tüm agent'lar doğru yüklendi
- [ ] Kategoriler doğru çalışıyor
- [ ] Model atamaları doğru

---

## 🚨 Önemli Notlar

1. **Schema Uyumluluğu**: `oh-my-opencode-optimized.jsonc`'de schema-dışı alanlar yorum olarak tutulur
2. **Güvenli Geliştirme**: Her zaman optimize dosyada geliştir, sonra prod'a aktar
3. **Test Zorunlu**: Production'a aktarmadan önce mutlaka test et

---

## 📝 Kısayol

Test için tek komut:
```bash
# Optimized dosyayı test et
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc && opencode debug config
```

---

## 🔗 İlgili Dosyalar

- [oh-my-opencode-optimized.jsonc](./oh-my-opencode-optimized.jsonc) - Geliştirme dosyası
- [oh-my-opencode.json](./oh-my-opencode.json) - Production dosyası
- [README.md](./README.md) - Genel dokümantasyon