# Flutter 環境構築 README（Windows用）

この README は、**Windows で Flutter + Android 開発環境をできるだけコピペで構築する用** です。  
Android アプリを作る前提で、**Flutter SDK + Android Studio + Android SDK + 環境変数** までまとめて書いています。

---

## 1. 先に入れるもの

### 必須
- Flutter SDK
- Android Studio

### 公式リンク
- Flutter インストール案内: [https://docs.flutter.dev/install/windows](https://docs.flutter.dev/install/windows)
- Flutter 手動インストール案内: [https://docs.flutter.dev/install/manual](https://docs.flutter.dev/install/manual)
- Android Studio ダウンロード: [https://developer.android.com/studio](https://developer.android.com/studio)
- Android Studio インストール案内: [https://developer.android.com/studio/install](https://developer.android.com/studio/install)
- Flutter の Android Studio 連携: [https://docs.flutter.dev/tools/android-studio](https://docs.flutter.dev/tools/android-studio)

---

## 2. インストール先

この README では以下を前提にします。

- Flutter SDK: `C:\src\flutter`
- Android SDK: `C:\Users\<ユーザー名>\AppData\Local\Android\Sdk`

> Flutter 公式では、SDK の配置先は **空白や特殊文字を含まないパス** が推奨です。  
> なので `C:\Program Files\...` ではなく `C:\src\flutter` にします。

---

## 3. Flutter SDK を配置する

1. Flutter SDK の zip をダウンロードする  
2. zip を展開する  
3. 展開後に `C:\src\flutter\bin\flutter.bat` が存在することを確認する

### PowerShell で zip を展開する例

```powershell
New-Item -ItemType Directory -Force -Path C:\src | Out-Null
Expand-Archive -Path "$env:USERPROFILE\Downloads\flutter_windows_*.zip" -DestinationPath C:\src -Force
```

### 配置確認

```powershell
Test-Path C:\src\flutter\bin\flutter.bat
```

`True` が出れば OK です。

---

## 4. Flutter の環境変数を設定する

以下を **PowerShell** で実行してください。

```powershell
$flutterBin = "C:\src\flutter\bin"
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")

if ([string]::IsNullOrWhiteSpace($userPath)) {
  $newPath = $flutterBin
} elseif ($userPath -split ";" -contains $flutterBin) {
  $newPath = $userPath
} else {
  $newPath = "$userPath;$flutterBin"
}

[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
```

### 今のターミナルにも即反映する

```powershell
$env:Path = [Environment]::GetEnvironmentVariable("Path", "User") + ";" + [Environment]::GetEnvironmentVariable("Path", "Machine")
flutter --version
dart --version
```

---

## 5. Android Studio をインストールする

以下から Android Studio を入れてください。

- Android Studio ダウンロード: [https://developer.android.com/studio](https://developer.android.com/studio)

インストール後、**一度起動してセットアップウィザードを最後まで進めてください。**

入っていればよいもの:
- Android SDK
- Android SDK Platform-Tools
- Android SDK Command-line Tools
- Android Emulator

> 通常は Android Studio のセットアップでまとめて入ります。

---

## 6. Android SDK の環境変数を設定する

Android SDK の既定パスを使います。

```powershell
$ANDROID_SDK = "$env:LOCALAPPDATA\Android\Sdk"

[Environment]::SetEnvironmentVariable("ANDROID_HOME", $ANDROID_SDK, "User")
[Environment]::SetEnvironmentVariable("ANDROID_SDK_ROOT", $ANDROID_SDK, "User")
```

### PATH に Android 関連を追加する

```powershell
$ANDROID_SDK = "$env:LOCALAPPDATA\Android\Sdk"
$pathsToAdd = @(
  "$ANDROID_SDK\platform-tools",
  "$ANDROID_SDK\cmdline-tools\latest\bin",
  "$ANDROID_SDK\emulator"
)

$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
$current = @()
if (-not [string]::IsNullOrWhiteSpace($userPath)) {
  $current = $userPath -split ";"
}

foreach ($p in $pathsToAdd) {
  if ($current -notcontains $p) {
    $current += $p
  }
}

$newPath = ($current | Where-Object { -not [string]::IsNullOrWhiteSpace($_) }) -join ";"
[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
```

### 今のターミナルにも反映する

```powershell
$env:ANDROID_HOME = [Environment]::GetEnvironmentVariable("ANDROID_HOME", "User")
$env:ANDROID_SDK_ROOT = [Environment]::GetEnvironmentVariable("ANDROID_SDK_ROOT", "User")
$env:Path = [Environment]::GetEnvironmentVariable("Path", "User") + ";" + [Environment]::GetEnvironmentVariable("Path", "Machine")

echo $env:ANDROID_HOME
adb --version
```

---

## 7. JAVA_HOME は基本あと回しでよい

Flutter は Android Studio 側の Java を見つけられることが多いので、まずは **設定しないで進めて OK** です。  
`flutter doctor` で Java 周りのエラーが出たときだけ設定します。

### Android Studio 同梱 JBR を使う例

```powershell
$JAVA_HOME_PATH = "C:\Program Files\Android\Android Studio\jbr"
[Environment]::SetEnvironmentVariable("JAVA_HOME", $JAVA_HOME_PATH, "User")

$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
if (($userPath -split ";") -notcontains "$JAVA_HOME_PATH\bin") {
  [Environment]::SetEnvironmentVariable("Path", "$userPath;$JAVA_HOME_PATH\bin", "User")
}
```

### 反映確認

```powershell
$env:JAVA_HOME = [Environment]::GetEnvironmentVariable("JAVA_HOME", "User")
$env:Path = [Environment]::GetEnvironmentVariable("Path", "User") + ";" + [Environment]::GetEnvironmentVariable("Path", "Machine")

echo $env:JAVA_HOME
java -version
```

---

## 8. ターミナルを開き直す

ここまで終わったら、**PowerShell / Windows Terminal / VS Code / Android Studio を全部閉じて開き直してください。**

そのあと以下を実行します。

```powershell
flutter --version
dart --version
flutter doctor
```

---

## 9. Android ライセンスに同意する

```powershell
flutter doctor --android-licenses
```

途中で `y` を押して進めます。

完了後にもう一度確認します。

```powershell
flutter doctor
```

---

## 10. Android Studio に Flutter プラグインを入れる

Android Studio を開いて以下で入れます。

- `File > Settings > Plugins > Marketplace`
- `flutter` で検索
- Flutter プラグインをインストール
- 必要なら再起動

公式手順:
- [https://docs.flutter.dev/tools/android-studio](https://docs.flutter.dev/tools/android-studio)

> Flutter プラグインを入れると、Dart プラグインも一緒に入ることがあります。

---

## 11. エミュレータを作る

Android Studio を開いて、**Device Manager** から仮想端末を 1 台作ります。

作成後、以下で確認できます。

```powershell
flutter emulators
flutter devices
```

起動する場合:

```powershell
flutter emulators --launch <emulator_id>
```

---

## 12. 動作確認

### プロジェクト作成

```powershell
New-Item -ItemType Directory -Force -Path C:\work | Out-Null
Set-Location C:\work
flutter create my_app
Set-Location .\my_app
flutter run
```

---

## 13. よくある確認コマンド

### Flutter の確認

```powershell
where flutter
flutter --version
dart --version
```

### Android SDK / adb の確認

```powershell
where adb
adb --version
echo $env:ANDROID_HOME
echo $env:ANDROID_SDK_ROOT
```

### Java の確認

```powershell
where java
java -version
echo $env:JAVA_HOME
```

### 詳細確認

```powershell
flutter doctor -v
```

---

## 14. まとめて再読込したいとき

```powershell
$env:ANDROID_HOME = [Environment]::GetEnvironmentVariable("ANDROID_HOME", "User")
$env:ANDROID_SDK_ROOT = [Environment]::GetEnvironmentVariable("ANDROID_SDK_ROOT", "User")
$env:JAVA_HOME = [Environment]::GetEnvironmentVariable("JAVA_HOME", "User")
$env:Path = [Environment]::GetEnvironmentVariable("Path", "User") + ";" + [Environment]::GetEnvironmentVariable("Path", "Machine")
```

---

## 15. 最終チェック

```powershell
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

---

## 16. 最短セットアップ手順だけ見たい人向け

```powershell
# 1) Flutter を C:\src\flutter に展開した前提
$flutterBin = "C:\src\flutter\bin"
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
if ([string]::IsNullOrWhiteSpace($userPath)) {
  $newPath = $flutterBin
} elseif ($userPath -split ";" -contains $flutterBin) {
  $newPath = $userPath
} else {
  $newPath = "$userPath;$flutterBin"
}
[Environment]::SetEnvironmentVariable("Path", $newPath, "User")

# 2) Android SDK 環境変数
$ANDROID_SDK = "$env:LOCALAPPDATA\Android\Sdk"
[Environment]::SetEnvironmentVariable("ANDROID_HOME", $ANDROID_SDK, "User")
[Environment]::SetEnvironmentVariable("ANDROID_SDK_ROOT", $ANDROID_SDK, "User")

# 3) Android SDK の PATH 追加
$pathsToAdd = @(
  "$ANDROID_SDK\platform-tools",
  "$ANDROID_SDK\cmdline-tools\latest\bin",
  "$ANDROID_SDK\emulator"
)
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
$current = @()
if (-not [string]::IsNullOrWhiteSpace($userPath)) {
  $current = $userPath -split ";"
}
foreach ($p in $pathsToAdd) {
  if ($current -notcontains $p) {
    $current += $p
  }
}
[Environment]::SetEnvironmentVariable("Path", (($current | Where-Object { $_ }) -join ";"), "User")

# 4) 今のシェルに反映
$env:ANDROID_HOME = [Environment]::GetEnvironmentVariable("ANDROID_HOME", "User")
$env:ANDROID_SDK_ROOT = [Environment]::GetEnvironmentVariable("ANDROID_SDK_ROOT", "User")
$env:Path = [Environment]::GetEnvironmentVariable("Path", "User") + ";" + [Environment]::GetEnvironmentVariable("Path", "Machine")

# 5) 確認
flutter --version
flutter doctor
```
