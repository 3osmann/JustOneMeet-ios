# 📱 JustOneMeet — Build iOS (IPA)

Un `.ipa` **ne peut pas être compilé sur Windows** (Xcode + signature Apple exigent macOS).
Ce dossier contient le **projet iOS complet et prêt à compiler** — il ne reste que l'étape
build, qui se fait de deux manières :

## Option A — Sur un Mac (recommandé pour commencer)

Prérequis : macOS + **Xcode 16** (App Store), Node.js, et pour l'installation sur iPhone
un **compte Apple** (gratuit suffit pour tester, 7 jours entre deux installs).

```bash
cd F:/JustOneMeet-ios      # (déplacez ce dossier sur le Mac)
npm install
npx cap sync ios
open ios/App/App.xcworkspace
```

Dans Xcode :
1. `ios/App/App.xcworkspace` → sélectionnez le schéma **App** + destination
   « Any iOS Device (arm64) ».
2. Onglet **Signing & Capabilities** → cochez « Automatically manage signing » →
   choisissez votre **Team** (votre compte Apple ou votre développeur).
   → Le `Bundle Identifier` `com.justonemeet.app` doit être libre/uniquement le vôtre.
3. Menu **Product → Archive**.
4. Fenêtre Archives → **Distribute App** :
   - **App Store Connect** (pour TestFlight / App Store) — besoin d'un compte développeur (99 $/an).
   - **Ad Hoc** ou **Development** (pour installer sur votre iPhone) — compte gratuit possible :
     exportez, puis installez le `.ipa` via Mac (Finder/AirDrop) ou un outil comme Apple Configurator.

> ⚠️ Compte gratuit : signature « Personal Team » → réinstallation tous les 7 jours.
> Compte développeur = pas de limite (TestFlight = testeurs illimités).

## Option B — Build cloud automatique (aucun Mac nécessaire)

Le fichier `.github/workflows/ios-build.yml` de ce dossier construit l'IPA sur un
runner macOS de GitHub Actions et le dépose en **artifact téléchargeable**.

1. Poussez ce dossier dans un dépôt GitHub (public ou privé) :
   ```bash
   git init && git add . && git commit -m "ios wrapper" && git branch -M main
   git remote add origin <votre-repo-url>
   git push -u origin main
   ```
2. Dans **Actions** → workflow *iOS build (IPA)* → *Run workflow* → l'archive finit
   dans les artifacts : `.xcarchive` + `.ipa` non signé.
3. **Pour un IPA installable**, ajoutez les secrets signants au dépôt
   (`Settings → Secrets and variables → Actions`) :
   - `APPLE_CERTIFICATE` (p12 en base64) · `APPLE_CERTIFICATE_PASSWORD`
   - `APPLE_PROFILE` (mobileprovision en base64) · `APPLE_TEAM_ID` · `APPLE_SIGNING_ID`

## Changer l'URL du site

Éditez `capacitor.config.json` (champ `server.url`), puis `npx cap sync ios`
avant de compiler. Le site de prod actuel : `https://justonemeet.com`.

## Identifiants

| Clé | Valeur |
|---|---|
| Bundle ID | `com.justonemeet.app` |
| Nom | JustOneMeet |
| URL | `https://justonemeet.com` |
| Icône | monogramme émeraude (générée depuis `/icon.svg`) |