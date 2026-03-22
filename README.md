# 📺 StreamVault IPTV Player

Application IPTV multiplateforme avec support MAC/Stalker, Xtream Codes et M3U.

## 🚀 Compiler l'APK avec GitHub Actions

### Étape 1 — Créer le dépôt GitHub

1. Crée un nouveau dépôt sur [github.com/new](https://github.com/new)
   - Nom : `streamvault-iptv`
   - Visibilité : **Privé** (recommandé)
2. Clone le dépôt sur ton PC :
   ```bash
   git clone https://github.com/TON_USERNAME/streamvault-iptv.git
   cd streamvault-iptv
   ```

### Étape 2 — Copier les fichiers du projet

Copie tous les fichiers de ce projet dans le dépôt cloné, puis push :
```bash
git add .
git commit -m "🎬 Initial commit — StreamVault IPTV Player"
git push origin main
```

### Étape 3 — GitHub compile automatiquement !

Dès le push sur `main`, GitHub Actions lance la compilation.

**Pour suivre la progression :**
1. Va sur ton repo → onglet **Actions**
2. Tu verras le workflow `Build StreamVault APK` en cours
3. Attends ~5-8 minutes
4. Clique sur le workflow terminé → **Artifacts** → télécharge l'APK

---

## 🔒 Signer l'APK (optionnel mais recommandé)

Un APK signé est requis pour le Play Store. Pour signer :

### Générer un keystore
```bash
keytool -genkey -v -keystore streamvault.jks \
  -alias streamvault \
  -keyalg RSA -keysize 2048 \
  -validity 10000
```

### Encoder en base64
```bash
# Windows (PowerShell)
[Convert]::ToBase64String([IO.File]::ReadAllBytes("streamvault.jks")) | Out-File keystore.txt

# Linux / macOS
base64 streamvault.jks > keystore.txt
```

### Ajouter les secrets GitHub
Va sur GitHub → ton repo → **Settings** → **Secrets and variables** → **Actions**

Ajoute ces 4 secrets :
| Secret | Valeur |
|--------|--------|
| `KEYSTORE_BASE64` | Contenu du fichier `keystore.txt` |
| `KEY_ALIAS` | `streamvault` |
| `KEY_STORE_PASSWORD` | Ton mot de passe keystore |
| `KEY_PASSWORD` | Ton mot de passe clé |

---

## 📦 Types d'APK générés

Le workflow génère **3 APK** optimisés par architecture :

| Fichier | Architecture | Appareils |
|---------|-------------|-----------|
| `*-arm64-v8a.apk` | 64-bit ARM | Téléphones modernes 2016+, Android TV |
| `*-armeabi-v7a.apk` | 32-bit ARM | Anciens appareils |
| `*-x86_64.apk` | x86 64-bit | Émulateurs, Fire TV |

> **Conseils :** Pour un Android TV ou Fire Stick → télécharge `arm64-v8a`

---

## 🎯 Déclencher un build manuellement

1. Va sur **Actions** → **Build StreamVault APK**
2. Clique **Run workflow**
3. Choisis `debug` ou `release`
4. Lance le workflow

---

## 📋 Créer une Release officielle

Pour créer une release avec les APK en téléchargement :

```bash
git tag v1.0.0
git push origin v1.0.0
```

GitHub Actions créera automatiquement une **Release** avec les APK attachés.

---

## 🛠️ Structure du projet

```
streamvault/
├── .github/
│   └── workflows/
│       └── build-apk.yml        ← Workflow de compilation
├── android/
│   └── app/
│       ├── build.gradle         ← Config Android + signature
│       └── src/main/
│           └── AndroidManifest.xml  ← Permissions + TV support
├── lib/
│   ├── main.dart                ← Point d'entrée Flutter
│   ├── screens/
│   │   ├── connect_screen.dart  ← Écran connexion (MAC/Xtream/M3U)
│   │   └── home_screen.dart     ← Interface principale
│   └── services/
│       ├── stalker_service.dart ← Protocole Stalker + auth MAC
│       └── xtream_service.dart  ← API Xtream Codes + M3U parser
└── pubspec.yaml                 ← Dépendances Flutter
```

---

## 📱 Fonctionnalités implémentées

- [x] Connexion MAC / Stalker Portal (protocole STB)
- [x] Connexion Xtream Codes (API standard)
- [x] Chargement M3U (URL distante)
- [x] Liste des chaînes avec catégories
- [x] Lecteur vidéo (VLC via flutter_vlc_player)
- [x] Guide TV (EPG) latéral
- [x] Grille VOD Films & Séries
- [x] Multi-profils
- [x] Support Android TV (navigation D-pad)
- [x] Compilation CI/CD GitHub Actions

---

## ❓ Support

En cas de problème avec le build, vérifie :
1. Flutter 3.19.0 est bien sélectionné dans le workflow
2. Les secrets GitHub sont correctement configurés (pour la signature)
3. Le `pubspec.yaml` liste bien toutes les dépendances
