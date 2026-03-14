# OpenCode + oh-my-openagent Kurulum Rehberi

> **Bu rehber, OpenCode kurulu ama oh-my-openagent eklentisi olmayan kullanıcılar içindir.**
>
> OpenCode'a bu rehberi vermek için şu komutu yapıştırın:
> ```
> OpenCode kurulu ancak oh-my-openagent eklentisi yok. Aşağıdaki kurulum rehberini sırasıyla uygula:
> 1. Gerekli tool'ları kontrol et (git, bun/node)
> 2. oh-my-openagent kurulumunu gerçekleştir
> 3. https://github.com/engodenis/OpenCodeConfigDev.git reposunu fork et ve clone et
> 4. Konfigürasyon dosyalarını doğru konuma yerleştir
> ```

---

## 📋 Kurulum Öncesi Kontroller

### Gerekli Tool'lar

| Tool | Gereklilik | Kontrol Komutu |
|------|------------|----------------|
| **Git** | Zorunlu | `git --version` |
| **Bun** veya **Node.js** | Zorunlu (biri) | `bun --version` veya `node --version` |
| **OpenCode** | Zorunlu | `opencode --version` |

### Kontrol Komutları

```bash
# Git kontrolü
if command -v git &> /dev/null; then
    echo "✅ Git kurulu: $(git --version)"
else
    echo "❌ Git kurulu değil!"
    echo "   Linux/macOS: brew install git veya apt install git"
    echo "   Windows: https://git-scm.com/download/win"
fi

# Bun veya Node kontrolü
if command -v bun &> /dev/null; then
    echo "✅ Bun kurulu: $(bun --version)"
elif command -v node &> /dev/null; then
    echo "✅ Node.js kurulu: $(node --version)"
else
    echo "❌ Bun veya Node.js kurulu değil!"
    echo "   Bun: curl -fsSL https://bun.sh/install | bash"
    echo "   Node.js: https://nodejs.org/"
fi

# OpenCode kontrolü
if command -v opencode &> /dev/null; then
    echo "✅ OpenCode kurulu: $(opencode --version)"
else
    echo "❌ OpenCode kurulu değil!"
    echo "   Kurulum: https://opencode.ai/docs/tr/"
fi
```

---

## 🚀 Adım 1: oh-my-openagent Kurulumu

### 1.1 Abonelik Durumu Kontrolü

Kurulumdan önce şu soruları yanıtlayın:

| # | Soru | Yanıt |
|---|------|-------|
| 1 | Claude Pro/Max aboneliğin var mı? | evet/hayır |
| 2 | Varsa, max20 (20x mod) kullanıyor musun? | evet/hayır |
| 3 | OpenAI/ChatGPT Plus aboneliğin var mı? | evet/hayır |
| 4 | Gemini modellerini kullanacak mısın? | evet/hayır |
| 5 | GitHub Copilot aboneliğin var mı? | evet/hayır |
| 6 | OpenCode Zen (opencode/ modelleri) erişimin var mı? | evet/hayır |
| 7 | Z.ai Coding Plan aboneliğin var mı? | evet/hayır |
| 8 | OpenCode Go ($10/ay) aboneliğin var mı? | evet/hayır |

### 1.2 Kurulum Komutları

**Yanıtlara göre kurulum komutu:**

```bash
# TAM ABONELİK (Tüm native abonelikler)
bunx oh-my-opencode install --no-tui --claude=max20 --openai=yes --gemini=yes --copilot=no

# SADECE CLAUDE
bunx oh-my-opencode install --no-tui --claude=yes --gemini=no --copilot=no

# CLAUDE + OPENAI
bunx oh-my-opencode install --no-tui --claude=yes --openai=yes --gemini=no --copilot=no

# SADECE GITHUB COPILOT
bunx oh-my-opencode install --no-tui --claude=no --gemini=no --copilot=yes

# Z.AI ABONELİĞİ
bunx oh-my-opencode install --no-tui --claude=yes --gemini=no --copilot=no --zai-coding-plan=yes

# SADECE OPENCODE ZEN
bunx oh-my-opencode install --no-tui --claude=no --gemini=no --copilot=no --opencode-zen=yes

# SADECE OPENCODE GO
bunx oh-my-opencode install --no-tui --claude=no --openai=no --gemini=no --copilot=no --opencode-go=yes

# HİÇBİR ABONELİK YOK
bunx oh-my-opencode install --no-tui --claude=no --gemini=no --copilot=no
```

### 1.3 Alternatif Kurulum (NPX)

```bash
# NPM ile kurulum
npx oh-my-opencode install
```

### 1.4 Kurulum Doğrulama

```bash
# OpenCode sürümü kontrolü (1.0.150+ olmalı)
opencode --version

# Plugin'in yüklendiğini kontrol et
cat ~/.config/opencode/opencode.json

# Bu dosyada "oh-my-opencode" plugin dizisinde görünmeli
```

---

## 🔑 Adım 2: Sağlayıcı Kimlik Doğrulama

### 2.1 Anthropic (Claude) Kimlik Doğrulama

```bash
opencode auth login
# Provider: Anthropic seç
# Login method: Claude Pro/Max seç
# Tarayıcıda OAuth akışını tamamla
```

### 2.2 Google Gemini Kimlik Doğrulama

Gemini için `opencode-antigravity-auth` plugin'i gerekli:

```bash
# Plugin'i ekle (opencode.json'a)
# "plugin": ["oh-my-opencode", "opencode-antigravity-auth@latest"]

# Kimlik doğrulama
opencode auth login
# Provider: Google seç
# Login method: OAuth with Google (Antigravity) seç
```

### 2.3 GitHub Copilot Kimlik Doğrulama

```bash
opencode auth login
# Provider: GitHub seç
# OAuth ile kimlik doğrula
```

---

## 📦 Adım 3: Eğitim Materyalleri Reposunu Kurma

### 3.1 Repository'i Fork Etme

1. Tarayıcıda şu adrese git: https://github.com/engodenis/OpenCodeConfigDev
2. Sağ üst köşeden **Fork** butonuna tıkla
3. Fork ayarlarını tamamla ve **Create fork** butonuna tıkla

### 3.2 Fork'u Klonlama

```bash
# Fork URL'ini kendi kullanıcı adınla değiştir
# Örnek: https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git

# Klonlama
git clone https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git
cd OpenCodeConfigDev
```

### 3.3 Upstream Ayarlama (Opsiyonel ama Önerilen)

```bash
# Orijinal repo'yu upstream olarak ekle
git remote add upstream https://github.com/engodenis/OpenCodeConfigDev.git

# Upstream'den güncellemeleri çek
git fetch upstream

# Ana dalı güncelle
git merge upstream/master
```

---

## ⚙️ Adım 4: Konfigürasyon Dosyalarını Yerleştirme

### 4.1 Optimize Edilmiş Konfigürasyonu Kopyalama

```bash
# Windows
copy opencode_config_dev\oh-my-opencode-optimized.jsonc %USERPROFILE%\.config\opencode\oh-my-opencode.jsonc

# macOS/Linux
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc
```

### 4.2 OpenCode Yapılandırmasını Doğrulama

```bash
# Konfigürasyon dosyalarını kontrol et
cat ~/.config/opencode/opencode.json
cat ~/.config/opencode/oh-my-opencode.jsonc  # veya .json

# OpenCode'u başlat
opencode
```

---

## ✅ Adım 5: Doğrulama ve İlk Kullanım

### 5.1 Kurulum Doğrulama

OpenCode içinde şu komutları çalıştır:

```
# Versiyon kontrolü
/opencode --version

# Plugin kontrolü
# opencode.json'da "oh-my-opencode" görünmeli
```

### 5.2 İlk Kullanım

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

## 🎯 Hızlı Başlangıç Komutları

### Tüm Adımları Tek Seferde Çalıştır (Bash Script)

```bash
#!/bin/bash

# Adım 0: Kontroller
echo "=== Kontroller ==="

# Git kontrolü
if ! command -v git &> /dev/null; then
    echo "❌ Git kurulu değil!"
    exit 1
else
    echo "✅ Git: $(git --version)"
fi

# Bun veya Node kontrolü
if command -v bun &> /dev/null; then
    echo "✅ Bun: $(bun --version)"
elif command -v node &> /dev/null; then
    echo "✅ Node: $(node --version)"
else
    echo "❌ Bun veya Node.js kurulu değil!"
    exit 1
fi

# OpenCode kontrolü
if ! command -v opencode &> /dev/null; then
    echo "❌ OpenCode kurulu değil!"
    echo "   Kurulum: https://opencode.ai/docs/tr/"
    exit 1
else
    echo "✅ OpenCode: $(opencode --version)"
fi

# Adım 1: oh-my-openagent kurulumu
echo ""
echo "=== oh-my-openagent Kurulumu ==="
echo "Abonelik durumunuza göre kurulum komutunu çalıştırın:"
echo ""
echo "Sadece Claude için:"
echo "  bunx oh-my-opencode install --no-tui --claude=yes --gemini=no --copilot=no"
echo ""
echo "Tüm abonelikler için:"
echo "  bunx oh-my-opencode install --no-tui --claude=max20 --openai=yes --gemini=yes --copilot=no"
echo ""

# Adım 2: Repo klonlama (örnek)
echo "=== Repo Kurulumu ==="
echo "1. https://github.com/engodenis/OpenCodeConfigDev adresine git"
echo "2. Fork butonuna tıkla"
echo "3. git clone https://github.com/KULLANICI_ADIN/OpenCodeConfigDev.git"
echo "4. cd OpenCodeConfigDev"
echo ""

# Adım 3: Konfigürasyon kopyalama
echo "=== Konfigürasyon ==="
echo "cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc"
echo ""

# Adım 4: Doğrulama
echo "=== Doğrulama ==="
echo "opencode --version"
echo "cat ~/.config/opencode/opencode.json"
echo ""

echo "Kurulum tamamlandı! 🎉"
```

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