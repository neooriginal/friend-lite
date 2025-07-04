# GitHub Actions CI/CD Setup for Friend Lite

This sets up **cloud-based** automatic builds that run on GitHub's servers whenever you push code.

## 🤔 How This Works

1. You push code to GitHub
2. GitHub's cloud servers automatically build your APK/IPA
3. You download the built files from GitHub
4. **No local setup required** - everything runs in the cloud!

## 🚀 Workflows Available

### 1. `android-apk-build.yml`
Builds Android APK files on GitHub's Ubuntu servers.

### 2. `ios-ipa-build.yml` 
Builds iOS IPA files on GitHub's macOS servers.

## ⚡ Quick Setup (2 Steps)

### Step 1: Get Expo Token
1. Go to [expo.dev](https://expo.dev) and sign in/create account
2. Go to [Access Tokens](https://expo.dev/accounts/[account]/settings/access-tokens)
3. Create a new token and copy it

### Step 2: Add GitHub Secret
1. In your GitHub repo: **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Name: `EXPO_TOKEN`
4. Value: Paste your token from Step 1
5. Click **Add secret**

## 🎯 That's It!

Now push any code change and GitHub will automatically build your app:

```bash
git add .
git commit -m "trigger build"
git push
```

Go to **Actions** tab to watch the build and download files when done.

## 📱 First Time Setup (Run Once)

The first build might fail because you need to set up certificates. Run these **once** on your computer:

```bash
cd friend-lite/
npx @expo/eas-cli@latest login
npx @expo/eas-cli@latest build:configure
```

Follow the prompts to set up Android keystore and iOS certificates.

## 📦 Downloading Your Apps

After a build completes:
1. Go to your repo's **Actions** tab
2. Click the completed workflow
3. Scroll to **Artifacts** section  
4. Download `friend-lite-android-apk` or `friend-lite-ios-ipa`

## 🔄 Build Triggers

Builds automatically start when you:
- Push to `main` or `develop` branch
- Create a pull request to `main`
- Manually trigger from Actions tab

## ❓ FAQ

**Q: Do I need to install anything locally?**
A: No! Builds run on GitHub's servers.

**Q: Why do I need an Expo account?**
A: To manage app certificates and signing credentials.

**Q: How much does this cost?**
A: Free for public repos. Private repos get free minutes monthly.

**Q: Build failed with "credentials not found"?**
A: Run `npx @expo/eas-cli@latest build:configure` once locally to set up certificates.

---

**🎉 You're all set!** Push code and get built apps automatically. 