# Flutter Environment Setup for macOS

[← Back to README](./README.md) | [Windows](./README.windows.md)

この README は、**macOS で Flutter + Android 開発環境をコピペ中心で構築する用** です。

## 1. 先に入れるもの

### 必須
- Flutter SDK
- Android Studio

### 公式リンク
- Flutter install: https://docs.flutter.dev/install
- Flutter manual install: https://docs.flutter.dev/install/manual
- Set up Android development with Flutter: https://docs.flutter.dev/platform-integration/android/setup
- Android Studio and IntelliJ: https://docs.flutter.dev/tools/android-studio
- Android Studio download: https://developer.android.com/studio
- Android Studio install guide: https://developer.android.com/studio/install

---

## 2. インストール先

この README では以下を前提にします。

- Flutter SDK: `$HOME/develop/flutter`
- Android SDK: `$HOME/Library/Android/sdk`

> Flutter 公式の手動インストールでは、macOS の展開先例として `~/develop/` が案内されています。  
> なのでこの README でも `~/develop/flutter` を使います。

---

## 3. Flutter SDK を配置する

1. Flutter SDK の macOS 用 zip をダウンロード
2. `$HOME/develop` に展開
3. `~/develop/flutter/bin/flutter` が存在することを確認

### 展開例

```bash
mkdir -p "$HOME/develop"
unzip "$HOME/Downloads/flutter_macos_*.zip" -d "$HOME/develop"
```

### 配置確認

```bash
test -x "$HOME/develop/flutter/bin/flutter" && echo OK
```

---

## 4. Flutter を PATH に追加

macOS のデフォルトシェルは通常 zsh なので、`~/.zshrc` に追記します。

```bash
grep -qxF 'export PATH="$HOME/develop/flutter/bin:$PATH"' ~/.zshrc 2>/dev/null || echo 'export PATH="$HOME/develop/flutter/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 反映確認

```bash
flutter --version
dart --version
```

---

## 5. Android Studio をインストールする

以下から Android Studio をインストールしてください。

- Android Studio download: https://developer.android.com/studio
- Android Studio install guide: https://developer.android.com/studio/install

インストール後、**一度起動して初期セットアップを最後まで完了**してください。

入っていればよいもの:
- Android SDK
- Android SDK Build-Tools
- Android SDK Command-line Tools
- Android SDK Platform-Tools
- Android Emulator
- CMake
- NDK (Side by side)

> Flutter の Android セットアップ案内では、SDK Manager で上記ツール群を確認する流れになっています。

---

## 6. Android SDK の環境変数を設定する

`~/.zshrc` に Android SDK 用の環境変数を追記します。
`ANDROID_HOME` が現在の推奨です（`ANDROID_SDK_ROOT` は非推奨）。

```bash
# ANDROID_HOME を優先設定
grep -qxF 'export ANDROID_HOME="$HOME/Library/Android/sdk"' ~/.zshrc 2>/dev/null || echo 'export ANDROID_HOME="$HOME/Library/Android/sdk"' >> ~/.zshrc
grep -qxF 'export ANDROID_SDK_ROOT="$HOME/Library/Android/sdk"' ~/.zshrc 2>/dev/null || echo 'export ANDROID_SDK_ROOT="$HOME/Library/Android/sdk"' >> ~/.zshrc
grep -qxF 'export PATH="$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/emulator:$PATH"' ~/.zshrc 2>/dev/null || echo 'export PATH="$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/emulator:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 反映確認

```bash
echo "$ANDROID_HOME"
adb --version
```

---

## 7. JAVA_HOME は基本あと回しでよい

まずは **設定しないで進めて OK** です。  
`flutter doctor` で Java 関連エラーが出たときだけ設定してください。

Android Studio 同梱 JBR を使う例:

```bash
grep -qxF 'export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"' ~/.zshrc 2>/dev/null || echo 'export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"' >> ~/.zshrc
grep -qxF 'export PATH="$JAVA_HOME/bin:$PATH"' ~/.zshrc 2>/dev/null || echo 'export PATH="$JAVA_HOME/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 確認

```bash
echo "$JAVA_HOME"
java -version
```

---

## 8. ターミナルを開き直す

ここまで終わったら、**Terminal / iTerm / VS Code / Android Studio を全部閉じて開き直してください。**

その後、以下を実行します。

```bash
flutter --version
dart --version
flutter doctor
```

---

## 9. Android ライセンスに同意する

```bash
flutter doctor --android-licenses
```

途中で `y` を押して進めます。

完了後に再確認します。

```bash
flutter doctor
```

---

## 10. Android Studio に Flutter プラグインを入れる

Android Studio を開いて以下からインストールします。

- `Android Studio > Settings...` または `Cmd + ,`
- `Plugins > Marketplace`
- `flutter` で検索
- Flutter プラグインをインストール
- 必要なら再起動

公式手順:
- https://docs.flutter.dev/tools/android-studio

---

## 11. エミュレータを作る

Android Studio を開いて、**Device Manager** から仮想端末を 1 台作ります。

作成後、以下で確認できます。

```bash
flutter emulators
flutter devices
```

起動する場合:

```bash
flutter emulators --launch <emulator_id>
```

---

## 12. 動作確認

```bash
mkdir -p "$HOME/work"
cd "$HOME/work"
flutter create my_app
cd my_app
flutter run
```

---

## 13. よくある確認コマンド

### Flutter の確認

```bash
which flutter
flutter --version
dart --version
```

### Android SDK / adb の確認

```bash
which adb
adb --version
echo "$ANDROID_HOME"
echo "$ANDROID_SDK_ROOT"
```

### Java の確認

```bash
which java
java -version
echo "$JAVA_HOME"
```

### 詳細確認

```bash
flutter doctor -v
```

---

## 14. 最終チェック

```bash
flutter --version
dart --version
adb --version
java -version
flutter doctor
flutter devices
```

以下がだいたい通れば完了です。

- Flutter
- Android toolchain
- Android Studio
- Connected device
