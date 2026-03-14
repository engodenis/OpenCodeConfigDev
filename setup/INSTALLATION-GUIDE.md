# OpenCode + oh-my-openagent Kurulum Rehberi

> **Bu rehber, OpenCode kurulu ama oh-my-openagent eklentisi olmayan kullanıcılar içindir.**
>
> OpenCode'a bu rehberi vermek için şu komutu yapıştırın:
> ```
> OpenCode kurulu ancak oh-my-openagent eklentisi yok. Aşağıdaki kurulum rehberini sırasıyla uygula:
> 1. oh-my-openagent kurulumunu gerçekleştir (resmi dokümantasyonu takip et)
> 2. https://github.com/engodenis/OpenCodeConfigDev.git reposunu fork et ve clone et
> 3. Konfigürasyon dosyalarını doğru konuma yerleştir
> ```

---

## 🚀 Adım 1: oh-my-openagent Kurulumu

Install and configure oh-my-opencode by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md

---

## 📦 Adım 2: Eğitim Materyalleri Reposunu Kurma

### 2.1 Repository'i Fork Etme

1. Tarayıcıda şu adrese git: https://github.com/engodenis/OpenCodeConfigDev
2. Sağ üst köşeden **Fork** butonuna tıkla
3. Fork ayarlarını tamamla ve **Create fork** butonuna tıkla

### 2.2 Fork'u Klonlama

```bash
# Fork URL'ini kendi kullanıcı adınla değiştir
# Örnek: https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git

# Klonlama
git clone https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git
cd OpenCodeConfigDev
```

### 2.3 Upstream Ayarlama (Opsiyonel ama Önerilen)

```bash
# Orijinal repo'yu upstream olarak ekle
git remote add upstream https://github.com/engodenis/OpenCodeConfigDev.git

# Upstream'den güncellemeleri çek
git fetch upstream

# Ana dalı güncelle
git merge upstream/master
```

---

## ⚙️ Adım 3: Konfigürasyon Dosyalarını Yerleştirme

### 3.1 Optimize Edilmiş Konfigürasyonu Kopyalama

```bash
# Windows
copy opencode_config_dev\oh-my-opencode-optimized.jsonc %USERPROFILE%\.config\opencode\oh-my-opencode.jsonc

# macOS/Linux
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
```

### 3.2 OpenCode Yapılandırmasını Doğrulama

```bash
# Konfigürasyon dosyalarını kontrol et
cat ~/.config/opencode/opencode.json
cat ~/.config/opencode/oh-my-opencode.jsonc  # veya .json

# OpenCode'u başlat
opencode
```

---

## ✅ Adım 4: Doğrulama ve İlk Kullanım

### 4.1 Kurulum Doğrulama

OpenCode içinde şu komutları çalıştır:

```
# Versiyon kontrolü
/opencode --version

# Plugin kontrolü
# opencode.json'da "oh-my-opencode" görünmeli
```

### 4.2 İlk Kullanım

OpenCode içinde:

```
# Proje başlatma
/init

# Ultrawork modu deneme
ulw Add a simple health check endpoint to the project

# Planlama modu deneme
@plan "Create a REST API with authentication"
```

---

## 🎯 Hızlı Başlangıç

### Özet

1. **oh-my-openagent kurulumu** için resmi dokümantasyonu takip edin (Adım 1)
2. **Repo kurulumu:**
   ```bash
   git clone https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git
   cd OpenCodeConfigDev
   ```
3. **Konfigürasyon:**
   ```bash
   cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
   ```
4. **Başlat:** `opencode` → `/init`

---

## 📚 Kaynaklar

| Kaynak | URL |
|--------|-----|
| OpenCode Dokümantasyonu | https://opencode.ai/docs/tr/ |
| oh-my-openagent GitHub | https://github.com/code-yeongyu/oh-my-openagent |
| oh-my-openagent Overview | https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/overview.md |
| oh-my-openagent Installation | https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/installation.md |
| Bu Eğitim Materyali | https://github.com/engodenis/OpenCodeConfigDev |

---

## 🆘 Sorun Giderme

### Yaygın Sorunlar

| Sorun | Çözüm |
|-------|-------|
| `opencode: command not found` | OpenCode kurulu değil. https://opencode.ai/docs/tr/ |
| `bunx: command not found` | Bun kurulu değil. `curl -fsSL https://bun.sh/install \| bash` |
| Plugin yüklenmedi | `opencode.json` içinde `"plugin": ["oh-my-opencode"]` kontrol et |
| Kimlik doğrulama hatası | `opencode auth login` çalıştır |
| Model bulunamadı | Sağlayıcı kimlik doğrulamasını kontrol et |

### Hata Raporlama

- GitHub Issues: https://github.com/code-yeongyu/oh-my-openagent/issues
- Discord: https://discord.gg/PUwSMR9XNk

---

## 🎉 Kurulum Sonrası

Teşrikkürler! Artık OpenCode + oh-my-openagent kullanmaya hazırsınız.

Sonraki adımlar:
1. Bir proje dizinine git: `cd /path/to/project`
2. OpenCode'u başlat: `opencode`
3. Proje başlat: `/init`
4. İlk görevi dene: `ulw Add a simple feature`

**İpucu:** `ulw` (ultrawork) modunu kullanarak ajanın her şeyi otomatik halletmesine izin verebilirsiniz!