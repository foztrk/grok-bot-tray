<div align="center">

# 🤖 Grok Bot System Tray & Enhancement Suite 📌

**Configurable "Minimize to Tray on Close" Toggle & Localization Blueprint for Grok Desktop (xAI)**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform: Windows](https://img.shields.io/badge/Platform-Windows-0078D6.svg?logo=windows)](https://github.com/foztrk/grok-bot-tray)
[![Supports: Electron](https://img.shields.io/badge/Engine-Electron%20%2F%20.NET-informational.svg)](https://github.com/foztrk/grok-bot-tray)
[![Release: v1.1.1](https://img.shields.io/badge/Release-v1.1.1-brightgreen.svg)](https://github.com/foztrk/grok-bot-tray/releases)
[![Star on GitHub](https://img.shields.io/github/stars/foztrk/grok-bot-tray?style=social)](https://github.com/foztrk/grok-bot-tray)

*A lightweight, native companion (160 KB) and dynamic patcher that adds a configurable system tray toggle and fixes Turkish localization issues for Grok Bot.*

---

### ⭐ Support the Project / Projeyi Destekleyin
> **If this tool saves you time and makes your Grok Bot experience better, please give this repository a ⭐ STAR!**  
> *Bu araç işinize yaradıysa ve Grok Bot deneyiminizi iyileştirdiyse, lütfen depoya bir ⭐ **YILDIZ** vererek destek olun!*

---

[English](#-english) • [Türkçe](#-t%C3%BCrk%C3%A7e) • [Blueprint for xAI / Grok Team](#-blueprint-for-xai--grok-team) • [Turkish Localization Bug Report](#-turkish-localization-bug-report-for-xai)

</div>

---

## 🇬🇧 English

### Why this project?
The official **Grok Desktop** application for Windows (by xAI) currently terminates completely when clicking the **Close (X)** button. Many users expect modern productivity software (like Discord, Telegram, or Slack) to stay active in the background ready to respond instantly.

Version **v1.1.0** introduces a **Configurable Toggle Switch**:
- **ON (Checked):** Clicking **X** minimizes Grok Bot to the system tray.
- **OFF (Unchecked):** Clicking **X** cleanly terminates Grok Bot.
- Persisted automatically in `%APPDATA%\Grok Bot\tray-settings.json`.

### Features
* **Configurable Close Behavior:** Toggle *"Kapatılınca Tepsiye Küçült"* directly from the tray context menu at any time.
* **Instant Background Restore:** Left-click or double-click the tray icon to restore Grok Bot instantly.
* **Smart Single-Instance Interception:** Clicking desktop or taskbar shortcuts restores the existing hidden window.
* **Non-Intrusive Balloon Tips:** Alerts you when the setting is toggled or when Grok Bot is minimized for the first time.
* **Update-Resilient AST Patching:** Dynamically adapts to minified JavaScript variables across official Grok updates.
* **Native C# Companion:** Ultra-fast, ~160 KB footprint communicating over local Named Pipes with zero overhead.
* **100% Reversible:** Revert back to official factory original files with `restore.ps1`.

---

### 🚀 Quick Installation

#### Prerequisites
* Windows 10 or Windows 11 (64-bit)
* [Node.js](https://nodejs.org) (v18+)
* Official [Grok Bot](https://grok.com) installed in default directory (`C:\Program Files\Grok Bot`)

#### One-Click Install
1. Clone or download this repository:
   ```powershell
   git clone https://github.com/foztrk/grok-bot-tray.git
   cd grok-bot-tray
   ```
2. Right-click **`install.ps1`** and select **"Run with PowerShell"** (or run `powershell -ExecutionPolicy Bypass -File install.ps1`).
3. Grok Bot will be patched and restarted automatically.

---

<br>

## 🇹🇷 Türkçe

### Neden Bu Proje?
Resmi **Grok Bot** masaüstü uygulamasında (xAI) varsayılan olarak kapatma butonuna (X) basıldığında uygulama tamamen kapanmaktadır. Birçok kullanıcı arka planda açık kalmasını ve anında yanıt vermeye hazır olmasını beklemektedir.

**v1.1.0 Sürümüyle Gelen Yenilikler:**
- **Açık/Kapalı Ayarı (Toggle):** Sistem tepsisi sağ tık menüsüne eklenen `✔ Kapatılınca Tepsiye Küçült` seçeneğiyle davranış anında değiştirilebilir.
  - **Açıkken (Tikli):** X butonuna basıldığında Grok Bot sağ alt sistem tepsisine küçülür.
  - **Kapalıyken:** X butonuna basıldığında Grok Bot normal şekilde tamamen kapanır.
  - Ayarınız `%APPDATA%\Grok Bot\tray-settings.json` dosyasında kalıcı olarak saklanır.
- **Hafif ve Hızlı:** C# ile geliştirilmiş yardımcı program yalnızca ~160 KB boyutundadır ve Named Pipe ile Electron çekirdeğiyle konuşur.
- **Türkçe Yerelleştirme ve Hata Analizi:** xAI derleme sürecinde tespit ettiğimiz Türkçe arayüz eksiklikleri ve çözüm blueprint'i dahil edilmiştir.

### 🚀 Kurulum
1. Depoyu klonlayın:
   ```powershell
   git clone https://github.com/foztrk/grok-bot-tray.git
   cd grok-bot-tray
   ```
2. **`install.ps1`** dosyasına sağ tıklayıp **"PowerShell ile Çalıştır"** seçeneğini seçin.
3. 5 saniye içinde işlem tamamlanacak ve Grok Bot yeni yetenekleriyle açılacaktır.

---

<br>

## 💡 Blueprint for xAI / Grok Team

> **Dear Elon Musk, Igor Babuschkin, and the xAI Grok Engineering Team,**  
> We reverse-engineered and profiled Grok Bot Desktop on Windows to demonstrate how easily native System Tray support can be added directly into the official repository without breaking Electron lifecycle events.

### Recommended Architectural Implementation

1. **Settings Schema (`electron-store` or user preferences):**
   ```typescript
   interface UserPreferences {
     minimizeToTrayOnClose: boolean; // default: true
   }
   ```

2. **Window Close Event Interception (`main-core.ts`):**
   ```typescript
   mainWindow.on('close', (event) => {
     const minimizeToTray = userPreferences.get('minimizeToTrayOnClose', true);
     if (!isQuitting && minimizeToTray) {
       event.preventDefault();
       mainWindow.hide();
       return false;
     }
   });
   ```

3. **Second-Instance Recovery:**
   ```typescript
   app.on('second-instance', () => {
     if (mainWindow) {
       if (mainWindow.isMinimized()) mainWindow.restore();
       mainWindow.show();
       mainWindow.focus();
     }
   });
   ```

---

<br>

## 🐛 Turkish Localization Bug Report for xAI

During our deep-dive analysis of `app.asar`, we identified a critical build/bundling defect in Grok Bot's localization pipeline:

1. **The Root Cause:**  
   In `dist/renderer/assets/index.eager-app-*.js`, the locale loader function `crt(e)` incorrectly maps the Turkish (`tr`) language code to the emoji-only bundle instead of the main translation catalog:
   ```javascript
   // Current xAI build output (INCORRECT):
   case `tr`: return bi(() => import(`./chunk-emoji-messages.tr-Dd-ZpGQn.js`), [], import.meta.url);
   ```
   Because of this bundling error, the entire UI renderer falls back to hardcoded English strings when Turkish is selected, even though complete Turkish FormatJS strings exist in `dist/electron-main/locales/tr/messages.cjs`!

2. **Legacy Fork Leftovers:**  
   Several Turkish strings in `dist/electron-main/locales/tr/messages.cjs` still contain legacy references to "Cursor" (e.g. `Cursor oturumu`, `Cursor ekibi`, `Cursor bulut aracı`) from upstream forks. These should be renamed to **Grok Bot** / **xAI**.

We would love to submit a direct Pull Request to the official repository if invited! 🚀

---

### 📝 Changelog / Güncelleme Geçmişi
* **v1.1.1 (21.10.2026):**
  - Resmi Grok Bot 21 Ekim güncellemesi (47.5 MB) ile test edildi ve tam uyumluluk doğrulandı.
  - Yeni JavaScript AST değişkenleri (`Pencere="g"`, `İzleyici="JA"`) dinamik ayrıştırıcı tarafından otomatik algılanıp yamalandı.
  - "Kapatılınca Tepsiye Küçült" anahtarı ve sistem tepsisi arka plan hizmeti kesintisiz çalışmaya devam ediyor.
* **v1.1.0 (20.10.2026):**
  - **Yeni Özellik:** "Kapatılınca Tepsiye Küçült" açma/kapatma (toggle) seçeneği eklendi.
  - **Kalıcı Yapılandırma:** `%APPDATA%\Grok Bot\tray-settings.json` üzerinden kullanıcı tercihi kaydedilir.
  - **Named Pipe Protokolü:** `CONFIG:1` ve `CONFIG:0` sinyalleri ile Electron ve C# yardımcısı anında senkronize olur.
  - **Türkçe Yama & Mimari Blueprint:** xAI ekibi için Türkçe arayüz derleme hatası (`chunk-emoji-messages`) analizi ve çözüm rehberi eklendi.
  - **Daha Hafif Derleme:** `GrokBotTray.exe` optimize edilerek 162 KB seviyesinde tutuldu.
* **v1.0.8 (20.10.2026):**
  - Resmi Grok Bot 20 Ekim güncellemesi (47.5 MB) ile test edildi ve tam uyumluluk doğrulandı.
* **v1.0.7 (15.10.2026):**
  - Resmi Grok Bot 15 Ekim güncellemesi (43.8 MB) ile test edildi.
* **v1.0.0 (29.09.2026):**
  - İlk açık kaynak sürüm yayınlandı.

---

### 📄 License
This project is open-source under the [MIT License](LICENSE).  
*Disclaimer: This is an unofficial, community-made enhancement tool. Grok and xAI are trademarks of their respective owners.*
