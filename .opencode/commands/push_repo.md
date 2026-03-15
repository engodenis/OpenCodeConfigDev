---
description: Local değişiklikleri akıllıca commit edip remote repoya push et
agent: sisyphus
model: opencode-go/kimi-k2.5
mode: all
---

# Local Değişiklikleri Commit Et ve Push Yap

## 🎯 Görev

Local repodaki commitlenmemiş değişiklikleri:
1. Analiz et ve akıllıca commit mesajı oluştur
2. Kullanıcıya commit mesajını onaylat
3. Commit işlemini gerçekleştir
4. Kullanıcıdan push onayı al
5. Remote repoya push yap

## 📋 Çalışma Akışı

```
1. Git durumunu kontrol et
   ↓
2. Değişiklikleri analiz et
   ↓
3. Akıllı commit mesajı oluştur
   ↓
4. Kullanıcıdan commit onayı al
   ↓
5. Commit işlemini gerçekleştir
   ↓
6. Kullanıcıdan push onayı al
   ↓
7. Remote repoya push yap
```

## 🔍 Adım 1: Git Durumunu Kontrol Et

### Komutlar
```bash
# Değişiklik durumunu kontrol et
git status --short

# Değişiklik istatistikleri
git diff --stat

# Hangi dosyalar staging'de
git diff --cached --name-only

# Hangi dosyalar staging'de değil
git diff --name-only
```

### Kontrol Listesi
- [ ] Değişiklik var mı?
- [ ] Staging'de dosya var mı?
- [ ] Working directory'de dosya var mı?
- [ ] Yeni dosya mı eklendi?
- [ ] Dosya mı silindi?

## 🧠 Adım 2: Değişiklikleri Analiz Et

### Değişiklik Türünü Tespit Et

**1. Dosya Türüne Göre**
```typescript
interface FileChange {
  path: string;
  type: 'added' | 'modified' | 'deleted' | 'renamed';
  isStaged: boolean;
  category: 'source' | 'test' | 'config' | 'docs' | 'assets';
}
```

**2. Değişiklik Kategorisi**
- **Source Code** (.ts, .js, .py, .java, vb.)
  - Yeni özellik mi?
  - Bug fix mi?
  - Refactoring mi?
  
- **Test Dosyaları** (.test.ts, .spec.js, vb.)
  - Yeni test eklendi mi?
  - Test güncellendi mi?
  
- **Konfigürasyon** (package.json, .env, config files)
  - Bağımlılık eklendi mi?
  - Ayar değişti mi?
  
- **Dokümantasyon** (README.md, docs/, vb.)
  - Belgeler güncellendi mi?
  - Yeni doküman eklendi mi?

**3. Etki Alanı Analizi**
- Kaç dosya değişti?
- Toplam kaç satır eklendi/silindi?
- Hangi modüller etkilendi?
- Breaking change var mı?

## ✍️ Adım 3: Akıllı Commit Mesajı Oluştur

### Conventional Commits Standardı

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Commit Tipi Seçimi

**Değişiklik Türüne Göre:**

| Durum | Tip | Emoji | Örnek |
|-------|-----|-------|-------|
| Yeni özellik | `feat` | ✨ | `feat(auth): add JWT token support` |
| Bug fix | `fix` | 🐛 | `fix(api): resolve null pointer exception` |
| Dokümantasyon | `docs` | 📝 | `docs(readme): update installation guide` |
| Kod stili | `style` | 💄 | `style(lint): fix indentation issues` |
| Refactoring | `refactor` | ♻️ | `refactor(db): optimize query performance` |
| Test | `test` | ✅ | `test(unit): add user service tests` |
| Build/Araç | `chore` | 🔧 | `chore(deps): update dependencies` |
| Performans | `perf` | ⚡ | `perf(cache): improve memory usage` |
| CI/CD | `ci` | 👷 | `ci(github): add auto-deploy workflow` |
| Geri alma | `revert` | ⏪ | `revert: undo breaking change in API` |

### Scope Belirleme

**Otomatik Scope Tespiti:**
- Dosya yolundan modül adını çıkar
- Örnek: `src/auth/login.ts` → `scope: auth`
- Örnek: `components/Button.tsx` → `scope: components`
- Örnek: `docs/api/README.md` → `scope: api`

### Mesaj Yapısı

**Kısa Mesaj (50 karakterden az)**
```
feat(auth): add OAuth2 Google login support
```

**Detaylı Mesaj (Gerekirse)**
```
feat(auth): implement JWT-based authentication

- Add login endpoint with email/password
- Integrate Google OAuth2 provider
- Add token refresh mechanism
- Update user model with auth fields

Closes #123
```

### Akıllı Mesaj Oluşturma Kuralları

**1. Imperative Mood (Emir Kipi)**
- ✅ "add" (ekle)
- ✅ "fix" (düzelt)
- ✅ "update" (güncelle)
- ❌ "added" (eklendi)
- ❌ "fixing" (düzeltme)

**2. İlk Harf Büyük Değil**
- ✅ `feat: add new feature`
- ❌ `feat: Add new feature`

**3. Sonunda Nokta Yok**
- ✅ `feat: add button`
- ❌ `feat: add button.`

**4. Türkçe Değil, İngilizce**
- ✅ `feat: add user authentication`
- ❌ `feat: kullanıcı doğrulama ekle`

## 👤 Adım 4: Kullanıcıdan Commit Onayı Al

### Etkileşim Akışı

```
📊 Değişiklik Analizi:

Dosyalar:
  M  src/auth/login.ts (+45, -12)
  M  src/auth/jwt.ts (+23, -5)
  A  src/auth/oauth.ts (+89, -0)
  M  package.json (+2, -1)

Toplam: 4 dosya, +159 satır, -18 satır

📝 Önerilen Commit Mesajı:

feat(auth): implement JWT and OAuth2 authentication

- Add JWT token generation and validation
- Integrate Google OAuth2 login
- Add token refresh endpoint
- Update dependencies for auth libraries

✅ Bu commit mesajını onaylıyor musunuz?

[1] Evet, commit yap
[2] Hayır, yeni mesaj gireceğim
[3] Değişiklikleri gözden geçir
[4] İptal
```

### Kullanıcı Seçenekleri

**Seçenek 1: Mesaj Onaylandı**
```bash
git add -A
git commit -m "feat(auth): implement JWT and OAuth2 authentication"
```

**Seçenek 2: Yeni Mesaj**
- Kullanıcıdan yeni mesaj iste
- Validate et (50 karakter limiti)
- Commit yap

**Seçenek 3: Değişiklikleri Gözden Geçir**
- `git diff` çıktısını göster
- Dosya içeriklerini özetle
- Tekrar onay iste

**Seçenek 4: İptal**
- İşlemi sonlandır
- "Commit iptal edildi" mesajı ver

## 💾 Adım 5: Commit İşlemini Gerçekleştir

### Staging İşlemi

```bash
# Tüm değişiklikleri staging'e ekle
git add -A

# VEYA sadece belirli dosyaları ekle (kullanıcı tercihine göre)
git add src/auth/
git add package.json
```

### Commit İşlemi

```bash
# Tek satırlık commit
git commit -m "feat(auth): implement JWT authentication"

# VEYA detaylı commit (eğer body varsa)
git commit -m "feat(auth): implement JWT authentication" -m "- Add token generation" -m "- Add validation middleware"
```

### Doğrulama

```bash
# Son commit'i kontrol et
git log -1 --stat

# Commit hash'i al
git rev-parse HEAD
```

## 📤 Adım 6: Kullanıcıdan Push Onayı Al

### Bilgilendirme Mesajı

```
✅ Commit başarıyla oluşturuldu!

📋 Commit Detayları:
Hash: a1b2c3d
Mesaj: feat(auth): implement JWT and OAuth2 authentication
Tarih: 2026-03-15 14:30:00
Dosya: 4 dosya, +159 satır

🌐 Remote Repo Bilgisi:
Remote: origin
Branch: main
URL: https://github.com/kullanici/proje.git

📤 Değişiklikleri remote repoya push etmek istiyor musunuz?

[1] Evet, push yap
[2] Hayır, sadece localde kalsın
[3] Farklı branch'e push yap
```

### Seçenekler

**Seçenek 1: Push Yap**
```bash
git push origin main
```

**Seçenek 2: Localde Kal**
- "Push işlemi iptal edildi" mesajı ver
- Kullanıcıya manuel push yapabileceğini hatırlat

**Seçenek 3: Farklı Branch**
- Mevcut branch'leri listele: `git branch -a`
- Kullanıcıdan hedef branch iste
- Push yap: `git push origin <branch>`

## 🚀 Adım 7: Remote Repoya Push Yap

### Push İşlemi

```bash
# Normal push
git push origin <branch>

# İlk push (upstream ayarla)
git push -u origin <branch>

# Force push (DİKKAT: Dikkatli kullan!)
# Sadece kullanıcı onay verirse
# git push --force-with-lease origin <branch>
```

### Push Sonrası Doğrulama

```bash
# Remote durumunu kontrol et
git log --oneline --graph --decorate --all

# Son commit remote'da mı?
git log origin/main..HEAD
```

### Başarı Mesajı

```
🎉 İşlem tamamlandı!

✅ Commit: a1b2c3d - feat(auth): implement JWT and OAuth2 authentication
🚀 Push: origin/main
📊 Durum: Local ve remote senkronize

Sonraki Adımlar:
• CI/CD pipeline'ı kontrol edin
• Pull request oluşturun (eğer branch kullanıyorsanız)
• Takım arkadaşlarınıza bildirin
```

## ⚠️ Hata Yönetimi

### Senaryo 1: Değişiklik Yok

```
❌ Hata: Commitlenecek değişiklik bulunamadı!

💡 Öneriler:
• Yeni dosya eklediyseniz: git add <dosya>
• Değişiklik yaptıysanız: Dosyaları kaydettiğinizden emin olun
• Git durumu: git status
```

### Senaryo 2: Commit Mesajı Çok Uzun

```
⚠️ Uyarı: Commit mesajı 50 karakterden uzun (72 karakter)

📝 Kısa versiyon:
feat(auth): implement JWT authentication

📄 Detaylı versiyon (body ile):
feat(auth): implement JWT authentication

- Add token generation
- Add validation middleware
- Update user model

Hangi versiyonu kullanmak istersiniz?
[1] Kısa
[2] Detaylı
```

### Senaryo 3: Remote Rejected

```
❌ Hata: Push reddedildi!

Nedenler:
• Remote'da daha yeni commit'ler var
• Branch koruma kuralı var
• Kimlik doğrulama hatası

🛠️ Çözümler:
[1] Pull yap ve merge et: git pull origin main
[2] Force push (DİKKATLI!): git push --force-with-lease
[3] Farklı branch'e push yap
[4] İptal et
```

### Senaryo 4: Kimlik Doğrulama Hatası

```
❌ Hata: Kimlik doğrulama başarısız!

🔑 Çözümler:
• Git credential manager'ı kontrol edin
• SSH key ekleyin: ssh-add ~/.ssh/id_rsa
• Token güncelleyin: git remote set-url origin https://<token>@github.com/...
```

## 🔐 Güvenlik ve İpuçları

### 1. Hassas Bilgileri Kontrol Et

Push öncesinde:
- [ ] `.env` dosyası commit edilmemiş
- [ ] API key'ler staging'de değil
- [ ] Şifreler ve token'lar .gitignore'da
- [ ] `console.log` debug kodları temizlendi

### 2. Commit Mesajı İnceleme

```bash
# Son değişiklikleri göster
git diff --cached

# Staging'deki dosyaları listele
git diff --cached --name-only
```

### 3. İleriye Dönük Git Ayarları

```bash
# Commit template ayarla
git config --global commit.template ~/.gitmessage

# İmza ekle (opsiyonel)
git commit -S -m "feat: add feature"
```

## 📊 İstatistik ve Özet

### İşlem Sonrası Rapor

```
📊 İşlem Özeti:

📝 Commit:
  • Hash: a1b2c3d
  • Mesaj: feat(auth): implement JWT authentication
  • Dosya: 4 dosya
  • Satır: +159, -18

🚀 Push:
  • Remote: origin
  • Branch: main
  • Durum: ✅ Başarılı

⏱️ Zaman:
  • Toplam: 2 dakika
  • Analiz: 30 saniye
  • Commit: 1 dakika
  • Push: 30 saniye
```

## 🎨 Gelişmiş Özellikler

### 1. Çoklu Commit Seçeneği

Eğer çok fazla değişiklik varsa:
- "Birden fazla commit'e bölmek ister misiniz?" sorusu sor
- Dosyaları mantıksal gruplara ayır
- Her grup için ayrı commit mesajı oluştur

### 2. Interactive Staging

```bash
# Dosya bazlı staging
git add -p

# Her hunk için:
# y - stage this hunk
# n - do not stage this hunk
# s - split into smaller hunks
# e - manually edit the hunk
```

### 3. Pre-commit Hook Kontrolü

```bash
# Hook varsa çalıştır
.git/hooks/pre-commit

# Linting, formatting kontrolü
npm run lint
npm run format
```

## 📝 Örnek Senaryolar

### Senaryo 1: Tek Dosya Değişikliği

```
📄 Değişiklik: src/utils/helper.ts (+5, -2)

📝 Önerilen: fix(utils): correct date formatting bug

✅ Onaylıyor musunuz? → Evet
💾 Commit: a1b2c3d
📤 Push? → Evet
🎉 Tamamlandı!
```

### Senaryo 2: Çoklu Dosya - Feature

```
📄 Değişiklikler:
  src/components/Button.tsx (+45)
  src/components/Input.tsx (+32)
  src/styles/button.css (+23)

📝 Önerilen: feat(ui): add reusable Button and Input components

✅ Onaylıyor musunuz? → Evet
💾 Commit: b2c3d4e
📤 Push? → Hayır (sonra push edeceğim)
✅ Commit localde hazır!
```

### Senaryo 3: Bug Fix

```
📄 Değişiklik: src/api/auth.ts (+12, -8)

📝 Önerilen: fix(auth): resolve token expiration handling

💡 Detay:
Token süresi dolunca kullanıcı otomatik logout olmuyordu.
Now token expiration is properly handled.

✅ Onaylıyor musunuz? → Evet
💾 Commit: c3d4e5f
📤 Push? → Evet
🎉 Production'da test edilebilir!
```

## 🚫 Yasaklar ve Dikkat Edilecekler

### ASLA Yapma
- ❌ Force push (`--force`) kullanma (riskli)
- ❌ Ana branch'e (main/master) direkt push (eğer kural varsa)
- ❌ Commit mesajında şifre veya API key yazma
- ❌ Büyük binary dosyaları commit etme
- ❌ `.env` ve `node_modules` commit etme

### DİKKAT Et
- ⚠️ Commit öncesi testleri çalıştır
- ⚠️ Lint hatalarını kontrol et
- ⚠️ Breaking change varsa belirt
- ⚠️ İlgili issue varsa commit mesajına ekle: `Closes #123`

## 📚 Referanslar

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Best Practices](https://git-scm.com/doc)
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html)

---

**Versiyon**: 1.0.0
**Son Güncelleme**: 2026-03-15
**Yazar**: OpenCode AI Agent (Sisyphus)
