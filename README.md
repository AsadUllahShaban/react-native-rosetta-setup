# React Native Rosetta Setup Guide (M1/M2/M3 Macs)
🧠 A step-by-step guide to setting up React Native iOS development on Apple Silicon (M1/M2/M3) using Rosetta. Solve CocoaPods, Metro, and architecture issues reliably.

---

## 📌 Problem

When building React Native apps for iOS on Apple Silicon Macs (M1/M2/M3), developers may encounter errors such as:

- ❌ Metro bundler not connecting to iOS simulator  
- ❌ `pod install` failing with `Bad CPU type in executable`  
- ❌ Architecture mismatches between Ruby, CocoaPods, and native dependencies

These issues occur because:

- iOS Simulator and some build tools still require x86_64 (Intel)
- Your terminal and dependencies (Ruby, Node, etc.) run as ARM (M1 native)

---

## ✅ Solution

Set up a Rosetta-compatible environment for reliable iOS builds with:

- `pod install` (CocoaPods)  
- Metro bundler  
- iOS Simulator  
- React Native CLI

---

## 🧰 Prerequisites

- macOS Terminal (with Rosetta enabled)  
- Homebrew (Rosetta version)  
- Ruby (x86_64)  
- CocoaPods (x86_64)

---

## 🧭 Setup Instructions

### 1. Create a Rosetta Terminal

1. Go to **Applications → Utilities → Terminal**
2. Duplicate `Terminal.app` and rename it to `Terminal (Rosetta)`
3. Right-click → **Get Info** → ✅ Check `Open using Rosetta`
4. Open it and verify:

```bash
uname -m  # should return: x86_64
```

---

### 2. Install Rosetta Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Homebrew for Rosetta will be installed at `/usr/local`.

---

### 3. Install Intel Ruby via Rosetta Homebrew

```bash
/usr/local/bin/brew install ruby
```

Add Ruby to your shell path:

```bash
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Verify:

```bash
ruby -v
which ruby
# ruby 3.x.x @ /usr/local/opt/ruby/bin/ruby
```

---

### 4. Install CocoaPods (Intel)

```bash
sudo gem install cocoapods
```

If `pod` is not found:

```bash
ls -l /usr/local/lib/ruby/gems/*/bin/pod
# Find exact path and add to PATH

echo 'export PATH="/usr/local/lib/ruby/gems/3.x.x/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Verify:

```bash
which pod
pod --version
```

---

### 5. Run Pod Install in Project

```bash
cd ios
arch -x86_64 pod install
```

---

### 6. Start Metro & Build iOS App

```bash
# Terminal 1 (Rosetta):
arch -x86_64 npx react-native start

# Terminal 2 (Rosetta):
npx react-native run-ios
```

---

## 🔧 Troubleshooting

### Problem: `Bad CPU type in executable`

You installed CocoaPods under ARM. Fix:

```bash
sudo gem uninstall cocoapods
# Reinstall in Rosetta terminal
sudo gem install cocoapods
```

---

### Problem: `Can't find pod in PATH`

Add the Ruby gems path manually:

```bash
echo 'export PATH="/usr/local/lib/ruby/gems/3.x.x/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

---

## 🙌 Final Notes

You now have a dual-architecture dev setup:

- Use native terminal for Android
- Use Rosetta terminal for iOS builds

> Inspired by real setup on a MacBook Pro M1 Pro (2021). Share it forward!

---

## 🔗 Resources

- [Homebrew Official Docs](https://brew.sh)
- [Rosetta 2 Apple Docs](https://support.apple.com/en-us/HT211861)
- [CocoaPods GitHub](https://github.com/CocoaPods/CocoaPods)

