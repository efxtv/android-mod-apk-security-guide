# Android Reverse Engineering: A Comprehensive Guide By [EFXTV](https://t.me/efxtv)

> **Disclaimer**: This guide is for educational and authorized security testing purposes only. Reverse engineering applications without proper authorization may violate laws and terms of service. Always obtain proper permission before analyzing any application.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Android Application Structure](#android-application-structure)
4. [Essential Tools Overview](#essential-tools-overview)
5. [Environment Setup](#environment-setup)
6. [APK Acquisition](#apk-acquisition)
7. [Static Analysis](#static-analysis)
   - [Decompilation with JADX](#decompilation-with-jadx)
   - [Disassembly with Apktool](#disassembly-with-apktool)
   - [Smali Analysis](#smali-analysis)
   - [Manifest Analysis](#manifest-analysis)
   - [Resource Analysis](#resource-analysis)
   - [String Analysis](#string-analysis)
   - [Certificate Analysis](#certificate-analysis)
8. [Dynamic Analysis](#dynamic-analysis)
   - [Frida Framework](#frida-framework)
   - [Objection](#objection)
   - [Xposed Framework](#xposed-framework)
   - [Network Traffic Analysis](#network-traffic-analysis)
   - [Logcat Monitoring](#logcat-monitoring)
9. [Advanced Techniques](#advanced-techniques)
   - [Bypassing Root Detection](#bypassing-root-detection)
   - [Bypassing SSL Pinning](#bypassing-ssl-pinning)
   - [Anti-Debugging Bypass](#anti-debugging-bypass)
   - [Emulator Detection Bypass](#emulator-detection-bypass)
   - [Tampering and Repackaging](#tampering-and-repackaging)
   - [Memory Analysis](#memory-analysis)
   - [Native Library Analysis](#native-library-analysis)
10. [Automation and Scripting](#automation-and-scripting)
11. [Common Vulnerability Patterns](#common-vulnerability-patterns)
12. [Reporting Findings](#reporting-findings)
13. [Legal and Ethical Considerations](#legal-and-ethical-considerations)
14. [Resources and Further Reading](#resources-and-further-reading)
15. [Cheat Sheets](#cheat-sheets)

---

## Introduction

### What is Android Reverse Engineering?

Android reverse engineering is the process of analyzing compiled Android applications (APK files) to understand their internal workings, extract information, identify vulnerabilities, or modify their behavior. This practice is fundamental to:

- **Security Research**: Discovering vulnerabilities and security flaws
- **Malware Analysis**: Understanding malicious application behavior
- **Penetration Testing**: Assessing application security during authorized engagements
- **Competitive Analysis**: Understanding how competing applications work
- **Digital Forensics**: Investigating criminal activities involving mobile applications
- **Bug Bounty Hunting**: Finding security bugs for rewards
- **Learning and Education**: Understanding Android internals and software architecture

### The Android Application Lifecycle

Understanding how an Android app goes from source code to installed application is crucial:

```
Source Code (.java/.kt)
        ↓
    Compilation
        ↓
    Bytecode (.class files)
        ↓
    DEX Conversion (dx/d8)
        ↓
    Dalvik Executable (.dex)
        ↓
    Packaging with resources
        ↓
    Android Package (.apk)
        ↓
    Signing (debug/release)
        ↓
    Installation on device
```

### Types of Analysis

| Analysis Type | Description | Tools Used |
|--------------|-------------|------------|
| **Static Analysis** | Examining the app without executing it | JADX, Apktool, Ghidra |
| **Dynamic Analysis** | Analyzing the app while it's running | Frida, Objection, Logcat |
| **Network Analysis** | Intercepting and analyzing network traffic | Burp Suite, mitmproxy |
| **Memory Analysis** | Examining the app's memory at runtime | Frida, GDB |
| **Hybrid Analysis** | Combining static and dynamic techniques | Multiple tools |

---

## Prerequisites

### Required Knowledge

Before diving into Android reverse engineering, you should have:

1. **Programming Fundamentals**
   - Java/Kotlin (Android's primary languages)
   - Basic understanding of object-oriented programming
   - Familiarity with design patterns
   - Basic Python/JavaScript scripting

2. **Android Development Basics**
   - Activity lifecycle
   - Intents and Services
   - Content Providers and Broadcast Receivers
   - AndroidManifest.xml structure
   - Android UI components

3. **Linux/Command Line Proficiency**
   - Basic shell commands
   - File system navigation
   - Package management
   - Text processing (grep, sed, awk)

4. **Networking Fundamentals**
   - HTTP/HTTPS protocols
   - TCP/IP basics
   - Certificate and TLS concepts
   - Proxy configuration

5. **Security Concepts**
   - Common vulnerability types (OWASP Mobile Top 10)
   - Encryption basics
   - Authentication mechanisms
   - API security

### Hardware Requirements

```
Minimum Requirements:
├── CPU: Multi-core processor (Intel i5/AMD Ryzen 5 or better)
├── RAM: 8 GB (16 GB recommended)
├── Storage: 50 GB free space (SSD strongly recommended)
├── OS: Linux (Ubuntu/Debian preferred), macOS, or Windows with WSL2
└── Network: Stable internet connection

Recommended Setup:
├── CPU: Intel i7/AMD Ryzen 7 or better
├── RAM: 32 GB
├── Storage: 256 GB SSD
├── GPU: NVIDIA GPU (for GPU-accelerated analysis)
└── Dedicated testing device or emulator
```

---

## Android Application Structure

### APK File Structure

An APK (Android Package Kit) is essentially a ZIP archive with a specific structure:

```
application.apk
├── AndroidManifest.xml          # App configuration and permissions
├── classes.dex                  # Dalvik executable (main code)
├── classes2.dex                 # Multi-dex secondary file
├── resources.arsc               # Compiled binary resource table
├── res/                         # Resource directory
│   ├── layout/                  # UI layouts
│   ├── drawable/                # Images and shapes
│   ├── values/                  # Strings, colors, styles
│   ├── xml/                     # XML configurations
│   ├── menu/                    # Menu definitions
│   └── raw/                     # Raw resource files
├── lib/                         # Native libraries
│   ├── armeabi-v7a/             # ARM 32-bit
│   ├── arm64-v8a/               # ARM 64-bit
│   ├── x86/                     # Intel 32-bit
│   └── x86_64/                  # Intel 64-bit
├── assets/                      # Raw asset files
│   ├── fonts/
│   ├── images/
│   └── databases/
├── META-INF/                    # Signature and certificate
│   ├── MANIFEST.MF
│   ├── CERT.SF
│   └── CERT.RSA
├── kotlin/                      # Kotlin metadata
└── unknown/                     # Files not in standard locations
```

### DEX File Format

The Dalvik Executable (DEX) format contains compiled bytecode:

```
DEX File Structure:
├── Header
│   ├── Magic number ("dex\n035\0")
│   ├── Checksum
│   ├── SHA-1 signature
│   ├── File size
│   ├── Header size
│   ├── Endian tag
│   └── Various offsets and sizes
├── String IDs
├── Type IDs
├── Proto IDs (method prototypes)
├── Field IDs
├── Method IDs
├── Class Definitions
│   ├── Class data
│   ├── Static fields
│   ├── Instance fields
│   ├── Direct methods
│   └── Virtual methods
├── Call Site IDs
├── Method Handle Items
└── Data Section
    ├── Code items
    ├── String data
    ├── Type lists
    ├── Annotations
    └── Encoded arrays
```

### AndroidManifest.xml Key Elements

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app"
    android:versionCode="1"
    android:versionName="1.0">

    <!-- Permissions the app requests -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_CONTACTS" />

    <!-- Hardware features required -->
    <uses-feature android:name="android.hardware.camera" />

    <!-- Application component declarations -->
    <application
        android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher"
        android:debuggable="true"
        android:allowBackup="true"
        android:networkSecurityConfig="@xml/network_security_config">

        <!-- Main activity (entry point) -->
        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- Other components -->
        <service android:name=".MyService" android:exported="false" />
        <receiver android:name=".MyReceiver" android:exported="true" />
        <provider android:name=".MyProvider"
            android:authorities="com.example.provider"
            android:exported="false" />
    </application>
</manifest>
```

---

## Essential Tools Overview

### Tool Categories and Selection Guide

| Category | Tool | Purpose | Difficulty | Platform |
|----------|------|---------|------------|----------|
| **Decompiler** | JADX | APK → Java source | Easy | All |
| **Decompiler** | Fernflower | JAR → Java source | Easy | All |
| **Decompiler** | Procyon | JAR → Java source | Easy | All |
| **Disassembler** | Apktool | APK → Smali + resources | Easy | All |
| **Disassembler** | Baksmali | DEX → Smali | Easy | All |
| **IDE/Editor** | Android Studio | Development & analysis | Medium | All |
| **Dynamic Instrumentation** | Frida | Runtime hooking & patching | Medium | All |
| **Dynamic Instrumentation** | Objection | Frida-based exploration | Easy | All |
| **Dynamic Instrumentation** | Xposed | System-level hooking | Hard | Android |
| **Disassembler** | Ghidra | Native library analysis | Hard | All |
| **Disassembler** | IDA Pro | Native library analysis | Hard | All |
| **Debugger** | GDB/LLDB | Native debugging | Hard | Linux/macOS |
| **Network Proxy** | Burp Suite | HTTP/HTTPS interception | Medium | All |
| **Network Proxy** | mitmproxy | HTTP/HTTPS interception | Medium | All |
| **Network Proxy** | Charles Proxy | HTTP/HTTPS interception | Easy | All |
| **Network Scanner** | nmap | Network discovery | Medium | All |
| **Packet Capture** | Wireshark | Packet-level analysis | Medium | All |
| **File Analysis** | Dex2jar | DEX → JAR conversion | Easy | All |
| **File Analysis** | Androguard | Python-based analysis | Medium | All |
| **File Analysis** | Androbugs | Vulnerability scanning | Easy | All |
| **Emulator** | Android Emulator | Testing environment | Easy | All |
| **Emulator** | Genymotion | Testing environment | Easy | All |
| **Automation** | ADB | Device communication | Easy | All |
| **Automation** | Drozer | Security testing framework | Medium | All |

### Recommended Tool Stack for Beginners

```
Essential (Must Have):
├── JADX-GUI          → Primary decompiler
├── Apktool           → Resource extraction and Smali
├── ADB               → Device interaction
├── Frida             → Dynamic analysis
├── Burp Suite CE     → Network interception
└── VS Code / Sublime → Text editing

Intermediate:
├── Objection         → Automated Frida scripts
├── mitmproxy         → Advanced proxy
├── Androguard        → Python analysis framework
└── dex2jar           → DEX to JAR conversion

Advanced:
├── Ghidra            → Native code analysis
├── IDA Pro           → Professional disassembly
├── GDB/LLDB          → Native debugging
└── Radare2/Cutter    → Open-source RE framework
```

---

## Environment Setup

### Step 1: Install Java Development Kit (JDK)

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-17-jdk openjdk-17-jre

# Verify installation
java -version
javac -version

# Set JAVA_HOME
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
source ~/.bashrc
```

```bash
# macOS (using Homebrew)
brew install openjdk@17
echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 17)' >> ~/.zshrc
source ~/.zshrc
```

### Step 2: Install Android SDK and ADB

```bash
# Ubuntu/Debian
sudo apt install android-sdk
# OR download Android Studio and use its SDK manager

# Install ADB standalone
sudo apt install android-tools-adb android-tools-fastboot

# Verify ADB
adb version

# macOS
brew install android-platform-tools

# Add to PATH (if installed manually)
echo 'export ANDROID_HOME=$HOME/Android/Sdk' >> ~/.bashrc
echo 'export PATH=$PATH:$ANDROID_HOME/platform-tools' >> ~/.bashrc
echo 'export PATH=$PATH:$ANDROID_HOME/tools' >> ~/.bashrc
source ~/.bashrc
```

### Step 3: Install JADX

```bash
# Download latest release
wget https://github.com/skylot/jadx/releases/download/v1.5.0/jadx-1.5.0.zip
unzip jadx-1.5.0.zip -d jadx
chmod +x jadx/bin/jadx jadx/bin/jadx-gui

# Add to PATH
echo 'export PATH=$PATH:$HOME/jadx/bin' >> ~/.bashrc
source ~/.bashrc

# Verify
jadx --version

# GUI version
jadx-gui
```

```bash
# macOS
brew install jadx
```

```bash
# Building from source
git clone https://github.com/skylot/jadx.git
cd jadx
./gradlew dist
# Binary will be in build/jadx/
```

### Step 4: Install Apktool

```bash
# Download Apktool wrapper script
sudo apt install apktool
# OR manual installation:

# Download latest apktool.jar
wget https://github.com/iBotPeaches/Apktool/releases/download/v2.9.3/apktool_2.9.3.jar
sudo mv apktool_2.9.3.jar /usr/local/bin/apktool.jar

# Create wrapper script
cat << 'EOF' | sudo tee /usr/local/bin/apktool
#!/bin/bash
java -jar /usr/local/bin/apktool.jar "$@"
EOF
sudo chmod +x /usr/local/bin/apktool

# Verify
apktool --version
```

### Step 5: Install Frida

```bash
# Install Frida tools (on your computer)
pip3 install frida-tools
pip3 install frida

# Verify installation
frida --version

# Download Frida server for Android (match your device architecture)
# Check device architecture
adb shell getprop ro.product.cpu.abi
# Common outputs: arm64-v8a, armeabi-v7a, x86_64, x86

# Download matching Frida server
FRIDA_VERSION=$(frida --version)
wget https://github.com/frida/frida/releases/download/${FRIDA_VERSION}/frida-server-${FRIDA_VERSION}-android-arm64.xz
xz -d frida-server-${FRIDA_VERSION}-android-arm64.xz

# Push to device
adb push frida-server-${FRIDA_VERSION}-android-arm64 /data/local/tmp/frida-server
adb shell "chmod 755 /data/local/tmp/frida-server"

# Start Frida server on device
adb shell "su -c /data/local/tmp/frida-server &"

# List running processes (verify connection)
frida-ps -U
```

### Step 6: Install Objection

```bash
pip3 install objection

# Verify
objection version

# Usage
objection -g <package_name> explore
```

### Step 7: Install Burp Suite Community Edition

```bash
# Download from https://portswigger.net/burp/communitydownload

# Ubuntu/Debian
chmod +x burpsuite_community_linux_v2024_x.sh
./burpsuite_community_linux_v2024_x.sh

# Or use snap
sudo snap install burpsuite

# Configure proxy listener:
# 1. Open Burp Suite
# 2. Go to Proxy → Options
# 3. Add listener on 127.0.0.1:8080
# 4. Configure Android device proxy to point to your computer's IP:8080
```

### Step 8: Install Ghidra (for Native Code Analysis)

```bash
# Install Ghidra dependencies
sudo apt install openjdk-17-jdk

# Download Ghidra from https://ghidra-sre.org/
unzip ghidra_11.0_PUBLIC_20240130.zip
cd ghidra_11.0
./ghidraRun
```

### Step 9: Install Additional Python Tools

```bash
# Androguard - Android analysis library
pip3 install androguard

# Drozer - Security testing framework
pip3 install drozer

# APKInspector
git clone https://github.com/honeynet/APKInspector.git
cd APKInspector
pip3 install -r requirements.txt
```

### Step 10: Set Up Testing Device/Emulator

```bash
# Option A: Android Emulator (via Android Studio)
# 1. Install Android Studio
# 2. Open AVD Manager
# 3. Create Virtual Device
# 4. Select system image (API 28-33 recommended)
# 5. Configure and launch

# Option B: Genymotion
# 1. Download Genymotion Desktop
# 2. Create account
# 3. Install and create virtual device

# Option C: Physical Device
# 1. Enable Developer Options (tap Build Number 7 times)
# 2. Enable USB Debugging
# 3. Connect via USB
# 4. Verify: adb devices

# Check connected devices
adb devices -l

# Root access (physical device with Magisk):
# 1. Unlock bootloader
# 2. Flash custom recovery (TWRP)
# 3. Install Magisk
```

---

## APK Acquisition

### Method 1: From Google Play Store

```bash
# Using APKPure or similar services
# 1. Search for the app on apkpure.com or apkmirror.com
# 2. Download the APK/XAPK file
# 3. If XAPK, use SAI (Split APKs Installer) or rename to .zip

# Using gplaydl (command line)
pip3 install gplaydl
gplaydl -a <app_package_name>
```

### Method 2: From Device with ADB

```bash
# List installed packages
adb shell pm list packages | grep <keyword>

# Get APK path on device
adb shell pm path com.example.app
# Output: package:/data/app/com.example.app-1/base.apk

# Pull APK from device
adb pull /data/app/com.example.app-1/base.apk ./app.apk

# Pull all APKs for split APK apps
adb shell pm path com.example.app | while read line; do
    path=$(echo $line | sed 's/package://')
    filename=$(basename $path)
    adb pull "$path" "./$filename"
done
```

### Method 3: Using ADB Backup (Legacy)

```bash
# Create backup (works on older Android versions)
adb backup -apk -shared -all -f backup.ab

# Convert .ab to .tar
java -jar abe.jar unpack backup.ab backup.tar

# Extract APKs from backup
tar -xf backup.tar
```

### Method 4: Extracting from Device File System (Root Required)

```bash
# With root access
adb shell su -c "cp /data/app/com.example.app-1/base.apk /sdcard/"
adb pull /sdcard/base.apk ./app.apk
```

### Method 5: Using APK Extractor Apps

Several apps on the Play Store can extract installed APKs:
- APK Extractor
- ML Manager
- App Backup & Restore

---

## Static Analysis

### Decompilation with JADX

JADX is the most popular and user-friendly decompiler for Android applications. It converts DEX files back into readable Java source code.

#### Command Line Usage

```bash
# Basic decompilation
jadx -d output_dir application.apk

# With deobfuscation
jadx -d output_dir --deobf application.apk

# With specific options
jadx -d output_dir \
    --deobf \
    --deobf-min \
    --deobf-use-sourcename \
    --show-bad-code \
    --no-res \
    application.apk

# Extract resources only (faster)
jadx -d output_dir --no-src application.apk

# Decompile specific DEX file
jadx -d output_dir classes.dex

# Threads for faster processing
jadx -d output_dir -j 8 application.apk  # 8 threads
```

#### JADX GUI Usage

```bash
# Launch GUI
jadx-gui

# Open APK directly
jadx-gui application.apk
```

**JADX GUI Features:**

1. **Navigation Panel (Left)**
   - Package tree view
   - Search functionality (Ctrl+Shift+F)
   - Class hierarchy

2. **Code View (Center)**
   - Decompiled Java source
   - Syntax highlighting
   - Cross-reference navigation (click on class/method names)

3. **Features Menu**
   - `Navigation → Search` - Find strings, classes, methods
   - `Tools → Strings` - View all strings
   - `Tools → Methods` - Search methods
   - `Tools → Smali` - View Smali code alongside Java

#### Analyzing Decompiled Code

**What to Look For:**

```java
// 1. API Keys and Secrets
public class Config {
    public static final String API_KEY = "sk-1234567890abcdef";  // FOUND!
    public static final String SECRET = "my-secret-key";
    private static final String DB_PASSWORD = "admin123";
}

// 2. Hardcoded Credentials
public class AuthService {
    public boolean login(String username, String password) {
        // Hardcoded admin credentials
        if (username.equals("admin") && password.equals("P@ssw0rd")) {
            return true;
        }
    }
}

// 3. Insecure Network Communication
public class NetworkManager {
    public void makeRequest() {
        // HTTP instead of HTTPS
        URL url = new URL("http://api.example.com/data");
        // OR trust all certificates
        TrustManager[] trustAllCerts = new TrustManager[]{
            new X509TrustManager() {
                public void checkClientTrusted(...) {}
                public void checkServerTrusted(...) {}
                public X509Certificate[] getAcceptedIssuers() { return null; }
            }
        };
    }
}

// 4. Weak Cryptography
public class CryptoUtil {
    public String encrypt(String data) {
        // DES is weak
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");
        // OR weak key
        SecretKey key = new SecretKeySpec("12345678".getBytes(), "DES");
    }
}

// 5. SQL Injection Vulnerabilities
public class DatabaseHelper {
    public Cursor getUser(String username) {
        // String concatenation in SQL
        String query = "SELECT * FROM users WHERE name = '" + username + "'";
        return db.rawQuery(query, null);
    }
}

// 6. Logging Sensitive Data
public class PaymentActivity {
    public void processPayment(Card card) {
        Log.d("Payment", "Card number: " + card.getNumber());
        Log.d("Payment", "CVV: " + card.getCvv());
    }
}

// 7. WebView Vulnerabilities
public class WebActivity extends Activity {
    void setupWebView() {
        webView.setJavaScriptEnabled(true);
        webView.addJavascriptInterface(new JSBridge(), "Android");
        webView.setAllowFileAccess(true);
        // Dangerous: allows file:// access
    }
}

// 8. Insecure Data Storage
public class UserManager {
    public void saveUser(User user) {
        SharedPreferences prefs = getSharedPreferences("user", MODE_WORLD_READABLE);
        prefs.putString("password", user.getPassword());
        prefs.putString("token", user.getAuthToken());
    }
}

// 9. Content Provider Exports
// Check AndroidManifest for exported providers with no permissions

// 10. Deep Link Handlers
public class DeepLinkActivity extends Activity {
    void onCreate(Bundle b) {
        Uri data = getIntent().getData();
        // No validation on URI
        webView.loadUrl(data.toString());
    }
}
```

#### JADX Search Techniques

```
Finding Specific Patterns:

1. String Search:
   - Ctrl+Shift+F → Enter search term
   - Use regex: "api[_-]?key" or "secret[_-]?key"

2. Class Search:
   - Ctrl+N → Enter class name
   - Supports partial matches

3. Method Search:
   - Ctrl+M → Enter method name

4. Text Search in Current File:
   - Ctrl+F → Enter text

5. Cross References:
   - Right-click on any identifier → "Find Usage"
   - Shows all places where the identifier is used

6. Go to Definition:
   - Ctrl+Click on any class/method/field name
```

---

### Disassembly with Apktool

Apktool decompiles APK files into Smali code and extracts resources in their original format.

#### Basic Commands

```bash
# Decompile APK
apktool d application.apk -o output_dir

# Decompile with framework resources
apktool d -f application.apk -o output_dir

# Force overwrite existing output
apktool d -f application.apk -o output_dir

# Decode resources only
apktool d -r application.apk -o output_dir

# No disassembly (resources only, faster)
apktool d -s application.apk -o output_dir

# Build/Recompile modified APK
apktool b output_dir -o modified.apk
```

#### Apktool Output Structure

```
output_dir/
├── AndroidManifest.xml          # Decoded manifest (readable XML)
├── apktool.yml                  # Apktool metadata
├── original/                    # Original META-INF and manifest
│   ├── AndroidManifest.xml
│   └── META-INF/
├── res/                         # Decoded resources
│   ├── layout/                  # XML layouts (readable)
│   ├── values/                  # Strings, styles, colors
│   ├── drawable/                # Image files
│   ├── xml/                     # Configuration files
│   └── ...
├── smali/                       # Disassembled Smali code
│   └── com/
│       └── example/
│           └── app/
│               ├── MainActivity.smali
│               ├── models/
│               └── utils/
├── smali_classes2/              # Multi-dex secondary classes
├── assets/                      # Extracted assets
├── lib/                         # Native libraries (.so files)
└── unknown/                     # Files not categorized
```

#### Key Apktool Use Cases

```bash
# 1. Readable AndroidManifest.xml
# After decompilation, open output_dir/AndroidManifest.xml
# Look for:
#   - Exported components
#   - Custom permissions
#   - Debuggable flag
#   - Backup allowed
#   - Network security config
#   - Deep link schemes

# 2. Resource analysis
# Browse res/values/strings.xml for:
#   - API endpoints
#   - Firebase config
#   - Hardcoded values
#   - Debug/test strings

# 3. Extract assets
# Browse assets/ for:
#   - SQLite databases
#   - Configuration files
#   - WebView HTML/JS
#   - Embedded certificates

# 4. Smali code analysis
# Browse smali/ for method implementations
```

---

### Smali Analysis

Smali is the human-readable assembly language for the Dalvik VM. Understanding Smali is crucial for deep analysis and patching.

#### Smali Register Types

```
v0, v1, v2, ...    - Local registers
p0, p1, p2, ...    - Parameter registers
                     p0 = 'this' in non-static methods
                     p0 = first parameter in static methods
```

#### Smali Data Types

| Smali Type | Java Type | Description |
|-----------|-----------|-------------|
| `V` | `void` | No return value |
| `Z` | `boolean` | True/false |
| `B` | `byte` | 8-bit signed |
| `S` | `short` | 16-bit signed |
| `C` | `char` | Unicode character |
| `I` | `int` | 32-bit signed |
| `J` | `long` | 64-bit signed |
| `F` | `float` | 32-bit floating point |
| `D` | `double` | 64-bit floating point |
| `Lpackage/ClassName;` | `ClassName` | Object reference |
| `[I` | `int[]` | Array of ints |
| `[[Lpackage/ClassName;` | `ClassName[][]` | 2D object array |

#### Smali Method Signatures

```
# Format: Lpackage/Class;->methodName(ParamTypes)ReturnType

# Java: public int calculate(int a, int b)
# Smali: Lcom/example/Calculator;->calculate(II)I

# Java: private String format(String template, Object... args)  
# Smali: Lcom/example/Formatter;->format(Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/String;

# Java: public static void main(String[] args)
# Smali: Lcom/example/Main;->main([Ljava/lang/String;)V
```

#### Common Smali Instructions

```smali
# ===== MOVEMENT =====
move v0, v1              # v0 = v1 (reference)
move/from16 v0, v1       # v0 = v1 (16-bit register)
move-wide v0, v2         # v0 = v2 (64-bit value)
move-object v0, v1       # v0 = v1 (object)

# ===== CONSTANTS =====
const/4 v0, 0x1          # v0 = 1 (4-bit literal)
const/16 v0, 0x100       # v0 = 256 (16-bit literal)
const v0, 0x12345678     # v0 = 305419896 (32-bit literal)
const-string v0, "Hello" # v0 = "Hello"
const-class v0, Lcom/example/MyClass;  # v0 = MyClass.class

# ===== ARITHMETIC =====
add-int v0, v1, v2       # v0 = v1 + v2
sub-int v0, v1, v2       # v0 = v1 - v2
mul-int v0, v1, v2       # v0 = v1 * v2
div-int v0, v1, v2       # v0 = v1 / v2
rem-int v0, v1, v2       # v0 = v1 % v2

# ===== COMPARISON =====
cmpl-float v0, v1, v2    # v0 = (v1 < v2) ? -1 : ((v1 == v2) ? 0 : 1)
cmpg-float v0, v1, v2    # v0 = (v1 > v2) ? 1 : ((v1 == v2) ? 0 : -1)
if-eq v0, v1, :label     # if (v0 == v1) goto label
if-ne v0, v1, :label     # if (v0 != v1) goto label
if-lt v0, v1, :label     # if (v0 < v1) goto label
if-ge v0, v1, :label     # if (v0 >= v1) goto label
if-gt v0, v1, :label     # if (v0 > v1) goto label
if-le v0, v1, :label     # if (v0 <= v1) goto label
if-eqz v0, :label        # if (v0 == 0) goto label
if-nez v0, :label        # if (v0 != 0) goto label

# ===== OBJECTS =====
new-instance v0, Lcom/example/MyClass;   # v0 = new MyClass()
invoke-direct {v0}, Lcom/example/MyClass;-><init>()V  # constructor
invoke-virtual {v0, v1}, Lcom/example/MyClass;->doSomething(I)V  # method call
invoke-static {v0}, Lcom/example/Util;->convert(Ljava/lang/String;)V  # static
invoke-interface {v0}, Ljava/util/List;->size()I  # interface method

# ===== ARRAYS =====
new-array v0, v1, [I     # v0 = new int[v1]
array-length v1, v0      # v1 = v0.length
aget v1, v0, v2          # v1 = v0[v2]
aput v1, v0, v2          # v0[v2] = v1

# ===== FIELDS =====
iget v0, v1, Lcom/example/MyClass;->count:I  # v0 = v1.count
iput v0, v1, Lcom/example/MyClass;->count:I  # v1.count = v0
sget v0, Lcom/example/MyClass;->TOTAL:I  # v0 = MyClass.TOTAL
sput v0, Lcom/example/MyClass;->TOTAL:I  # MyClass.TOTAL = v0

# ===== RETURN =====
return-void               # return (void)
return v0                 # return v0 (primitive)
return-object v0          # return v0 (object)

# ===== EXCEPTIONS =====
:try_start_0
    # ... code ...
:try_end_0
.catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0
:catch_0
    move-exception v0     # v0 = caught exception
```

#### Example: Complete Smali Method

```smali
# Java equivalent:
# public boolean isValidPassword(String password) {
#     if (password == null || password.length() < 8) {
#         return false;
#     }
#     return password.matches(".*[A-Z].*") && 
#            password.matches(".*[0-9].*") &&
#            password.matches(".*[!@#$%].*");
# }

.method public isValidPassword(Ljava/lang/String;)Z
    .registers 5
    .param p1, "password"

    # Check if null
    if-eqz p1, :cond_null
    invoke-virtual {p1}, Ljava/lang/String;->length()I
    move-result v0
    const/16 v1, 0x8
    if-lt v0, v1, :cond_invalid

    # Check uppercase
    const-string v0, ".*[A-Z].*"
    invoke-virtual {p1, v0}, Ljava/lang/String;->matches(Ljava/lang/String;)Z
    move-result v0
    if-eqz v0, :cond_invalid

    # Check digit
    const-string v0, ".*[0-9].*"
    invoke-virtual {p1, v0}, Ljava/lang/String;->matches(Ljava/lang/String;)Z
    move-result v0
    if-eqz v0, :cond_invalid

    # Check special char
    const-string v0, ".*[!@#$%].*"
    invoke-virtual {p1, v0}, Ljava/lang/String;->matches(Ljava/lang/String;)Z
    move-result v0
    if-eqz v0, :cond_invalid

    # Return true
    :cond_valid
    const/4 v0, 0x1
    return v0

    :cond_null
    :cond_invalid
    const/4 v0, 0x0
    return v0
.end method
```

#### Common Smali Patches

```smali
# ===== PATCH 1: Always return true =====
# Original:
.method public isPremium()Z
    .registers 2
    # ... complex premium check logic ...
    return v0
.end method

# Patched:
.method public isPremium()Z
    .registers 2
    const/4 v0, 0x1
    return v0
.end method


# ===== PATCH 2: Always return false (disable check) =====
# Original:
.method public isRooted()Z
    .registers 5
    # ... root detection logic ...
    return v0
.end method

# Patched:
.method public isRooted()Z
    .registers 2
    const/4 v0, 0x0
    return v0
.end method


# ===== PATCH 3: Remove method body =====
.method public checkLicense()V
    .registers 1
    return-void
.end method


# ===== PATCH 4: Change string constant =====
# Find:
const-string v0, "http://api.example.com"

# Replace with:
const-string v0, "http://your-server.com"


# ===== PATCH 5: Skip a conditional check =====
# Original:
if-nez v0, :cond_skip
# ... protected code ...
:cond_skip

# Patched (remove the if-nez, making execution always continue):
# Delete the if-nez line
# Or change to:
if-eqz v0, :cond_skip  # inverted condition (or just nop it)
```

---

### Manifest Analysis

The AndroidManifest.xml reveals critical security information about the application.

#### Key Security Attributes to Analyze

```xml
<!-- 1. Debuggable Flag -->
<!-- If true, the app can be debugged - major security issue in production -->
<application android:debuggable="true">  <!-- VULNERABILITY -->

<!-- 2. Backup Allowed -->
<!-- If true, app data can be extracted via adb backup -->
<application android:allowBackup="true">  <!-- VULNERABILITY -->

<!-- 3. Network Security Config -->
<!-- Check for custom network security configuration -->
<application android:networkSecurityConfig="@xml/network_security_config">

<!-- Example insecure network_security_config.xml: -->
<!-- Allows cleartext traffic, trusts user certificates -->
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
            <certificates src="user" />  <!-- VULNERABILITY: trusts user certs -->
        </trust-anchors>
    </base-config>
</network-security-config>

<!-- 4. Exported Components -->
<!-- Exported components can be accessed by other apps -->
<activity android:name=".SecretActivity" android:exported="true">  <!-- VULNERABILITY -->
    <intent-filter>
        <action android:name="com.example.ADMIN_ACTION" />
    </intent-filter>
</activity>

<!-- 5. Custom Permissions -->
<permission android:name="com.example.ADMIN"
    android:protectionLevel="normal" />  <!-- Weak protection level -->

<!-- 6. Content Provider Permissions -->
<provider android:name=".MyProvider"
    android:authorities="com.example.provider"
    android:exported="true"
    android:readPermission="com.example.READ"
    android:writePermission="" />  <!-- Missing write permission -->

<!-- 7. Intent Filter Analysis -->
<!-- Look for custom schemes that could be exploited -->
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="myapp" android:host="open" />  <!-- Deep link -->
</intent-filter>

<!-- 8. Service Analysis -->
<service android:name=".AdminService"
    android:exported="true"
    android:permission="" />  <!-- No permission required -->
```

#### Automated Manifest Analysis Script

```bash
#!/bin/bash
# manifest_analysis.sh - Analyze AndroidManifest.xml for security issues

MANIFEST="$1"

echo "=== Android Security Analysis: $MANIFEST ==="
echo ""

# Debuggable check
if grep -q 'android:debuggable="true"' "$MANIFEST"; then
    echo "[CRITICAL] Application is debuggable!"
fi

# Backup check
if grep -q 'android:allowBackup="true"' "$MANIFEST"; then
    echo "[HIGH] Application allows backup (data extraction possible)"
elif ! grep -q 'android:allowBackup' "$MANIFEST"; then
    echo "[MEDIUM] allowBackup not explicitly set (defaults to true on API < 12)"
fi

# Exported components
echo ""
echo "=== Exported Components ==="
grep -n 'android:exported="true"' "$MANIFEST" | while read -r line; do
    echo "[INFO] Exported: $line"
done

# Components with intent-filters (implicitly exported on older APIs)
echo ""
echo "=== Components with Intent Filters ==="
grep -B5 'intent-filter' "$MANIFEST" | grep -E '<(activity|service|receiver|provider)'

# Permissions
echo ""
echo "=== Declared Permissions ==="
grep '<permission ' "$MANIFEST"

# Custom URL schemes
echo ""
echo "=== Custom URL Schemes ==="
grep 'android:scheme' "$MANIFEST"

# Network security
echo ""
echo "=== Network Security Config ==="
if grep -q 'networkSecurityConfig' "$MANIFEST"; then
    echo "[INFO] Custom network security config found"
fi

# Uses cleartext traffic
if grep -q 'cleartextTrafficPermitted="true"' "$MANIFEST"; then
    echo "[HIGH] Cleartext traffic is permitted!"
fi

echo ""
echo "=== Analysis Complete ==="
```

---

### Resource Analysis

#### Key Resource Files to Examine

```bash
# 1. strings.xml - Contains all app strings
cat res/values/strings.xml
# Look for:
#   - API endpoints (http://, https://)
#   - Firebase URLs
#   - OAuth client IDs
#   - AWS keys
#   - Map API keys
#   - Debug/test strings

# 2. colors.xml, styles.xml - Usually safe but check for notes

# 3. XML configurations
ls res/xml/
# Look for:
#   - network_security_config.xml
#   - backup_rules.xml
#   - file_paths.xml (FileProvider)

# 4. Raw resources
ls res/raw/
# Look for:
#   - Configuration files
#   - Database templates
#   - Certificate files

# 5. Assets directory
ls assets/
# Look for:
#   - SQLite databases
#   - WebView content
#   - JavaScript files
#   - Configuration files
```

#### Firebase Configuration Analysis

```bash
# Find Firebase configuration
grep -r "firebaseio.com" .
grep -r "firebase" res/values/strings.xml

# Common Firebase strings:
# <string name="firebase_database_url">https://myapp.firebaseio.com</string>
# <string name="gcm_defaultSenderId">123456789</string>
# <string name="google_api_key">AIzaSy...</string>
# <string name="google_app_id">1:123:android:abc</string>
# <string name="project_id">my-project-id</string>

# Test Firebase Realtime Database access (no authentication)
curl -s "https://myapp.firebaseio.com/.json"

# Test Firestore access
curl -s "https://firestore.googleapis.com/v1/projects/my-project/databases/(default)/documents"
```

#### Asset Analysis

```bash
# Check for SQLite databases in assets
find . -name "*.db" -o -name "*.sqlite" -o -name "*.sqlite3"

# Examine SQLite databases
sqlite3 database.db ".tables"
sqlite3 database.db ".schema"
sqlite3 database.db "SELECT * FROM users;"

# Check for JavaScript files (WebView content)
find . -name "*.js" -o -name "*.html"

# Look for configuration files
find . -name "*.json" -o -name "*.xml" -o -name "*.properties" -o -name "*.cfg"

# Check for embedded certificates
find . -name "*.pem" -o -name "*.crt" -o -name "*.cer" -o -name "*.p12"

# Search for secrets in all files
grep -rn "api_key\|apikey\|api-key\|secret\|password\|token\|credential" .
grep -rn "AAAA[A-Za-z0-9_-]*:" .  # GCM/FCM server keys
```

---

### String Analysis

```bash
# Extract all strings from DEX file
strings classes.dex | grep -i "http\|api\|key\|secret\|password\|token\|url"

# Using dexdump
dexdump -d classes.dex | grep "const-string"

# Using Androguard
python3 -c "
from androguard.core.apk import APK
a = APK('application.apk')
for s, _ in a.get_files():
    print(s)
"

# Comprehensive string search
grep -rn \
    -e "https\?://" \
    -e "api[_-]\?key" \
    -e "secret" \
    -e "password" \
    -e "token" \
    -e "bearer" \
    -e "authorization" \
    -e "credential" \
    -e "firebase" \
    -e "amazonaws.com" \
    -e "private[_-]\?key" \
    -e "BEGIN.*PRIVATE" \
    -e "AES\|DES\|RSA\|MD5\|SHA" \
    output_dir/
```

### Certificate Analysis

```bash
# View APK signing certificate
keytool -printcert -jarfile application.apk

# Detailed certificate info
keytool -printcert -file CERT.RSA

# Using apksigner
apksigner verify --print-certs application.apk

# Extract certificate
unzip application.apk META-INF/CERT.RSA -d /tmp/cert
keytool -printcert -file /tmp/cert/META-INF/CERT.RSA

# Check certificate validity
openssl pkcs7 -in CERT.RSA -inform DER -print_certs

# Compare certificates (useful for detecting repackaged apps)
keytool -printcert -jarfile original.apk > original_cert.txt
keytool -printcert -jarfile modified.apk > modified_cert.txt
diff original_cert.txt modified_cert.txt

# What to look for:
# - Self-signed certificates (not from a CA)
# - Short validity periods
# - Weak key sizes (< 2048 bits)
# - Certificate mismatches across app versions
# - Known malware certificates
```

---

## Dynamic Analysis

### Frida Framework

Frida is the most powerful dynamic instrumentation toolkit for Android. It allows you to inject JavaScript snippets into running processes to hook methods, modify behavior, and inspect data at runtime.

#### Setting Up Frida

```bash
# Ensure Frida server is running on device
adb shell "su -c 'pkill frida-server'"
adb shell "su -c '/data/local/tmp/frida-server -D &'"

# Verify connection
frida-ps -U                    # List processes
frida-ps -U | grep -i <app>   # Find specific app
```

#### Basic Frida Commands

```bash
# Spawn and attach to an app
frida -U -f com.example.app -l script.js

# Attach to running app
frida -U com.example.app -l script.js

# Run with debug output
frida -U -f com.example.app -l script.js --debug

# List all running apps
frida-ps -Ua                   # Applications only
frida-ps -Uai                  # Include system apps
```

#### Frida Script Examples

**Script 1: Hook All Methods of a Class**

```javascript
// hook_class.js
Java.perform(function() {
    var MyClass = Java.use("com.example.app.MyClass");

    // Hook all overloads of a method
    MyClass.secretMethod.overload('java.lang.String').implementation = function(arg) {
        console.log("[+] secretMethod called with: " + arg);
        var result = this.secretMethod(arg);
        console.log("[+] secretMethod returned: " + result);
        return result;
    };
});
```

**Script 2: Enumerate All Loaded Classes**

```javascript
// enumerate_classes.js
Java.perform(function() {
    Java.enumerateLoadedClasses({
        onMatch: function(className) {
            if (className.includes("com.example")) {
                console.log("[CLASS] " + className);
            }
        },
        onComplete: function() {
            console.log("[*] Enumeration complete");
        }
    });
});
```

**Script 3: Enumerate Methods of a Class**

```javascript
// enumerate_methods.js
Java.perform(function() {
    var targetClass = Java.use("com.example.app.LoginActivity");
    var methods = targetClass.class.getDeclaredMethods();

    methods.forEach(function(method) {
        console.log("[METHOD] " + method.toString());
    });
});
```

**Script 4: Bypass Root Detection**

```javascript
// bypass_root.js
Java.perform(function() {

    // Hook common root detection methods
    var RootBeer = Java.use("com.scottyab.rootbeer.RootBeer");
    RootBeer.isRooted.implementation = function() {
        console.log("[+] RootBeer.isRooted() called - returning false");
        return false;
    };

    // Hook Runtime.exec for root checks
    var Runtime = Java.use("java.lang.Runtime");
    Runtime.exec.overload('java.lang.String').implementation = function(cmd) {
        console.log("[+] Runtime.exec() called with: " + cmd);
        if (cmd.includes("su") || cmd.includes("which") || cmd.includes("busybox")) {
            console.log("[!] Blocked root-related command: " + cmd);
            throw Java.use("java.io.IOException").$new("Permission denied");
        }
        return this.exec(cmd);
    };

    // Hook File.exists() for root file checks
    var File = Java.use("java.io.File");
    File.exists.implementation = function() {
        var path = this.getAbsolutePath();
        var rootPaths = [
            "/system/app/Superuser.apk",
            "/sbin/su",
            "/system/bin/su",
            "/system/xbin/su",
            "/data/local/xbin/su",
            "/data/local/bin/su",
            "/system/sd/xbin/su",
            "/system/bin/failsafe/su",
            "/data/local/su",
            "/su/bin/su",
            "/system/xbin/busybox",
            "/system/bin/busybox"
        ];

        for (var i = 0; i < rootPaths.length; i++) {
            if (path.includes(rootPaths[i])) {
                console.log("[!] Blocked root path check: " + path);
                return false;
            }
        }
        return this.exists();
    };

    // Hook Build properties
    var Build = Java.use("android.os.Build");
    Build.TAGS.value = "release-keys";

    console.log("[*] Root detection bypass loaded");
});
```

**Script 5: Bypass SSL Pinning**

```javascript
// bypass_ssl.js
Java.perform(function() {

    // Method 1: TrustManager bypass
    var X509TrustManager = Java.use("javax.net.ssl.X509TrustManager");
    var SSLContext = Java.use("javax.net.ssl.SSLContext");

    var TrustManager = Java.registerClass({
        name: "com.custom.TrustManager",
        implements: [X509TrustManager],
        methods: {
            checkClientTrusted: function(chain, authType) {},
            checkServerTrusted: function(chain, authType) {},
            getAcceptedIssuers: function() {
                return [];
            }
        }
    });

    var TrustManagers = [TrustManager.$new()];
    var SSLContextInit = SSLContext.init.overload(
        "[Ljavax.net.ssl.KeyManager;",
        "[Ljavax.net.ssl.TrustManager;",
        "java.security.SecureRandom"
    );

    SSLContextInit.implementation = function(keyManager, trustManager, secureRandom) {
        console.log("[+] SSLContext.init() called - injecting custom TrustManager");
        SSLContextInit.call(this, keyManager, TrustManagers, secureRandom);
    };

    // Method 2: OkHttp3 CertificatePinner bypass
    try {
        var CertificatePinner = Java.use("okhttp3.CertificatePinner");
        CertificatePinner.check.overload("java.lang.String", "java.util.List")
            .implementation = function(hostname, peerCertificates) {
                console.log("[+] OkHttp3 CertificatePinner.check() bypassed for: " + hostname);
                return;  // Skip pinning check
            };
    } catch(e) {
        console.log("[-] OkHttp3 CertificatePinner not found");
    }

    // Method 3: Network Security Config bypass
    try {
        var NetworkSecurityTrustManager = Java.use(
            "android.security.net.config.NetworkSecurityTrustManager"
        );
        NetworkSecurityTrustManager.checkServerTrusted.implementation = function() {
            console.log("[+] NetworkSecurityTrustManager bypassed");
        };
    } catch(e) {
        console.log("[-] NetworkSecurityTrustManager not found");
    }

    console.log("[*] SSL pinning bypass loaded");
});
```

**Script 6: Monitor Crypto Operations**

```javascript
// monitor_crypto.js
Java.perform(function() {

    // Monitor Cipher operations
    var Cipher = Java.use("javax.crypto.Cipher");
    Cipher.doFinal.overload("[B").implementation = function(input) {
        var algorithm = this.getAlgorithm();
        console.log("[CIPHER] Algorithm: " + algorithm);
        console.log("[CIPHER] Input: " + bytesToHex(input));
        var result = this.doFinal(input);
        console.log("[CIPHER] Output: " + bytesToHex(result));
        return result;
    };

    // Monitor SecretKeySpec creation
    var SecretKeySpec = Java.use("javax.crypto.spec.SecretKeySpec");
    SecretKeySpec.$init.overload("[B", "java.lang.String").implementation = function(key, algo) {
        console.log("[KEY] Algorithm: " + algo);
        console.log("[KEY] Key: " + bytesToHex(key));
        return this.$init(key, algo);
    };

    // Monitor MessageDigest
    var MessageDigest = Java.use("java.security.MessageDigest");
    MessageDigest.digest.overload("[B").implementation = function(input) {
        var algorithm = this.getAlgorithm();
        console.log("[HASH] Algorithm: " + algorithm);
        console.log("[HASH] Input: " + bytesToHex(input));
        var result = this.digest(input);
        console.log("[HASH] Output: " + bytesToHex(result));
        return result;
    };

    function bytesToHex(bytes) {
        if (bytes === null) return "null";
        var hex = [];
        for (var i = 0; i < bytes.length; i++) {
            hex.push(("0" + (bytes[i] & 0xFF).toString(16)).slice(-2));
        }
        return hex.join("");
    }

    console.log("[*] Crypto monitoring loaded");
});
```

**Script 7: Intercept and Modify HTTP Requests**

```javascript
// intercept_http.js
Java.perform(function() {

    // Hook OkHttp3
    try {
        var OkHttpClient = Java.use("okhttp3.OkHttpClient");
        var Request = Java.use("okhttp3.Request");
        var Interceptor = Java.use("okhttp3.Interceptor");
        var Buffer = Java.use("okio.Buffer");

        var RequestBuilder = Java.use("okhttp3.Request$Builder");

        // Hook URL building
        var HttpUrl = Java.use("okhttp3.HttpUrl");
        HttpUrl.parse.implementation = function(url) {
            console.log("[HTTP] URL: " + url);
            return this.parse(url);
        };
    } catch(e) {
        console.log("[-] OkHttp3 not found: " + e);
    }

    // Hook HttpURLConnection
    var HttpURLConnection = Java.use("java.net.HttpURLConnection");
    HttpURLConnection.setRequestProperty.implementation = function(key, value) {
        console.log("[HTTP] Header: " + key + ": " + value);
        if (key.toLowerCase() === "authorization") {
            console.log("[AUTH] Token found: " + value);
        }
        this.setRequestProperty(key, value);
    };

    // Hook URL class
    var URL = Java.use("java.net.URL");
    URL.$init.overload("java.lang.String").implementation = function(url) {
        console.log("[URL] " + url);
        this.$init(url);
    };

    console.log("[*] HTTP interceptor loaded");
});
```

**Script 8: Activity/Fragment Lifecycle Monitor**

```javascript
// monitor_lifecycle.js
Java.perform(function() {

    var Activity = Java.use("android.app.Activity");

    Activity.onCreate.overload("android.os.Bundle").implementation = function(savedInstanceState) {
        console.log("[LIFECYCLE] " + this.getClass().getName() + ".onCreate()");
        this.onCreate(savedInstanceState);
    };

    Activity.onResume.implementation = function() {
        console.log("[LIFECYCLE] " + this.getClass().getName() + ".onResume()");
        this.onResume();
    };

    Activity.onPause.implementation = function() {
        console.log("[LIFECYCLE] " + this.getClass().getName() + ".onPause()");
        this.onPause();
    };

    Activity.onDestroy.implementation = function() {
        console.log("[LIFECYCLE] " + this.getClass().getName() + ".onDestroy()");
        this.onDestroy();
    };

    // Monitor Intent data
    Activity.getIntent.implementation = function() {
        var intent = this.getIntent();
        if (intent !== null) {
            var action = intent.getAction();
            var data = intent.getDataString();
            if (action !== null) console.log("[INTENT] Action: " + action);
            if (data !== null) console.log("[INTENT] Data: " + data);
        }
        return intent;
    };

    console.log("[*] Lifecycle monitor loaded");
});
```

**Script 9: SharedPreferences Monitor**

```javascript
// monitor_sharedprefs.js
Java.perform(function() {

    var SharedPreferencesImpl = Java.use("android.app.SharedPreferencesImpl");

    // Monitor getString
    SharedPreferencesImpl.getString.implementation = function(key, defValue) {
        var result = this.getString(key, defValue);
        console.log("[PREFS] getString('" + key + "') = '" + result + "'");
        return result;
    };

    // Monitor putString
    SharedPreferencesImpl.putString.implementation = function(key, value) {
        console.log("[PREFS] putString('" + key + "', '" + value + "')");
        return this.putString(key, value);
    };

    // Monitor getInt
    SharedPreferencesImpl.getInt.implementation = function(key, defValue) {
        var result = this.getInt(key, defValue);
        console.log("[PREFS] getInt('" + key + "') = " + result);
        return result;
    };

    // Monitor getBoolean
    SharedPreferencesImpl.getBoolean.implementation = function(key, defValue) {
        var result = this.getBoolean(key, defValue);
        console.log("[PREFS] getBoolean('" + key + "') = " + result);
        return result;
    };

    console.log("[*] SharedPreferences monitor loaded");
});
```

**Script 10: Dump All Strings in Memory**

```javascript
// dump_strings.js
Java.perform(function() {
    Java.enumerateLoadedClasses({
        onMatch: function(className) {
            try {
                var clazz = Java.use(className);
                var fields = clazz.class.getDeclaredFields();
                fields.forEach(function(field) {
                    field.setAccessible(true);
                    try {
                        if (field.getType().getName() === "java.lang.String") {
                            var value = field.get(null);
                            if (value !== null && value.length > 0) {
                                console.log("[STRING] " + className + "." + field.getName() + " = " + value);
                            }
                        }
                    } catch(e) {}
                });
            } catch(e) {}
        },
        onComplete: function() {
            console.log("[*] String dump complete");
        }
    });
});
```

---

### Objection

Objection is a runtime mobile exploration toolkit powered by Frida. It provides a higher-level interface with pre-built commands.

#### Basic Usage

```bash
# Start exploration session
objection -g com.example.app explore

# Spawn mode (restart app)
objection -g com.example.app --startup-command "android sslpinning disable" explore

# Run a command file
objection -g com.example.app explore -c commands.txt
```

#### Key Objection Commands

```bash
# ===== ENVIRONMENT =====
# Get environment info
env

# Get device info
android info

# List installed apps
android hooking list activities

# ===== SSL PINNING =====
# Disable SSL pinning (one-liner!)
android sslpinning disable

# ===== ROOT DETECTION =====
# Disable root detection
android root disable

# ===== MEMORY =====
# Search memory for a string
memory search "secret_key"
memory search "48 65 6c 6c 6f" --pattern  # Hex pattern

# Dump memory
memory dump all dump.bin

# ===== HOOKING =====
# List activities
android hooking list activities

# List services
android hooking list services

# List receivers
android hooking list receivers

# List classes
android hooking list classes

# Search for classes
android hooking list classes | grep -i login

# Get methods of a class
android hooking list class_methods com.example.app.LoginActivity

# Watch a method (log when called)
android hooking watch class com.example.app.LoginActivity

# Watch specific method
android hooking watch class_method com.example.app.LoginActivity.checkPassword --dump-args --dump-return

# Watch all methods matching a pattern
android hooking watch class_method com.example.app.* --dump-args

# ===== FILE SYSTEM =====
# List files
file download /data/data/com.example.app/databases/app.db

# Read file
file cat /data/data/com.example.app/shared_prefs/user.xml

# ===== KEYSTORE =====
# List key store entries
android keystore list

# ===== JAVASCRIPT =====
# Execute custom JavaScript
import frida_script.js

# Execute inline
execute "console.log('hello')"

# ===== JOBS =====
# List running jobs
jobs list

# ===== UI =====
# Show UI hierarchy
android ui dump
```

---

### Xposed Framework

Xposed provides system-level hooking capabilities on rooted Android devices.

#### Installation

```bash
# Install Magisk first
# Then install LSPosed (modern Xposed for Android 8-14)
# Download LSPosed from https://github.com/LSPosed/LSPosed/releases

# Or use EdXposed for older devices
```

#### Xposed Module Example

```java
// HookModule.java
package com.example.hookmodule;

import de.robv.android.xposed.IXposedHookLoadPackage;
import de.robv.android.xposed.XC_MethodHook;
import de.robv.android.xposed.XposedBridge;
import de.robv.android.xposed.XposedHelpers;
import de.robv.android.xposed.callbacks.XC_LoadPackage;

public class HookModule implements IXposedHookLoadPackage {

    @Override
    public void handleLoadPackage(XC_LoadPackage.LoadPackageParam lpparam) throws Throwable {
        if (!lpparam.packageName.equals("com.target.app")) return;

        XposedBridge.log("Hooked into: " + lpparam.packageName);

        // Hook a method
        XposedHelpers.findAndHookMethod(
            "com.target.app.LoginActivity",
            lpparam.classLoader,
            "isValidPassword",
            String.class,
            new XC_MethodHook() {
                @Override
                protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
                    XposedBridge.log("Password check called: " + param.args[0]);
                    // Modify the password
                    param.args[0] = "modified_password";
                }

                @Override
                protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                    // Force return value to true
                    param.setResult(true);
                }
            }
        );

        // Hook all constructors
        XposedHelpers.findAndHookConstructor(
            "com.target.app.models.User",
            lpparam.classLoader,
            String.class,
            String.class,
            new XC_MethodHook() {
                @Override
                protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                    XposedBridge.log("User created: " + param.args[0]);
                }
            }
        );
    }
}
```

---

### Network Traffic Analysis

#### Burp Suite Setup

```bash
# 1. Configure Burp Suite proxy listener
#    Proxy → Options → Add → All Interfaces:8080

# 2. Install Burp CA certificate on Android device
#    - Visit http://<your-ip>:8080 in device browser
#    - Download CA certificate
#    - Go to Settings → Security → Install from storage
#    - OR: Settings → Encryption & credentials → Install certificate

# 3. Configure device proxy
#    - Settings → Wi-Fi → Modify network
#    - Proxy: Manual
#    - Host: <your-computer-ip>
#    - Port: 8080

# 4. For Android 7+ (system-level cert trust)
#    - Use Magisk module: MagiskTrustUserCerts
#    - OR: Add cert to app's network security config (requires repackaging)
```

#### mitmproxy Setup

```bash
# Install
pip3 install mitmproxy

# Start mitmproxy
mitmproxy --listen-port 8080

# Or use mitmweb for web interface
mitmweb --listen-port 8080

# Or use mitmdump for scripted analysis
mitmdump --listen-port 8080 -s script.py

# Example mitmproxy script
cat > script.py << 'EOF'
from mitmproxy import http

def request(flow: http.HTTPFlow) -> None:
    # Log all requests
    print(f"[REQ] {flow.request.method} {flow.request.pretty_url}")
    
    # Log headers
    for name, value in flow.request.headers.items():
        print(f"  {name}: {value}")
    
    # Log body
    if flow.request.content:
        print(f"  Body: {flow.request.content.decode('utf-8', errors='ignore')}")

def response(flow: http.HTTPFlow) -> None:
    print(f"[RES] {flow.response.status_code} {flow.request.pretty_url}")
    
    # Check for sensitive data in responses
    if flow.response.content:
        content = flow.response.content.decode('utf-8', errors='ignore')
        if 'token' in content.lower():
            print(f"  [!] Token found in response!")
        if 'password' in content.lower():
            print(f"  [!] Password-related content found!")
EOF

# Run script
mitmdump -s script.py --listen-port 8080
```

#### Wireshark Packet Capture

```bash
# Capture device traffic
# Method 1: tcpdump on rooted device
adb shell "su -c 'tcpdump -i any -s 0 -w /sdcard/capture.pcap'"
# Let it run, then pull the file
adb pull /sdcard/capture.pcap
# Open in Wireshark

# Method 2: Remote capture via adb
adb shell "su -c 'tcpdump -i wlan0 -s 0 -w -'" | wireshark -k -i -

# Useful Wireshark filters:
# dns                    - DNS queries
# http                   - HTTP traffic
# tcp.port == 443        - HTTPS traffic (encrypted)
# ip.addr == 10.0.0.1    - Specific host
# ssl.handshake          - TLS handshakes
```

#### Charles Proxy

```bash
# Download from https://www.charlesproxy.com/
# Configure similar to Burp Suite
# Advantages: Better SSL handling, cleaner UI
# Supports: HTTP, HTTPS, WebSocket, SOCKS
```

---

### Logcat Monitoring

```bash
# View all logs
adb logcat

# Filter by tag
adb logcat -s "MyApp"

# Filter by priority
adb logcat *:E  # Errors only
adb logcat *:W  # Warnings and above

# Filter by package
adb logcat --pid=$(adb shell pidof com.example.app)

# Clear log buffer
adb logcat -c

# Save to file
adb logcat -f /sdcard/logcat.txt

# Filter specific patterns
adb logcat | grep -i "password\|token\|key\|secret\|error\|exception"

# Monitor in real-time with color
adb logcat -v color | grep -E "(com.example|Error|Exception)"

# Common sensitive log tags to watch:
# - okhttp3          (HTTP requests)
# - Retrofit          (API calls)
# - FirebaseAuth      (Authentication)
# - SharedPreferences (Data storage)
# - SQLiteDatabase    (Database operations)
# - WebView           (Web content)
```

---

## Advanced Techniques

### Bypassing Root Detection

Applications use various techniques to detect rooted devices. Here's how to bypass each:

#### Common Root Detection Methods and Bypasses

```javascript
// comprehensive_root_bypass.js
Java.perform(function() {

    console.log("[*] Loading comprehensive root detection bypass...");

    // 1. Bypass common root detection libraries
    try {
        var RootBeer = Java.use("com.scottyab.rootbeer.RootBeer");
        RootBeer.isRooted.implementation = function() { return false; };
        RootBeer.isRootedWithBusyBoxCheck.implementation = function() { return false; };
        console.log("[+] RootBeer bypassed");
    } catch(e) {}

    try {
        var RootBeerNative = Java.use("com.scottyab.rootbeer.util.QLog");
        RootBeerNative.isXposedEnabled.implementation = function() { return false; };
    } catch(e) {}

    // 2. Bypass SafetyNet/Play Integrity
    try {
        var SafetyNetHelper = Java.use("com.scottyab.safetynet.SafetyNetHelper");
        SafetyNetHelper.attest.implementation = function() {
            console.log("[+] SafetyNet attestation bypassed");
        };
    } catch(e) {}

    // 3. Bypass file existence checks
    var File = Java.use("java.io.File");
    File.exists.implementation = function() {
        var path = this.getAbsolutePath();
        var suspiciousPaths = [
            "su", "busybox", "Superuser", "SuperSU",
            "Magisk", "xposed", "substrate", "frida",
            "/system/app/Superuser",
            "/system/xbin/su", "/system/bin/su",
            "/sbin/su", "/data/local/xbin/su",
            "/data/local/bin/su", "/data/local/tmp/su",
            "/su/bin/su", "/system/sd/xbin/su",
            "/system/bin/failsafe/su",
            "/system/xbin/busybox", "/system/bin/busybox",
            "/data/adb/magisk", "/data/adb/modules",
            "/proc/self/maps",  // Often checked for Frida
            "com.topjohnwu.magisk",
            "de.robv.android.xposed",
            "org.lsposed"
        ];

        for (var i = 0; i < suspiciousPaths.length; i++) {
            if (path.indexOf(suspiciousPaths[i]) !== -1) {
                return false;
            }
        }
        return this.exists();
    };

    // 4. Bypass Runtime.exec checks
    var Runtime = Java.use("java.lang.Runtime");
    var execMethods = [
        'exec(java.lang.String)',
        'exec([Ljava.lang.String;)',
        'exec(java.lang.String,[Ljava.lang.String;)',
        'exec([Ljava.lang.String,[Ljava.lang.String;)',
        'exec(java.lang.String,[Ljava.lang.String;,java.io.File)',
        'exec([Ljava.lang.String,[Ljava.lang.String;,java.io.File)'
    ];

    execMethods.forEach(function(sig) {
        try {
            Runtime.exec.overload(sig).implementation = function() {
                var cmd = arguments[0];
                if (typeof cmd === 'string') {
                    if (cmd.indexOf("su") !== -1 || cmd.indexOf("which") !== -1) {
                        console.log("[!] Blocked exec: " + cmd);
                        throw Java.use("java.io.IOException").$new("No such file");
                    }
                }
                return this.exec.apply(this, arguments);
            };
        } catch(e) {}
    });

    // 5. Bypass ProcessBuilder checks
    var ProcessBuilder = Java.use("java.lang.ProcessBuilder");
    ProcessBuilder.command.overload("java.util.List").implementation = function(cmd) {
        var cmdStr = cmd.toString();
        if (cmdStr.indexOf("su") !== -1 || cmdStr.indexOf("which") !== -1) {
            console.log("[!] Blocked ProcessBuilder: " + cmdStr);
            throw Java.use("java.io.IOException").$new("No such file");
        }
        return this.command(cmd);
    };

    // 6. Bypass Build.TAGS check
    var Build = Java.use("android.os.Build");
    try {
        Build.TAGS.value = "release-keys";
    } catch(e) {}

    // 7. Bypass Build properties
    try {
        Build.FINGERPRINT.value = "google/walleye/walleye:8.1.0/OPM1.171019.011/4448085:user/release-keys";
        Build.HARDWARE.value = "walleye";
        Build.MODEL.value = "Pixel 2";
        Build.MANUFACTURER.value = "Google";
        Build.BRAND.value = "google";
        Build.DEVICE.value = "walleye";
        Build.PRODUCT.value = "walleye";
        Build.HOST.value = "abfarm60";
        Build.DISPLAY.value = "OPM1.171019.011";
    } catch(e) {}

    // 8. Bypass package manager checks
    var PackageManager = Java.use("android.app.ApplicationPackageManager");
    PackageManager.getPackageInfo.overload("java.lang.String", "int").implementation = function(name, flags) {
        if (name.indexOf("supersu") !== -1 || 
            name.indexOf("superuser") !== -1 ||
            name.indexOf("magisk") !== -1 ||
            name.indexOf("xposed") !== -1 ||
            name.indexOf("frida") !== -1) {
            throw Java.use("android.content.pm.PackageManager$NameNotFoundException").$new(name);
        }
        return this.getPackageInfo(name, flags);
    };

    // 9. Bypass test-keys check
    System = Java.use("java.lang.System");
    System.getProperty.overload("java.lang.String").implementation = function(key) {
        if (key === "ro.build.tags") return "release-keys";
        return this.getProperty(key);
    };

    console.log("[*] Root detection bypass fully loaded");
});
```

#### Automated Bypass with Objection

```bash
objection -g com.example.app explore
# Then in objection console:
android root disable
```

---

### Bypassing SSL Pinning

#### Types of SSL Pinning

| Type | Description | Difficulty |
|------|-------------|------------|
| **No Pinning** | Only system CA validation | N/A |
| **Certificate Pinning** | Specific certificate hash pinned | Medium |
| **Public Key Pinning** | Public key hash pinned | Hard |
| **Network Security Config** | Android 7+ declarative pinning | Easy |
| **Custom TrustManager** | Custom validation logic | Medium |
| **OkHttp CertificatePinner** | OkHttp library pinning | Easy |
| **TrustKit** | Open-source pinning library | Easy |

#### Frida-Based Bypass

```javascript
// ssl_pinning_comprehensive.js
Java.perform(function() {

    console.log("[*] Comprehensive SSL pinning bypass...");

    // === 1. TrustManager bypass ===
    var X509TrustManager = Java.use("javax.net.ssl.X509TrustManager");
    var SSLContext = Java.use("javax.net.ssl.SSLContext");

    var TrustManager = Java.registerClass({
        name: "com.bypass.TrustAllManager",
        implements: [X509TrustManager],
        methods: {
            checkClientTrusted: function(chain, authType) {},
            checkServerTrusted: function(chain, authType) {},
            getAcceptedIssuers: function() { return []; }
        }
    });

    var EmptyTrustManagers = [TrustManager.$new()];

    // Override SSLContext.init
    SSLContext.init.overload(
        "[Ljavax.net.ssl.KeyManager;",
        "[Ljavax.net.ssl.TrustManager;",
        "java.security.SecureRandom"
    ).implementation = function(km, tm, sr) {
        console.log("[+] SSLContext.init() - injecting trust-all manager");
        this.init(km, EmptyTrustManagers, sr);
    };

    // === 2. OkHttp3 CertificatePinner ===
    try {
        var CertificatePinner = Java.use("okhttp3.CertificatePinner");
        CertificatePinner.check.overload("java.lang.String", "java.util.List")
            .implementation = function(hostname, peerCerts) {
                console.log("[+] OkHttp3 pinning bypassed for: " + hostname);
            };
    } catch(e) {
        console.log("[-] OkHttp3 not found");
    }

    // === 3. OkHttp3 CertificatePinner$check (newer versions) ===
    try {
        var CertificatePinner2 = Java.use("okhttp3.CertificatePinner");
        CertificatePinner2.check$okhttp.overload("java.lang.String", "kotlin.jvm.functions.Function0")
            .implementation = function(hostname, func) {
                console.log("[+] OkHttp3 (new) pinning bypassed for: " + hostname);
            };
    } catch(e) {}

    // === 4. TrustKit ===
    try {
        var TrustKit = Java.use("com.datatheorem.android.trustkit.TrustKit");
        TrustKit.getInstance().getTrustManager.overload("java.lang.String")
            .implementation = function(hostname) {
                console.log("[+] TrustKit bypassed for: " + hostname);
                return EmptyTrustManagers[0];
            };
    } catch(e) {}

    // === 5. Appcelerator Titanium ===
    try {
        var PinningTrustManager = Java.use("appcelerator.https.PinningTrustManager");
        PinningTrustManager.checkServerTrusted.implementation = function(chain, authType) {
            console.log("[+] Appcelerator pinning bypassed");
        };
    } catch(e) {}

    // === 6. Apache Cordova ===
    try {
        var CordovaClient = Java.use("org.apache.cordova.CordovaWebViewClient");
        CordovaClient.onReceivedSslError.implementation = function(view, handler, error) {
            console.log("[+] Cordova SSL error bypassed");
            handler.proceed();
        };
    } catch(e) {}

    // === 7. WebViewClient ===
    try {
        var WebViewClient = Java.use("android.webkit.WebViewClient");
        WebViewClient.onReceivedSslError.implementation = function(view, handler, error) {
            console.log("[+] WebViewClient SSL error bypassed");
            handler.proceed();
        };
    } catch(e) {}

    // === 8. HostnameVerifier ===
    var HostnameVerifier = Java.use("javax.net.ssl.HostnameVerifier");
    var AllowAllVerifier = Java.registerClass({
        name: "com.bypass.AllowAllHostnameVerifier",
        implements: [HostnameVerifier],
        methods: {
            verify: function(hostname, session) {
                console.log("[+] Hostname verification bypassed for: " + hostname);
                return true;
            }
        }
    });

    try {
        var HttpsURLConnection = Java.use("javax.net.ssl.HttpsURLConnection");
        HttpsURLConnection.setDefaultHostnameVerifier.implementation = function(verifier) {
            console.log("[+] Setting custom hostname verifier");
            this.setDefaultHostnameVerifier(AllowAllVerifier.$new());
        };
        HttpsURLConnection.setHostnameVerifier.implementation = function(verifier) {
            this.setHostnameVerifier(AllowAllVerifier.$new());
        };
    } catch(e) {}

    // === 9. NetworkSecurityConfig (Android 7+) ===
    try {
        var NetworkSecurityConfig = Java.use("android.security.net.config.NetworkSecurityConfig");
        NetworkSecurityConfig.isCleartextTrafficPermitted.implementation = function() {
            return true;
        };
    } catch(e) {}

    // === 10. Conscrypt ===
    try {
        var ConscryptFileDescriptorSocket = Java.use("com.android.org.conscrypt.ConscryptFileDescriptorSocket");
        ConscryptFileDescriptorSocket.setHostname.overload("java.lang.String").implementation = function(hostname) {
            // Allow any hostname
        };
    } catch(e) {}

    console.log("[*] SSL pinning bypass complete");
});
```

#### Method 2: Using Objection

```bash
objection -g com.example.app explore
# In objection:
android sslpinning disable
```

#### Method 3: Patching Network Security Config

```xml
<!-- Modify res/xml/network_security_config.xml -->
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
            <certificates src="user" />
        </trust-anchors>
    </base-config>
    <debug-overrides>
        <trust-anchors>
            <certificates src="user" />
        </trust-anchors>
    </debug-overrides>
</network-security-config>
```

```bash
# After modifying, rebuild with apktool
apktool b output_dir -o patched.apk
# Sign the APK
keytool -genkey -v -keystore my-key.keystore -alias mykey -keyalg RSA -keysize 2048 -validity 10000
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-key.keystore patched.apk mykey
# OR
apksigner sign --ks my-key.keystore --ks-key-alias mykey patched.apk
```

---

### Anti-Debugging Bypass

```javascript
// anti_debug_bypass.js
Java.perform(function() {

    console.log("[*] Loading anti-debugging bypass...");

    // 1. Bypass Debug.isDebuggerConnected()
    var Debug = Java.use("android.os.Debug");
    Debug.isDebuggerConnected.implementation = function() {
        return false;
    };

    // 2. Bypass Debug.waitForDebugger()
    Debug.waitForDebugger.implementation = function() {
        console.log("[+] waitForDebugger() bypassed");
    };

    // 3. Bypass ApplicationInfo.FLAG_DEBUGGABLE check
    var ApplicationInfo = Java.use("android.content.pm.ApplicationInfo");
    // The flags field might be checked
    // We'll hook getPackageInfo instead
    var PackageManager = Java.use("android.app.ApplicationPackageManager");
    PackageManager.getPackageInfo.overload("java.lang.String", "int").implementation = function(name, flags) {
        var info = this.getPackageInfo(name, flags);
        if (name === "com.example.app") {
            info.applicationInfo.flags.value &= ~0x2;  // Remove FLAG_DEBUGGABLE
        }
        return info;
    };

    // 4. Bypass TracerPid check (/proc/self/status)
    var BufferedReader = Java.use("java.io.BufferedReader");
    var InputStreamReader = Java.use("java.io.InputStreamReader");
    var FileInputStream = Java.use("java.io.FileInputStream");

    // Hook reading of /proc/self/status
    var Runtime = Java.use("java.lang.Runtime");
    Runtime.exec.overload("java.lang.String").implementation = function(cmd) {
        if (cmd.indexOf("TracerPid") !== -1 || cmd.indexOf("status") !== -1) {
            console.log("[!] Blocked TracerPid check");
            throw Java.use("java.io.IOException").$new("Permission denied");
        }
        return this.exec(cmd);
    };

    // 5. Bypass file-based checks
    var File = Java.use("java.io.File");
    File.exists.implementation = function() {
        var path = this.getAbsolutePath();
        if (path.indexOf("/proc/self/status") !== -1 || 
            path.indexOf("/proc/self/maps") !== -1) {
            return false;  // Pretend proc files don't exist
        }
        return this.exists();
    };

    // 6. Bypass System.exit() (used as anti-tamper)
    var System = Java.use("java.lang.System");
    System.exit.implementation = function(code) {
        console.log("[+] System.exit(" + code + ") blocked");
        // Don't call exit
    };

    // 7. Bypass Process.killProcess()
    var Process = Java.use("android.os.Process");
    Process.killProcess.implementation = function(pid) {
        console.log("[+] Process.killProcess(" + pid + ") blocked");
    };

    // 8. Bypass clock-based detection
    var SystemClock = Java.use("android.os.SystemClock");
    var elapsedRealtime = SystemClock.elapsedRealtime();

    SystemClock.elapsedRealtime.implementation = function() {
        return elapsedRealtime;
    };

    console.log("[*] Anti-debugging bypass loaded");
});
```

---

### Emulator Detection Bypass

```javascript
// emulator_bypass.js
Java.perform(function() {

    console.log("[*] Loading emulator detection bypass...");

    // 1. Bypass Build properties
    var Build = Java.use("android.os.Build");
    Build.FINGERPRINT.value = "google/walleye/walleye:8.1.0/OPM1.171019.011/4448085:user/release-keys";
    Build.MODEL.value = "Pixel 2";
    Build.MANUFACTURER.value = "Google";
    Build.BRAND.value = "google";
    Build.DEVICE.value = "walleye";
    Build.PRODUCT.value = "walleye";
    Build.HARDWARE.value = "walleye";
    Build.HOST.value = "abfarm60";
    Build.TAGS.value = "release-keys";
    Build.TYPE.value = "user";
    Build.BOARD.value = "walleye";
    Build.ID.value = "OPM1.171019.011";
    Build.DISPLAY.value = "walleye-user 8.1.0 OPM1.171019.011 4448085 release-keys";
    Build.SERIAL.value = "HT7BM1A00001";

    // 2. Bypass SystemProperties
    try {
        var SystemProperties = Java.use("android.os.SystemProperties");
        SystemProperties.get.overload("java.lang.String").implementation = function(key) {
            var props = {
                "ro.hardware": "walleye",
                "ro.product.model": "Pixel 2",
                "ro.product.brand": "google",
                "ro.product.device": "walleye",
                "ro.product.board": "walleye",
                "ro.build.type": "user",
                "ro.build.tags": "release-keys",
                "ro.boot.hardware": "walleye",
                "ro.serialno": "HT7BM1A00001",
                "gsm.version.baseband": "1.0.0.0"
            };
            if (key in props) return props[key];
            return this.get(key);
        };
    } catch(e) {}

    // 3. Bypass file checks for emulator artifacts
    var File = Java.use("java.io.File");
    File.exists.implementation = function() {
        var path = this.getAbsolutePath();
        var emulatorPaths = [
            "/dev/socket/qemud",
            "/dev/qemu_pipe",
            "/system/lib/libc_malloc_debug_qemu.so",
            "/sys/qemu_trace",
            "/system/bin/qemu-props",
            "/dev/socket/genyd",
            "/dev/socket/baseband_genyd",
            "/dev/goldfish_pipe",
            "/system/etc/init.goldfish.rc"
        ];
        for (var i = 0; i < emulatorPaths.length; i++) {
            if (path.indexOf(emulatorPaths[i]) !== -1) {
                return false;
            }
        }
        return this.exists();
    };

    // 4. Bypass IMEI/Telephony checks
    var TelephonyManager = Java.use("android.telephony.TelephonyManager");
    TelephonyManager.getDeviceId.implementation = function() {
        return "354154081234567";  // Valid-looking IMEI
    };
    TelephonyManager.getLine1Number.implementation = function() {
        return "+1234567890";
    };
    TelephonyManager.getNetworkOperatorName.implementation = function() {
        return "T-Mobile";
    };
    TelephonyManager.getNetworkCountryIso.implementation = function() {
        return "us";
    };
    TelephonyManager.getSimOperatorName.implementation = function() {
        return "T-Mobile";
    };

    // 5. Bypass android.os.Build checks for emulator strings
    try {
        Build.FINGERPRINT.value = Build.FINGERPRINT.value.replace("generic", "walleye");
        Build.FINGERPRINT.value = Build.FINGERPRINT.value.replace("unknown", "walleye");
        Build.MODEL.value = Build.MODEL.value.replace("sdk", "Pixel 2");
    } catch(e) {}

    // 6. Bypass sensor checks
    var SensorManager = Java.use("android.hardware.SensorManager");
    SensorManager.getSensorList.implementation = function(type) {
        var list = this.getSensorList(type);
        if (list.size() === 0) {
            // Return a fake sensor list for emulator
            console.log("[+] Empty sensor list detected, returning fake data");
        }
        return list;
    };

    // 7. Bypass battery check (emulators often show always charging)
    // This is usually detected through BroadcastReceiver

    console.log("[*] Emulator detection bypass loaded");
});
```

---

### Tampering and Repackaging

#### Step-by-Step APK Modification

```bash
# 1. Decompile with Apktool
apktool d target.apk -o target_decompiled/

# 2. Make modifications (Smali, Manifest, Resources)
# Example: Enable debuggable flag
# Edit target_decompiled/AndroidManifest.xml
# Add: android:debuggable="true" to <application>

# 3. Modify Smali code
# Example: Bypass a check in a Smali file
# Edit target_decompiled/smali/com/example/app/Check.smali

# 4. Rebuild
apktool b target_decompiled/ -o target_modified.apk

# 5. Align the APK
zipalign -v 4 target_modified.apk target_aligned.apk

# 6. Generate a signing key (if you don't have one)
keytool -genkeypair -v \
    -keystore my-release-key.keystore \
    -alias my-alias \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000

# 7. Sign the APK
apksigner sign \
    --ks my-release-key.keystore \
    --ks-key-alias my-alias \
    --out target_signed.apk \
    target_aligned.apk

# 8. Verify signature
apksigner verify target_signed.apk

# 9. Install on device
adb install target_signed.apk
# OR
adb install -r target_signed.apk  # Replace existing
```

#### Advanced: Adding Frida Gadget

```bash
# Frida Gadget allows dynamic instrumentation without root
# 1. Download frida-gadget for your architecture
wget https://github.com/frida/frida/releases/download/16.1.4/frida-gadget-16.1.4-android-arm64.so.xz
xz -d frida-gadget-16.1.4-android-arm64.so.xz

# 2. Decompile APK
apktool d target.apk -o target_decompiled/

# 3. Place gadget in lib directory
cp frida-gadget-16.1.4-android-arm64.so target_decompiled/lib/arm64-v8a/libfrida-gadget.so

# 4. Inject System.loadLibrary call in the main Activity's onCreate
# In the Smali of the launcher Activity, add:
# const-string v0, "frida-gadget"
# invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V

# 5. Add internet permission if not present
# In AndroidManifest.xml:
# <uses-permission android:name="android.permission.INTERNET" />

# 6. Rebuild, sign, and install
apktool b target_decompiled/ -o target_modified.apk
zipalign -v 4 target_modified.apk target_aligned.apk
apksigner sign --ks my-key.keystore --ks-key-alias mykey target_aligned.apk
adb install target_aligned.apk

# 7. Connect to gadget
frida -U Gadget
```

#### Advanced: Method Hooking with Smali

```smali
# Example: Change a method to always return true
# Original method that checks license:
.method public isLicensed()Z
    .registers 3

    # ... complex license checking code ...
    
    return v0
.end method

# Modified (patched) method:
.method public isLicensed()Z
    .registers 2
    
    # Simply return true
    const/4 v0, 0x1
    return v0
.end method
```

---

### Memory Analysis

```bash
# 1. Dump process memory with Frida
frida -U com.example.app -e "
    var modules = Process.enumerateModules();
    modules.forEach(function(m) {
        console.log(JSON.stringify(m));
    });
"

# 2. Search memory for specific values
frida -U com.example.app -e "
    Memory.scan(ptr('0x7f00000000'), 0x1000000, '48 65 6c 6c 6f', {
        onMatch: function(address, size) {
            console.log('Found at: ' + address);
        },
        onComplete: function() {
            console.log('Scan complete');
        }
    });
"

# 3. Dump specific memory region
frida -U com.example.app -e "
    var module = Process.findModuleByName('libnative.so');
    var bytes = module.base.readByteArray(module.size);
    var file = new File('/tmp/dump.bin', 'wb');
    file.write(bytes);
    file.close();
    console.log('Dumped to /tmp/dump.bin');
"

# 4. Using objection for memory operations
objection -g com.example.app explore
# In objection:
memory dump all memory_dump.bin
memory search "password"
memory search "41 42 43" --pattern  # Hex pattern
```

---

### Native Library Analysis

#### Extracting and Analyzing .so Files

```bash
# 1. Extract native libraries from APK
unzip application.apk "lib/*" -d extracted_libs/

# List all native libraries
find extracted_libs/ -name "*.so" -exec file {} \;

# 2. Basic analysis
file lib/arm64-v8a/libnative.so
# Output: ELF 64-bit LSB shared object, ARM aarch64

# 3. Extract strings
strings lib/arm64-v8a/libnative.so | head -50
strings lib/arm64-v8a/libnative.so | grep -i "api\|key\|secret\|http\|password"

# 4. Check exported symbols
readelf -s lib/arm64-v8a/libnative.so | grep -i "FUNC"
nm -D lib/arm64-v8a/libnative.so

# 5. Check dynamic dependencies
readelf -d lib/arm64-v8a/libnative.so
ldd lib/arm64-v8a/libnative.so  # May not work on all platforms

# 6. Disassemble with objdump
aarch64-linux-gnu-objdump -d lib/arm64-v8a/libnative.so | head -100
```

#### Ghidra Analysis Workflow

```
1. Open Ghidra → Create New Project
2. Import the .so file: File → Import File → select .so
3. Auto-analyze: Yes when prompted
4. Wait for analysis to complete
5. Navigation:
   - Symbol Tree → Functions → Look for interesting functions
   - Search → For Strings → Find "http://", "api", "key"
   - Decompiler window shows pseudo-C code
6. Key functions to look for:
   - JNI_OnLoad: Entry point, often contains anti-analysis checks
   - Java_*: JNI native method implementations
   - Functions with "check", "verify", "validate" in name
   - Functions handling encryption/decryption
   - Functions with URL strings
7. Cross-reference analysis:
   - Right-click → References → Show all references to/from
```

#### Using Ghidra Headless (Command Line)

```bash
# Run Ghidra analysis in headless mode
analyzeHeadless /path/to/project ProjectName \
    -import libnative.so \
    -postScript DecompileAll.java \
    -scriptPath /path/to/scripts/
```

#### Frida Hooking for Native Functions

```javascript
// native_hook.js
// Hook a native function
var moduleName = "libnative.so";
var funcName = "Java_com_example_app_Native_checkLicense";

var nativeFunc = Module.findExportByName(moduleName, funcName);
if (nativeFunc) {
    Interceptor.attach(nativeFunc, {
        onEnter: function(args) {
            console.log("[NATIVE] " + funcName + " called");
            // args[0] = JNIEnv*
            // args[1] = jobject (this) or jclass (static)
            // args[2+] = actual arguments
            
            // Read string argument
            var env = Java.vm.getEnv();
            var jstr = Java.cast(args[2], Java.use("java.lang.String"));
            console.log("[NATIVE] String arg: " + jstr);
        },
        onLeave: function(retval) {
            console.log("[NATIVE] Return value: " + retval);
            // Change return value
            retval.replace(ptr(1));  // Return true
        }
    });
}

// Hook JNI_OnLoad
var JNI_OnLoad = Module.findExportByName(moduleName, "JNI_OnLoad");
if (JNI_OnLoad) {
    Interceptor.attach(JNI_OnLoad, {
        onEnter: function(args) {
            console.log("[NATIVE] JNI_OnLoad called");
        },
        onLeave: function(retval) {
            console.log("[NATIVE] JNI_OnLoad returned: " + retval);
        }
    });
}
```

---

## Automation and Scripting

### Python-Based Analysis with Androguard

```python
#!/usr/bin/env python3
# androguard_analysis.py

from androguard.core.apk import APK
from androguard.core.dex import DEX
from androguard.misc import AnalyzeAPK
import sys
import re

def analyze_apk(apk_path):
    """Comprehensive APK analysis using Androguard"""
    
    print(f"[*] Analyzing: {apk_path}")
    
    # Load APK
    a = APK(apk_path)
    d, dx = AnalyzeAPK(apk_path)
    
    # Basic info
    print("\n=== Basic Information ===")
    print(f"Package: {a.get_package()}")
    print(f"App Name: {a.get_app_name()}")
    print(f"Version: {a.get_androidversion_name()}")
    print(f"Version Code: {a.get_androidversion_code()}")
    print(f"Min SDK: {a.get_min_sdk_version()}")
    print(f"Target SDK: {a.get_target_sdk_version()}")
    print(f"Debuggable: {a.get_attribute_value('application', 'debuggable')}")
    print(f"Allow Backup: {a.get_attribute_value('application', 'allowBackup')}")
    
    # Permissions
    print("\n=== Permissions ===")
    permissions = a.get_permissions()
    dangerous = [
        'READ_CONTACTS', 'READ_SMS', 'READ_CALL_LOG', 'READ_PHONE_STATE',
        'CAMERA', 'RECORD_AUDIO', 'ACCESS_FINE_LOCATION',
        'READ_EXTERNAL_STORAGE', 'WRITE_EXTERNAL_STORAGE',
        'SEND_SMS', 'CALL_PHONE', 'READ_CALENDAR'
    ]
    for perm in permissions:
        risk = " [DANGEROUS]" if any(d in perm for d in dangerous) else ""
        print(f"  {perm}{risk}")
    
    # Activities
    print("\n=== Activities ===")
    for activity in a.get_activities():
        exported = a.get_attribute_value('activity', 'exported', name=activity)
        print(f"  {activity} (exported={exported})")
    
    # Services
    print("\n=== Services ===")
    for service in a.get_services():
        print(f"  {service}")
    
    # Receivers
    print("\n=== Broadcast Receivers ===")
    for receiver in a.get_receivers():
        print(f"  {receiver}")
    
    # Providers
    print("\n=== Content Providers ===")
    for provider in a.get_procected_providers():
        print(f"  {provider}")
    
    # URL extraction
    print("\n=== URLs Found ===")
    urls = set()
    for s in dx.get_strings():
        found = re.findall(r'https?://[^\s"\'<>]+', str(s))
        urls.update(found)
    for url in sorted(urls):
        print(f"  {url}")
    
    # Potential secrets
    print("\n=== Potential Secrets ===")
    patterns = {
        'API Key': r'(?i)api[_-]?key["\s:=]+["\']?([A-Za-z0-9_\-]{20,})',
        'AWS Key': r'(?i)(AKIA[0-9A-Z]{16})',
        'Firebase URL': r'https://[a-z0-9-]+\.firebaseio\.com',
        'Generic Secret': r'(?i)(secret|password|token|key)["\s:=]+["\']?([^\s"\']{8,})',
        'Private Key': r'-----BEGIN.*PRIVATE KEY-----',
        'JWT': r'eyJ[A-Za-z0-9_-]+\.eyJ[A-Za-z0-9_-]+\.[A-Za-z0-9_-]+',
    }
    
    for s in dx.get_strings():
        for name, pattern in patterns.items():
            matches = re.findall(pattern, str(s))
            if matches:
                print(f"  [{name}] {matches}")
    
    # Crypto usage
    print("\n=== Crypto Usage ===")
    crypto_classes = [
        'Ljavax/crypto/Cipher;',
        'Ljavax/crypto/spec/SecretKeySpec;',
        'Ljavax/crypto/spec/IvParameterSpec;',
        'Ljava/security/MessageDigest;',
        'Ljavax/crypto/Mac;',
    ]
    for cls in crypto_classes:
        try:
            class_analysis = dx.get_class_analysis(cls)
            if class_analysis:
                for _, method, _ in class_analysis.get_xref_from():
                    print(f"  Used in: {method}")
        except:
            pass
    
    print("\n[*] Analysis complete")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python3 androguard_analysis.py <apk_file>")
        sys.exit(1)
    analyze_apk(sys.argv[1])
```

### Automated Frida Script Runner

```python
#!/usr/bin/env python3
# frida_runner.py

import frida
import sys
import time
import os

def on_message(message, data):
    if message['type'] == 'send':
        print(f"[APP] {message['payload']}")
    elif message['type'] == 'error':
        print(f"[ERROR] {message['stack']}")
    else:
        print(f"[?] {message}")

def run_script(package_name, script_path):
    """Attach to app and run Frida script"""
    
    # Read script
    with open(script_path, 'r') as f:
        script_code = f.read()
    
    # Connect to USB device
    device = frida.get_usb_device()
    
    # Spawn app
    pid = device.spawn([package_name])
    print(f"[*] Spawned {package_name} with PID: {pid}")
    
    # Attach
    session = device.attach(pid)
    print(f"[*] Attached to PID: {pid}")
    
    # Create and load script
    script = session.create_script(script_code)
    script.on('message', on_message)
    script.load()
    print(f"[*] Script loaded: {script_path}")
    
    # Resume app
    device.resume(pid)
    print(f"[*] App resumed")
    
    # Keep running
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        print("\n[*] Detaching...")
        session.detach()

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python3 frida_runner.py <package_name> <script.js>")
        sys.exit(1)
    
    run_script(sys.argv[1], sys.argv[2])
```

### Bash Automation Script

```bash
#!/bin/bash
# full_analysis.sh - Automated Android APK analysis

APK="$1"
OUTPUT_DIR="analysis_$(date +%Y%m%d_%H%M%S)"

if [ -z "$APK" ]; then
    echo "Usage: $0 <apk_file>"
    exit 1
fi

echo "=== Android APK Automated Analysis ==="
echo "Target: $APK"
echo "Output: $OUTPUT_DIR"
echo ""

mkdir -p "$OUTPUT_DIR"

# 1. Basic file info
echo "[*] Getting file info..."
file "$APK" > "$OUTPUT_DIR/file_info.txt"
md5sum "$APK" >> "$OUTPUT_DIR/file_info.txt"
sha256sum "$APK" >> "$OUTPUT_DIR/file_info.txt"
ls -lh "$APK" >> "$OUTPUT_DIR/file_info.txt"

# 2. Extract with Apktool
echo "[*] Decompiling with Apktool..."
apktool d "$APK" -o "$OUTPUT_DIR/apktool_output" -f 2>/dev/null

# 3. Decompile with JADX
echo "[*] Decompiling with JADX..."
jadx -d "$OUTPUT_DIR/jadx_output" --deobf "$APK" 2>/dev/null

# 4. Extract and analyze manifest
echo "[*] Analyzing manifest..."
if [ -f "$OUTPUT_DIR/apktool_output/AndroidManifest.xml" ]; then
    cp "$OUTPUT_DIR/apktool_output/AndroidManifest.xml" "$OUTPUT_DIR/manifest.xml"
    
    # Check debuggable
    grep -q 'debuggable="true"' "$OUTPUT_DIR/manifest.xml" && \
        echo "[CRITICAL] App is debuggable!" >> "$OUTPUT_DIR/security_issues.txt"
    
    # Check allowBackup
    grep -q 'allowBackup="true"' "$OUTPUT_DIR/manifest.xml" && \
        echo "[HIGH] App allows backup" >> "$OUTPUT_DIR/security_issues.txt"
    
    # Find exported components
    grep 'exported="true"' "$OUTPUT_DIR/manifest.xml" > "$OUTPUT_DIR/exported_components.txt"
    
    # Find permissions
    grep '<uses-permission' "$OUTPUT_DIR/manifest.xml" > "$OUTPUT_DIR/permissions.txt"
fi

# 5. Extract strings
echo "[*] Extracting strings..."
strings "$APK" | grep -iE "http|api|key|secret|password|token|firebase|amazon" \
    > "$OUTPUT_DIR/interesting_strings.txt"

# 6. Search for secrets
echo "[*] Searching for secrets..."
grep -rn -i \
    -e "api[_-]\?key" \
    -e "secret" \
    -e "password" \
    -e "bearer" \
    -e "token" \
    -e "private[_-]\?key" \
    -e "BEGIN.*PRIVATE" \
    -e "AKIA[0-9A-Z]{16}" \
    "$OUTPUT_DIR/jadx_output/" > "$OUTPUT_DIR/potential_secrets.txt" 2>/dev/null

# 7. Search for URLs
echo "[*] Extracting URLs..."
grep -rnoE "https?://[a-zA-Z0-9./?=_%&-]+" "$OUTPUT_DIR/jadx_output/" \
    > "$OUTPUT_DIR/urls.txt" 2>/dev/null
sort -u "$OUTPUT_DIR/urls.txt" > "$OUTPUT_DIR/unique_urls.txt"

# 8. Analyze certificate
echo "[*] Analyzing certificate..."
keytool -printcert -jarfile "$APK" > "$OUTPUT_DIR/certificate.txt" 2>/dev/null

# 9. Check for native libraries
echo "[*] Checking native libraries..."
if [ -d "$OUTPUT_DIR/apktool_output/lib" ]; then
    find "$OUTPUT_DIR/apktool_output/lib" -name "*.so" > "$OUTPUT_DIR/native_libs.txt"
    while read lib; do
        echo "--- $lib ---" >> "$OUTPUT_DIR/native_lib_info.txt"
        file "$lib" >> "$OUTPUT_DIR/native_lib_info.txt"
        strings "$lib" | grep -iE "http|api|key|secret" >> "$OUTPUT_DIR/native_lib_strings.txt"
    done < "$OUTPUT_DIR/native_libs.txt"
fi

# 10. Summary
echo ""
echo "=== Analysis Summary ==="
echo "Security Issues: $(wc -l < "$OUTPUT_DIR/security_issues.txt" 2>/dev/null || echo 0)"
echo "URLs Found: $(wc -l < "$OUTPUT_DIR/unique_urls.txt" 2>/dev/null || echo 0)"
echo "Secret Candidates: $(wc -l < "$OUTPUT_DIR/potential_secrets.txt" 2>/dev/null || echo 0)"
echo "Native Libraries: $(wc -l < "$OUTPUT_DIR/native_libs.txt" 2>/dev/null || echo 0)"
echo ""
echo "Results saved to: $OUTPUT_DIR/"
```

---

## Common Vulnerability Patterns

### OWASP Mobile Top 10 (2024)

| # | Category | Example |
|---|----------|---------|
| M1 | Improper Credential Usage | Hardcoded API keys in source |
| M2 | Inadequate Supply Chain Security | Vulnerable third-party libraries |
| M3 | Insecure Authentication/Authorization | Missing auth on sensitive endpoints |
| M4 | Insufficient Input/Output Validation | SQL injection via Content Provider |
| M5 | Insecure Communication | HTTP instead of HTTPS, no cert pinning |
| M6 | Inadequate Privacy Controls | PII stored in plain text |
| M7 | Insufficient Binary Protections | No obfuscation, debuggable |
| M8 | Security Misconfiguration | Exported components without permissions |
| M9 | Insecure Data Storage | Sensitive data in SharedPreferences |
| M10 | Insufficient Cryptography | Weak algorithms (DES, MD5) |

### Vulnerability Checklist

```
□ Insecure Data Storage
  □ Sensitive data in SharedPreferences (unencrypted)
  □ Sensitive data in SQLite database (unencrypted)
  □ Sensitive data in external storage
  □ Sensitive data in log files
  □ Sensitive data in backup
  □ Credentials stored in plain text
  □ Encryption keys hardcoded in source

□ Insecure Communication
  □ HTTP instead of HTTPS
  □ No SSL/TLS certificate validation
  □ Weak cipher suites accepted
  □ No certificate pinning (for sensitive apps)
  □ Cleartext traffic permitted
  □ Custom TrustManager accepts all certificates

□ Insecure Authentication
  □ Weak password policy
  □ No brute force protection
  □ Authentication bypass possible
  □ Session management issues
  □ Token not invalidated on logout

□ Insecure Authorization
  □ Horizontal privilege escalation
  □ Vertical privilege escalation
  □ Missing function-level access control
  □ Insecure Direct Object References (IDOR)

□ Code Quality Issues
  □ SQL injection
  □ Path traversal
  □ Code injection
  □ Buffer overflows (native code)
  □ Race conditions
  □ Memory leaks

□ Client Code Quality
  □ Debuggable in production
  □ No code obfuscation
  □ Root detection missing
  □ Anti-tampering missing
  □ Anti-debugging missing
  □ Emulator detection missing
  □ WebView JavaScript interface exposed
  □ WebView file access enabled

□ Tampering & Reverse Engineering
  □ No code obfuscation (ProGuard/R8)
  □ No native library protection
  □ No integrity checking
  □ Easy to repackaged
  □ No anti-hooking measures
```

---

## Reporting Findings

### Report Template

```markdown
# Android Application Security Assessment Report

## Executive Summary
- Application: [App Name] v[Version]
- Package: com.example.app
- Assessment Date: [Date]
- Tester: [Name]
- Overall Risk: [Critical/High/Medium/Low]

## Scope
- Application version tested
- Devices/emulators used
- Testing methodology

## Findings Summary

| # | Title | Severity | Status |
|---|-------|----------|--------|
| 1 | Hardcoded API Key | Critical | Open |
| 2 | SSL Pinning Missing | High | Open |
| 3 | Weak Password Policy | Medium | Open |

## Detailed Findings

### Finding 1: Hardcoded API Key

**Severity:** Critical
**CVSS Score:** 7.5
**CWE:** CWE-798 (Use of Hard-Coded Credentials)

**Description:**
The application contains a hardcoded API key in the source code...

**Location:**
File: `com/example/app/Config.java`
Line: 42
```
public static final String API_KEY = "sk-1234567890";
```

**Impact:**
An attacker can extract this key and...

**Recommendation:**
Store API keys securely using...

**References:**
- OWASP M1: Improper Credential Usage
- CWE-798

### Finding 2: ...

## Appendix
- Tool versions used
- Testing environment details
- Raw data/logs
```

---

## Legal and Ethical Considerations

### Legal Framework

```
IMPORTANT: Always ensure you have proper authorization before
reverse engineering any application.

Laws that may apply:
├── DMCA (USA) - Digital Millennium Copyright Act
├── CFAA (USA) - Computer Fraud and Abuse Act
├── EU Copyright Directive
├── National laws of your jurisdiction
└── Terms of Service of the application

Permitted uses (generally):
├── Security research (with authorization)
├── Interoperability (in some jurisdictions)
├── Education and learning
├── Bug bounty programs (within scope)
├── Your own applications
└── Open source software analysis

Prohibited uses:
├── Piracy and copyright infringement
├── Bypassing DRM for illegal purposes
├── Stealing trade secrets
├── Creating malware
├── Unauthorized access to services
└── Violating Terms of Service
```

### Responsible Disclosure

```
1. Document the vulnerability thoroughly
2. Contact the developer/vendor
3. Provide a reasonable timeframe for fix (usually 90 days)
4. Do not publicly disclose before fix is available
5. Do not exploit the vulnerability for personal gain
6. Follow the organization's bug bounty rules (if applicable)
```

---

## Resources and Further Reading

### Books

```
- "Android Hacker's Handbook" - Joshua Drake et al.
- "The Mobile Application Hacker's Handbook" - Dominic Chell et al.
- "Android Security Internals" - Nikolay Elenkov
- "Hacking Android" - Srinivasa Rao Kotipalli, Mohammed A. Imran
- "Practical Binary Analysis" - Dennis Andriesse
```

### Online Resources

```
- OWASP Mobile Security Testing Guide (MSTG)
  https://mas.owasp.org/

- OWASP Mobile Top 10
  https://owasp.org/www-project-mobile-top-10/

- Frida Documentation
  https://frida.re/docs/home/

- Android Developer Documentation
  https://developer.android.com/

- Android Security Bulletin
  https://source.android.com/docs/security/bulletin

- CWE - Common Weakness Enumeration
  https://cwe.mitre.org/

- Exploit Database (Android)
  https://www.exploit-db.com/
```

### Practice Applications

```
- DIVA (Damn Insecure and Vulnerable App)
  https://github.com/OWASP/owasp-mstg/tree/master/Crackmes

- InsecureBankv2
  https://github.com/dineshshetty/Android-InsecureBankv2

- MSTG Crackmes
  https://github.com/OWASP/owasp-mstg/tree/master/Crackmes

- OWASP UnCrackable Apps
  https://github.com/OWASP/owasp-mstg/tree/master/Crackmes

- AndroGoat
  https://github.com/satishpatnayak/AndroGoat

- Vuldroid
  https://github.com/Jewel591/Vuldroid
```

### Conferences and Communities

```
- DEF CON
- Black Hat
- OWASP meetings
- Mobile Security-focused Discord servers
- Reddit: r/ReverseEngineering, r/NetSec, r/Android
- XDA Developers forums
```

---

## Cheat Sheets

### ADB Commands

```bash
# Device management
adb devices                        # List connected devices
adb devices -l                     # Detailed device list
adb -s <serial> shell              # Connect to specific device
adb shell                          # Open shell on device

# Package management
adb shell pm list packages         # List all packages
adb shell pm list packages -3      # Third-party packages only
adb shell pm path <package>        # Get APK path
adb shell pm dump <package>        # Package info
adb shell pm clear <package>       # Clear app data
adb install app.apk                # Install APK
adb install -r app.apk             # Replace existing
adb install -t app.apk             # Allow test APKs
adb uninstall <package>            # Uninstall app

# File operations
adb pull <remote> <local>          # Pull file from device
adb push <local> <remote>          # Push file to device

# App operations
adb shell am start -n <component>              # Start activity
adb shell am start -a <action> -d <data>       # Start with intent
adb shell am force-stop <package>              # Stop app
adb shell am broadcast -a <action>             # Send broadcast

# Debugging
adb logcat                         # View logs
adb logcat -c                      # Clear logs
adb logcat -s <tag>                # Filter by tag
adb bugreport                      # Generate bug report
adb shell dumpsys                  # System info
adb shell dumpsys activity         # Activity info
adb shell dumpsys meminfo <pkg>    # Memory info

# Screenshots/Recording
adb shell screencap /sdcard/screen.png
adb pull /sdcard/screen.png
adb shell screenrecord /sdcard/video.mp4

# System
adb shell getprop                  # System properties
adb shell getprop ro.product.cpu.abi  # CPU architecture
adb shell settings list secure     # Secure settings
adb shell settings list system     # System settings

# Network
adb shell ifconfig                 # Network interfaces
adb shell netstat                  # Network connections
adb shell ping <host>              # Ping test
adb reverse tcp:<local> tcp:<remote>  # Reverse port forwarding
adb forward tcp:<local> tcp:<remote>  # Port forwarding
```

### Frida Cheat Sheet

```bash
# Process operations
frida-ps -U                        # List USB processes
frida-ps -Ua                       # Applications only
frida-ps -Uai                      # Include system apps
frida-ps -D <device>               # Specific device

# Attaching
frida -U <package>                 # Attach to running app
frida -U -f <package>              # Spawn app
frida -U -f <package> -l script.js # Spawn and load script
frida -U -f <package> --no-pause   # Don't pause at start
frida -U <package> -l script.js -o output.log  # Log to file

# Remote device
frida -H <host>:<port> <package>   # Remote device

# Tracing
frida-trace -U <package> -i "*encrypt*"  # Trace matching functions
frida-trace -U <package> -j "*LoginActivity*"  # Java tracing
```

### Common Frida One-Liners

```javascript
// Enumerate loaded classes matching pattern
Java.perform(function(){Java.enumerateLoadedClasses({onMatch:function(c){if(c.match(/login/i))console.log(c)},onComplete:function(){}})})

// Get all methods of a class
Java.perform(function(){var c=Java.use("com.example.app.LoginActivity");c.class.getDeclaredMethods().forEach(function(m){console.log(m)})})

// Hook a method and log args
Java.perform(function(){var c=Java.use("com.example.app.ApiClient");c.sendRequest.implementation=function(a){console.log("sendRequest:",a);return this.sendRequest(a)}})

// Read SharedPreferences
Java.perform(function(){var ctx=Java.use("android.app.ActivityThread").currentApplication().getApplicationContext();var prefs=ctx.getSharedPreferences("user",0);console.log(prefs.getAll())})

// Get current Activity
Java.perform(function(){var am=Java.use("android.app.ActivityThread").currentActivityThread();var activities=am.mActivities.value;var it=activities.values().iterator();while(it.hasNext()){var r=it.next();if(!r.value.paused.value)console.log(r.value.activity.value.getClass().getName())}})
```

### Useful grep Patterns

```bash
# Find API keys
grep -rn "api[_-]\?key\|apikey\|apiKey" --include="*.java" --include="*.kt" .

# Find hardcoded secrets
grep -rn "secret\|password\|token\|credential" --include="*.java" --include="*.kt" .

# Find URLs
grep -rnoE "https?://[^\"' ]+" --include="*.java" --include="*.kt" .

# Find crypto usage
grep -rn "Cipher\|DES\|AES\|RSA\|MD5\|SHA1\|SecretKey" --include="*.java" .

# Find SQL queries
grep -rn "rawQuery\|execSQL\|query(" --include="*.java" .

# Find logging
grep -rn "Log\.\(d\|v\|i\|w\|e\)" --include="*.java" .

# Find WebView usage
grep -rn "WebView\|setJavaScript\|addJavascriptInterface\|loadUrl" --include="*.java" .

# Find exported components
grep -n 'exported="true"' AndroidManifest.xml

# Find permissions
grep '<uses-permission' AndroidManifest.xml
```

---

## Final Notes

This guide provides a comprehensive foundation for Android reverse engineering. Key takeaways:

1. **Start with static analysis** - JADX and Apktool are your primary tools
2. **Move to dynamic analysis** - Frida is essential for runtime exploration
3. **Network analysis is critical** - Burp Suite/mitmproxy for traffic inspection
4. **Native code requires special tools** - Ghidra for .so file analysis
5. **Always stay legal** - Get proper authorization before testing
6. **Practice regularly** - Use intentionally vulnerable apps to build skills
7. **Document everything** - Proper reporting is essential for professional work

Remember: **With great power comes great responsibility.** Use these skills ethically and legally.

---

*Last Updated: July 2026*
*Guide Version: 2.0*
*Author: Arena.ai Agent*
