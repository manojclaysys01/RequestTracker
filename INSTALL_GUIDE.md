# 📱 How to Get Your APK — Phone Only Guide

Follow these steps entirely from your mobile phone. Takes about 10 minutes.

---

## PART 1 — Create a GitHub Account (skip if you have one)

1. Open your phone browser and go to → **https://github.com**
2. Tap **Sign up**
3. Enter your email, create a password, choose a username
4. Verify your email
5. You're in ✅

---

## PART 2 — Create a New Repository

1. On GitHub, tap the **+** icon (top right) → **New repository**
2. Fill in:
   - **Repository name:** `RequestTracker`
   - **Visibility:** Public ✅ (required for free Actions)
   - Leave everything else as default
3. Tap **Create repository**

---

## PART 3 — Upload the Project Files

You need to upload the zip file contents to GitHub.

### Method A — Using GitHub's Web Upload (Easiest)

1. On your new repo page, tap **uploading an existing file** (shown on the empty repo page)
2. Extract the `RequestTracker.zip` on your phone first:
   - On Android: Use **Files** app → Long press the zip → Extract
3. Upload files **folder by folder** using GitHub's web uploader:

   Upload in this order:
   - Upload `build.gradle`, `settings.gradle`, `gradlew`, `gradlew.bat`, `.gitignore`
   - Then upload the `gradle/` folder contents
   - Then upload the `app/` folder
   - Then upload `.github/` folder (MOST IMPORTANT — contains the workflow!)

   > 💡 Tip: GitHub web uploader lets you drag & drop multiple files at once

4. After each batch, scroll down and tap **Commit changes**

### Method B — Use GitHub Mobile App (Easier for navigation)

1. Install **GitHub** app from Play Store
2. Log in → Open your repository
3. Use the app to upload files

---

## PART 4 — Trigger the Build

Once all files are uploaded (especially `.github/workflows/build-apk.yml`):

1. On your repo page, tap the **Actions** tab
2. You should see **"Build & Release APK"** workflow listed
3. If it hasn't started automatically, tap on it → tap **Run workflow** → tap **Run workflow** (green button)
4. Watch the build run — it takes about **5–8 minutes** ⏳

---

## PART 5 — Download Your APK

When the build turns ✅ green:

### Option A — From Releases (Recommended)
1. Go to your repo main page
2. On the right side, tap **Releases**
3. Tap the latest release
4. Tap **app-debug.apk** to download it directly to your phone

### Option B — From Actions Artifacts
1. Tap **Actions** tab → tap the latest successful run
2. Scroll down to **Artifacts**
3. Tap **RequestTracker-debug** to download

---

## PART 6 — Install the APK on Your Phone

1. Open your **Files** / **Downloads** app
2. Tap on **app-debug.apk**
3. If prompted: tap **Settings** → enable **Install unknown apps** for your browser/Files app
4. Go back and tap **Install**
5. Tap **Open** — the app is running! 🎉

---

## 🔁 Future Updates

Every time you push new code to the `main` branch, GitHub Actions will automatically build a new APK and create a new Release. You can always find the latest APK under the **Releases** tab.

---

## ❓ Troubleshooting

| Problem | Fix |
|--------|-----|
| Actions tab not visible | Make sure `.github/workflows/build-apk.yml` was uploaded |
| Build fails with Gradle error | Check the Actions log — tap the failed step to see details |
| "Install blocked" on phone | Go to Settings → Apps → Special access → Install unknown apps → allow your browser |
| Workflow not triggering | Go to Actions → select workflow → click "Run workflow" manually |
