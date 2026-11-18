# Awesome Mobile Security

> A practitioner-built reference for Android and iOS security — covering the full stack from app analysis and reverse engineering through kernel internals, malware research, and advanced platform exploitation. No fluff, no marketing copy. Built for security engineers who need to find the right tool or resource fast.

Updated March 2026. Contributions welcome.

---

## Table of Contents

| # | Section |
|---|---------|
| 1 | [Tools — Static Analysis](#1-tools--static-analysis) |
| 2 | [Tools — Dynamic Analysis & Instrumentation](#2-tools--dynamic-analysis--instrumentation) |
| 3 | [Tools — Reverse Engineering](#3-tools--reverse-engineering) |
| 4 | [Tools — Network Analysis & Traffic Interception](#4-tools--network-analysis--traffic-interception) |
| 5 | [Tools — Malware Analysis](#5-tools--malware-analysis) |
| 6 | [Lab Environments & Vulnerable Apps](#6-lab-environments--vulnerable-apps) |
| 7 | [Research Papers & Technical Write-ups](#7-research-papers--technical-write-ups) |
| 8 | [Conference Talks](#8-conference-talks) |
| 9 | [Exploit Techniques & Attack Surface Reference](#9-exploit-techniques--attack-surface-reference) |
| 10 | [Advanced Platform Internals](#10-advanced-platform-internals) |
| 11 | [CTF Challenges & Writeups](#11-ctf-challenges--writeups) |
| 12 | [Books](#12-books) |
| 13 | [Community, Blogs & Researchers](#13-community-blogs--researchers) |
| 14 | [Threat Intelligence](#14-threat-intelligence) |
| 15 | [Courses & Certifications](#15-courses--certifications) |
| 16 | [Standards & Frameworks](#16-standards--frameworks) |
| 17 | [Official Documentation](#17-official-documentation) |
| 18 | [Regulatory & Compliance](#18-regulatory--compliance) |
| 19 | [Bug Bounty Programs](#19-bug-bounty-programs) |
| 20 | [Free Practical Challenges](#20-free-practical-challenges) |

---

**Platform labels:** `[Android]` `[iOS]` `[Cross-Platform]`

**Skill labels:** `[Beginner]` `[Intermediate]` `[Advanced]` `[Emerging]`

---

## 1. Tools — Static Analysis

Static analysis covers decompilation, disassembly, code scanning, and automated vulnerability detection — performed without running the application. Start here for initial triage of any APK or IPA.

### Android Decompilation & Code Analysis

| Tool | Description | Notes |
|------|-------------|-------|
| [jadx](https://github.com/skylot/jadx) `[Android]` | DEX-to-Java decompiler with both CLI and GUI modes. Best-in-class for Android APK decompilation — handles obfuscated code better than most alternatives. | Active development. jadx-gui includes search, cross-reference navigation, and code comments. First choice for APK analysis. |
| [apktool](https://github.com/iBotPeaches/Apktool) `[Android]` | Decodes and rebuilds APKs. Extracts smali bytecode, resources, and AndroidManifest.xml in editable form. Essential for manifest inspection and resource analysis. | Use for patching and rebuilding APKs — jadx is better for code reading, apktool for manipulation. |
| [dex2jar](https://github.com/pxb1988/dex2jar) `[Android]` | Converts DEX bytecode to JAR for use with Java decompilers like JD-GUI or Procyon. | Complementary to jadx. Useful when jadx decompilation fails or produces poor output on certain classes. |
| [smali/baksmali](https://github.com/JesusFreke/smali) `[Android]` | Assembler and disassembler for Android DEX format. Gold standard for low-level bytecode analysis and precise patching. | Required for cases where Java-level decompilation loses fidelity. Essential for understanding obfuscated code. `[Intermediate]` |
| [androguard](https://github.com/androguard/androguard) `[Android]` | Python library for programmatic APK and DEX analysis. Control-flow graph generation, inter-procedural analysis, malware classification. | Best for automated analysis pipelines and custom tooling. Well-maintained. `[Intermediate]` |

### Android Scanning & Automated Analysis

| Tool | Description | Notes |
|------|-------------|-------|
| [MobSF (Mobile Security Framework)](https://github.com/MobSF/Mobile-Security-Framework-MobSF) `[Cross-Platform]` | Automated all-in-one framework covering static analysis, dynamic analysis, malware detection, and API security analysis for Android and iOS. Runs as a Docker container or standalone server. | Most complete automated scanner available. Output includes CVSS-scored findings, code references, and network analysis. Start here for fast initial assessment. |
| [Quark Engine](https://github.com/ev-flow/quark-engine) `[Android]` | Behavior-based APK analysis engine. Uses a custom crime and behavior detection model rather than signature matching. Integrates with MobSF. | Good complement to MobSF for behavior-based detection of malicious patterns in custom code. |
| [APKLeaks](https://github.com/dwisiswant0/apkleaks) `[Android]` | Scans APKs for hardcoded URLs, endpoints, API keys, and secrets using configurable regex patterns. | Fast triage tool — run early in any assessment to surface exposed credentials and endpoints. |
| [FlutterGuard](https://github.com/anasfik/FlutterGuard) `[Android]` | Open-source Android app that audits a released Flutter app or APK on the device: manifest policy, `libapp.so` strings, assets, dependencies, endpoints and signing. No INTERNET permission; reports stay on the phone. | Fills the Flutter gap: app logic sits in `libapp.so`, so DEX-only scans miss strings and endpoints. MIT licensed; Google Play and GitHub APK. |
| [APKiD](https://github.com/rednaga/APKiD) `[Android]` | Identifies packers, protectors, obfuscators, and anti-analysis techniques in APKs using compiler and packer fingerprints. | Run before analysis to understand what protections are in place. Identifies Dexguard, ProGuard, DexProtect, and 30+ other packers. |
| [semgrep](https://github.com/semgrep/semgrep) `[Cross-Platform]` | Static analysis with mobile-specific community rulesets for Android and iOS security patterns. Scans source code rather than compiled artifacts. | Best for source-available assessments and SAST in CI/CD pipelines. Mobile rulesets at [semgrep.dev/r](https://semgrep.dev/r). |
| [APKLab](https://github.com/Surendrajat/APKLab) `[Android]` | Android reverse engineering workbench as a VS Code extension. Integrates apktool, jadx, smali, and apksigner into an IDE workflow. | Reduces context-switching between tools. Good for analysts who prefer an IDE over standalone GUIs. |
| [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) `[Android]` | Static data flow analysis for Android apps. Tracks how sensitive data (location, contacts, credentials) propagates through app code to sinks (network, storage, logs). | Academic-grade tool for privacy and data leakage analysis. `[Advanced]` |
| [mariana-trench](https://github.com/facebook/mariana-trench) `[Android]` | Meta's security-focused static analysis tool for Android and Java. Scales to large codebases. Detects data flows, injection vulnerabilities, and insecure patterns. | Production-grade tool used internally at Meta. Better suited to large apps than academic taint analysis tools. |
| [trueseeing](https://github.com/alterakey/trueseeing) `[Android]` | Fast, accurate Android vulnerability scanner. Checks for common security anti-patterns including exported components, crypto misuse, and weak storage. | Good for automated pipeline scanning. |
| [BlackDex](https://github.com/CodingGay/BlackDex) `[Android]` | Android runtime unpacking tool (dex dump). Defeats dex packers by dumping decrypted DEX from memory at runtime. | Essential for analyzing packed/protected APKs where static decompilation produces unreadable output. `[Intermediate]` |

### Android App Bundle & Split APK Handling

Working with `.aab` files (Android App Bundles) requires specific tooling — you cannot directly install or decompile a `.aab` without first converting it.

| Tool | Description | Notes |
|------|-------------|-------|
| [bundletool](https://github.com/google/bundletool) `[Android]` | Official Google tool for building, analyzing, and extracting APKs from Android App Bundles (.aab). Generates device-specific APK sets or universal APKs from bundles. | Required for working with AAB files. Use `bundletool build-apks --bundle=app.aab --output=app.apks` then extract and analyze individual APKs. |

**Workflow for AAB analysis:**
1. `bundletool build-apks --bundle=app.aab --output=app.apks --mode=universal` — produces a universal APK
2. Rename `app.apks` to `app.zip`, extract, locate `universal.apk`
3. Analyze with jadx or MobSF as a standard APK

Split APK sets (where a single app is delivered as multiple `.apk` files) can be reassembled the same way.

### iOS Static Analysis & Decryption

| Tool | Description | Notes |
|------|-------------|-------|
| [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) `[iOS]` | Static analysis of IPA files including binary metadata, embedded frameworks, plist parsing, URL scheme enumeration, and ATS configuration checks. | Requires an unencrypted IPA. App Store apps must be decrypted first (see below). |
| [class-dump](https://github.com/nygard/class-dump) `[iOS]` | Dumps Objective-C class interfaces, protocols, and categories from Mach-O binaries. Standard for inspecting Objective-C app structure. | Use `dsdump` for Swift or mixed Swift/ObjC binaries — handles Swift metadata better. |
| [dsdump](https://github.com/DerekSelander/dsdump) `[iOS]` | Improved class dumper with better handling of Swift metadata, enums, and structs. Produces more complete output for Swift apps than class-dump. | **Archived December 2025.** Author recommends [ipsw](https://github.com/blacktop/ipsw) as replacement. Still useful as a reference. `[Intermediate]` |
| [frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) `[iOS]` | Decrypts and dumps IPA files from jailbroken devices using Frida. Handles FairPlay decryption at runtime. | Requires jailbroken device with Frida server running. Standard approach for App Store IPA decryption. |
| [bagbak](https://github.com/ChiChou/bagbak) `[iOS]` | Modern IPA decryption tool using Frida. Cleaner interface than frida-ios-dump, better handling of app extensions and Watch apps. | Preferred over Clutch (abandoned) and over frida-ios-dump for newer iOS versions. |
| [ktool](https://github.com/0cyn/ktool) `[iOS]` | Static Mach-O analysis and modification toolkit. Header dumps, library dependency analysis, load command inspection, symbol table extraction. | Good for quick structural analysis of iOS binaries without a full disassembler. `[Intermediate]` |
| [SwiftDump](https://github.com/neil-wu/SwiftDump/) `[iOS]` | Command-line tool for extracting Swift object/struct/enum metadata from Mach-O binaries. Complements dsdump. | Useful when dsdump output is incomplete on certain Swift builds. |
| [iLEAPP](https://github.com/abrignoni/iLEAPP) `[iOS]` | iOS Logs, Events, And Plist Parser. Parses iOS backup and file system artifacts for forensic investigation — app data, location history, call logs, messages. | Primary tool for iOS forensic artifact parsing. Regularly updated with new artifact parsers. `[Intermediate]` |
| [XMachOViewer](https://github.com/horsicq/XMachOViewer) `[iOS/macOS]` | Cross-platform graphical Mach-O viewer. Inspect load commands, sections, symbols, and dynamic library dependencies. | Lightweight — useful for quick structural inspection without a full disassembler. |
| [BinaryCookieReader](https://github.com/as0ler/BinaryCookieReader) `[iOS]` | Reads iOS binary cookie files (`.binarycookies` format) from app data directories. | Useful during iOS app security assessments for inspecting stored web session cookies. |

### Cross-Platform Framework Static Analysis

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [blutter](https://github.com/worawit/blutter) | `[Flutter]` | Flutter application reverse engineering tool. Extracts Dart code and function signatures from snapshot files (libapp.so) in compiled Flutter apps. March 2026 update supports latest Flutter versions. | Primary tool for Flutter RE. Output dramatically reduces analysis time compared to manual Dart snapshot parsing. `[Advanced]` |
| [reFlutter](https://github.com/ptswarm/reFlutter) | `[Flutter]` | Flutter recompilation framework. Patches the Flutter engine to enable traffic interception and bypass certificate pinning. Requires app repackaging. | Alternative to blutter for traffic analysis focus. Also useful for patching anti-analysis checks. `[Advanced]` |
| [hermes-dec](https://github.com/P1sec/hermes-dec) | `[React Native]` | Decompiler for Hermes bytecode (`.hbc` files) used in React Native apps. Converts bytecode back to readable JavaScript. | Essential for modern React Native apps that ship with Hermes. Output quality varies but significantly better than raw bytecode inspection. |
| [dnSpy](https://github.com/dnSpy/dnSpy) | `[Xamarin/.NET]` | .NET debugger and assembly editor. Essential for Xamarin and MAUI app analysis — decompiles, edits, and debugs .NET assemblies. | Note: dnSpy is no longer actively maintained. Use [dnSpyEx](https://github.com/dnSpyEx/dnSpy) (community fork) for ongoing updates. |
| [ILSpy](https://github.com/icsharpcode/ILSpy) | `[Xamarin/.NET]` | .NET assembly browser and decompiler. Open-source, actively maintained. | Better maintenance than dnSpy. Use for read-only decompilation; dnSpyEx for editing/debugging. |

### Security Scanners & CI/CD Integration

| Tool | Description | Notes |
|------|-------------|-------|
| [AppSweep](https://platform.guardsquare.com) `[Android/iOS]` | Free CI/CD integrated mobile app security scanner from Guardsquare. Upload APKs/IPAs or integrate via Gradle plugin. Identifies hardcoded keys, dangerous permissions, insecure configurations, and known-vulnerable SDK versions. | Free tier available. Clean, actionable output. Good for developer-facing security feedback in CI. |
| [jadx-ai-mcp](https://github.com/mobilehackinglab/jadx-mcp-plugin) `[Android]` | MCP plugin for jadx that enables AI assistants to interact with decompiled code directly. Query class structures, search for patterns, and navigate code through AI interfaces. | New 2025. Useful for accelerating analysis workflows with AI-assisted code review. `[Emerging]` |
| [Ostorlab](https://ostorlab.co) `[Android/iOS]` | MAST platform with AI-assisted monkey testing, privacy analysis (21 data collection categories), attack surface profiling, and CI/CD integration. Free tier available. | Actively developed alternative to MobSF for automated assessments. May 2025 update added advanced privacy tracking detection. `[Intermediate]` |

---

## 2. Tools — Dynamic Analysis & Instrumentation

Dynamic analysis covers runtime inspection, function hooking, behavior monitoring, and API tracing — performed while the application is running. Most mobile security research requires dynamic analysis to understand real behavior, bypass protections, and observe network and data flows.

### Core Instrumentation

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [Frida](https://frida.re) | `[Cross-Platform]` | Dynamic instrumentation toolkit. The de facto standard for mobile security research. Injects a JavaScript engine into running processes, enabling function hooking, memory inspection, and API tracing across Android and iOS. | Frida 17 (May 2025): **breaking change** — bundled runtime bridges (frida-objc, frida-swift, frida-java-bridge) removed from core; install separately via `frida-pm` (new package manager). Multiple 17.x releases through late 2025. ARM64e improvements critical for iOS 17+ research. Master Frida before all other dynamic analysis tools. |
| [objection](https://github.com/sensepost/objection) | `[Cross-Platform]` | Runtime mobile exploration toolkit built on Frida. Provides high-level commands for SSL pinning bypass, root/jailbreak bypass, memory dumping, file system access, and class/method enumeration. | Best starting point for Frida-based analysis. Covers 90% of common assessment tasks without writing custom Frida scripts. v1.12.3. `[Beginner–Intermediate]` |
| [Medusa](https://github.com/Ch0pin/medusa) | `[Cross-Platform]` | Modular Frida-based analysis framework with 90+ modules. Covers function tracing, crypto hooking, network interception, certificate validation bypass, anti-analysis bypass, and more. | More extensible than objection. v3.2 (2024) adds Android 14+ API support. Choose Medusa when you need specific modules or want to combine multiple hooks. `[Intermediate]` |
| [r2frida](https://github.com/nowsecure/r2frida) | `[Cross-Platform]` | Integration between radare2 and Frida. Combines radare2's static analysis with live Frida instrumentation — search memory, disassemble at runtime, trace native calls. | Best for native code analysis requiring both static context and runtime observation. `[Advanced]` |

### Android Dynamic Analysis

| Tool | Description | Notes |
|------|-------------|-------|
| [Drozer](https://github.com/WithSecureLabs/drozer) `[Android]` | Android security assessment framework for attacking IPC mechanisms. Tests exported Activities, Content Providers, Broadcast Receivers, and Services. Identifies intent injection and content provider traversal vulnerabilities. | The standard tool for Android IPC security testing. Maintained by WithSecure. `[Intermediate]` |
| [ZygiskFrida](https://github.com/lico-n/ZygiskFrida) `[Android]` | Stealthy Frida injection via Zygisk (Magisk module). Injects Frida gadget at app startup, bypassing many frida-server detection mechanisms. | Essential for testing apps with anti-tampering or Frida detection. Significantly harder to detect than a running frida-server. `[Advanced]` |
| [ADB (Android Debug Bridge)](https://developer.android.com/tools/adb) `[Android]` | Core Android SDK tool for device interaction — install/uninstall apps, shell access, logcat, file transfer, port forwarding. Built into Android SDK. | Required baseline for all Android analysis. Master `adb shell`, `adb logcat`, and `adb forward` before anything else. `[Beginner]` |
| [androidqf](https://github.com/botherder/androidqf) `[Android]` | Rapid forensic data collection from Android devices. Captures installed packages, running processes, network connections, accessibility services, and persistence mechanisms. Amnesty International tool. | Updated March 2026. Best first-pass tool for triage of potentially compromised Android devices. No root required. |
| [Runtime Mobile Security (RMS)](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security) `[Android/iOS]` | Web-based Frida GUI for real-time API hooking. Intercept and modify calls to crypto libraries, URL sessions, file operations, and custom methods through a browser interface. | Lower barrier to entry for interactive hooking sessions. Good for team-based analysis. |

### iOS Dynamic Analysis

| Tool | Description | Notes |
|------|-------------|-------|
| [Grapefruit](https://github.com/ChiChou/Grapefruit) `[iOS]` | Web-based iOS app analysis tool for jailbroken devices. Features file manager, class-dump, Frida REPL, hooking UI, and keychain inspector — all accessible through a browser. | v0.13.1. Actively maintained. Lowest-friction entry point for iOS dynamic analysis on jailbroken devices. `[Beginner–Intermediate]` |
| [idb (iOS Development Bridge)](https://github.com/facebook/idb) `[iOS]` | Meta's iOS testing tool for automating device and simulator interactions. App installation, launch, kill, log streaming, video recording, accessibility inspection. | Best for scripting repeatable iOS analysis workflows. Integrates well with CI environments. |
| [LLDB](https://lldb.llvm.org) `[iOS]` | Xcode's debugger. The standard debugging environment for iOS reverse engineering at the native level. Supports Python scripting for automated analysis. | Required for any iOS native code debugging or exploit development. Learn LLDB commands and scripting alongside Frida. `[Intermediate–Advanced]` |
| [iSH / a-shell](https://github.com/ish-app/ish) `[iOS]` | Linux shell environments for iOS. Useful for on-device testing without a Mac. Limited but handy for specific scenarios. | Supplementary tool — not a replacement for desktop-based analysis. |

### Additional Dynamic Analysis & Emulation

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [fishhook](https://github.com/facebook/fishhook) `[iOS]` | Meta library for dynamically rebinding symbols in Mach-O binaries running on iOS. Used to intercept C-level functions in iOS apps. | Useful for hooking system library calls without Frida when building lightweight iOS instrumentation tools. `[Intermediate]` |
| [Fridax](https://github.com/NorthwaveSecurity/fridax) `[Cross-Platform]` | Frida-based toolkit for reading variables and intercepting functions in Xamarin/Mono JIT and AOT compiled iOS/Android applications. | Fills the gap for .NET-based mobile app analysis where standard Frida scripts don't apply. |
| [FridaHookSwiftAlamofire](https://github.com/neil-wu/FridaHookSwiftAlamofire) `[iOS]` | Frida script that captures GET/POST HTTP requests made via the Alamofire Swift library and disables SSL pinning. | Useful reference for hooking Swift networking libraries — adapts to other Swift HTTP clients with modification. |
| [House](https://github.com/nccgroup/house) `[Android]` | Runtime mobile application analysis toolkit with a web GUI. NCC Group. Frida-powered interface for hooking, method tracing, and traffic interception. | Web interface alternative to objection. Lower barrier for analysts new to Frida. |
| [Inspeckage](https://github.com/ac-pm/Inspeckage) `[Android]` | Xposed-based Android dynamic analysis tool. Hooks common Android APIs — crypto, file I/O, network, shared preferences — and displays results in a web UI. | Requires Xposed Framework. Still useful on older Android versions. Largely superseded by Medusa on modern Android. |
| [Dwarf](https://github.com/iGio90/Dwarf) `[Cross-Platform]` | Multi-architecture debugger built on PyQt5 and Frida. Visual debugger UI for Frida-based analysis. | Full-featured GUI debugging experience over Frida. Useful for long analysis sessions requiring a visual interface. |
| [unidbg](https://github.com/zhkl0228/unidbg) `[Android]` | Emulates Android ARM32/ARM64 native libraries and iOS binaries without a real device. Useful for analyzing native code in isolated environments. | Good for malware analysis and understanding anti-tamper checks in native code. `[Advanced]` |
| [Qiling](https://github.com/qilingframework/qiling) `[Cross-Platform]` | Advanced binary emulation framework supporting multiple architectures and OS environments. More flexible than unidbg but requires more setup. | Best for complex emulation scenarios requiring custom OS environment simulation. `[Advanced]` |
| [LiME](https://github.com/504ensicsLabs/LiME) `[Android]` | Linux Memory Extractor. Loadable kernel module for capturing full memory dumps from running Android devices. | Required for memory forensics on Android. Root and kernel module loading required. `[Advanced]` |
| [fridump](https://github.com/Nightbringer21/fridump) `[Cross-Platform]` | Universal memory dumper using Frida. Dumps memory regions from running processes on Android and iOS. | Quick memory dump tool. Useful for extracting decrypted content, keys, and runtime strings. |

### Stealthy Frida & Anti-Detection

Advanced apps implement Frida detection. These tools and techniques help:

- **[ZygiskFrida](https://github.com/lico-n/ZygiskFrida)** — Zygisk-based injection avoids frida-server process detection

- **Frida gadget mode** — Embed `frida-gadget.so` directly in the app instead of running a server; harder to detect than an external frida-server process

- **Custom Frida builds** — Rename Frida internals and recompile to evade string-based detection; community guides and forks available

- **[objection](https://github.com/sensepost/objection)'s `--startup-command`** — Inject bypass scripts at process initialization before detection logic runs

### Notable Updates (2025–2026)

- **[Frida 17.9.1](https://github.com/frida/frida/releases)** (March 2026): The 17.x series has moved fast — 35+ releases since 17.0 (May 2025). Key milestones: 17.8.0 shipped `frida-strace`, a cross-platform syscall tracer that works without a jailbreak on iOS and works by user account on Android; 17.6.0 eliminated the long-standing intrusive Zygote/system_server injection that caused crashes on Android; 17.0 removed runtime bridges (frida-objc, frida-swift, frida-java-bridge) from core — install separately via `frida-pm`. ARM64e improvements throughout critical for iOS 17+ research.

- **[objection 1.12.4](https://github.com/sensepost/objection/releases)** (March 2026): 1.12.0 added RootBeer and JailMonkey bypass, pattern-based hooking for Android and iOS, and Android keystore listing details. 1.12.4 (2026-03-25) added `reconnect` and `reconnect_spawn` commands and fixed attach-to-PID-0 edge case.

- **KernelSU-Next v3.1.0** (February 2026): Community fork of KernelSU with broader device compatibility (kernels 4.4–6.6). Root managed at kernel level — harder to detect than Magisk from userspace. Preferred for GKI Android research devices. [https://github.com/KernelSU-Next/KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next)

- **[Medusa v3.2.0](https://github.com/Ch0pin/medusa/releases)** (November 2025): 90+ modules, Android 14 API coverage, improved iOS module reliability. New manifest diffing across app versions and multi-session support for parallel analysis. New `Stheno` subproject for Android intent monitoring.

- **[ZygiskFrida v1.9.0](https://github.com/lico-n/ZygiskFrida/releases)** (October 2025): Has become essential post-2023 as Frida detection in banking and fintech apps has increased significantly. Tracks Frida gadget version updates; current bundled gadget is 17.4.0.

### iOS Jailbreak Tweaks for Security Testing

Cydia/Sileo tweaks that are useful for security research on jailbroken iOS devices:

**Jailbreak Detection Bypass:**
- **[Shadow](https://github.com/jjolano/shadow)** — Lightweight general-purpose jailbreak detection bypass. Low overhead, broad compatibility.

- **Liberty Lite** — General purpose jailbreak detection bypass. Per-app configuration via Settings. Install from Ryley Angus's repo (`ryleyangus.com/repo/`) in Cydia/Sileo.

- **KernBypass** — Kernel-level jailbreak detection bypass. More thorough than userspace tweaks. Install from ichitaso's repo (`cydia.ichitaso.com`) in Cydia/Sileo. `[Advanced]`

- **vnodebypass** — Hides jailbreak files from process file system visibility. Complements other bypass tweaks. Install from ichitaso's repo (`cydia.ichitaso.com`) in Cydia/Sileo.

- **[Hestia](https://repo.packix.com/)** — Global jailbreak detection bypass tweak. Works across all apps without per-app configuration.

**SSL Pinning Bypass:**
- **[SSL Kill Switch 2](https://github.com/nabla-c0d3/ssl-kill-switch2)** — Disables SSL/TLS certificate validation globally including certificate pinning. Standard first-pass SSL bypass for jailbroken iOS.

- **[SSLBypass](https://github.com/evilpenguin/SSLBypass)** — Alternative SSL pinning bypass tweak. iOS 8–14.

**Utilities:**
- **Filza File Manager** — Full file system access with IPA installer and terminal. Essential for manual app data inspection. Available via Sileo/Cydia from TIGI Software repo.

- **[AppSync Unified](https://github.com/akemin-dayo/AppSync)** — Enables installing unsigned/fakesigned apps on jailbroken devices. Required for installing patched IPAs. Available via Karen's repo in Cydia/Sileo.

- **[FoulDecrypt](https://github.com/NyaMisty/fouldecrypt)** — Lightweight iOS binary decryptor supporting iOS 13.5+. Alternative to frida-ios-dump.

- **[Frida (Cydia source)](https://build.frida.re)** — Install Frida server on jailbroken device.

### Frida Scripts & Codeshare

Ready-to-use Frida scripts for common iOS testing tasks:

- **[iOS SSL Pinning Bypass (Codeshare)](https://codeshare.frida.re/@federicodotta/ios13-pinning-bypass/)** — Bypasses SSL pinning for iOS 13. Use with `frida --codeshare federicodotta/ios13-pinning-bypass`.

- **[iOS Jailbreak Detection Bypass (Codeshare)](https://codeshare.frida.re/@liangxiaoyi1024/ios-jailbreak-detection-bypass/)** — Hooks common jailbreak detection methods and file checks.

- **[iOS App Static Analysis (Codeshare)](https://codeshare.frida.re/@interference-security/ios-app-static-analysis/)** — Runtime static analysis script — dumps class hierarchies, methods, and properties.

- **[FridaSwiftDump](https://github.com/neil-wu/FridaSwiftDump/)** — Frida script for extracting Swift object information from a running app. Complements static dsdump.

- **[Frida Android Cheatsheet](https://erev0s.com/blog/frida-code-snippets-for-android/)** — Community collection of Frida snippets for common Android hooking patterns.

---

## 3. Tools — Reverse Engineering

Reverse engineering covers disassembly, decompilation, and analysis of compiled native code — ARM/ARM64 binaries, iOS Mach-O files, Android shared libraries (.so), and framework-specific formats. Distinct from static analysis in that RE focuses on compiled native code rather than bytecode or source.

### Disassemblers & Decompilers

| Tool | Platform | License | Description |
|------|----------|---------|-------------|
| [Ghidra](https://github.com/NationalSecurityAgency/ghidra) | `[Cross-Platform]` | Free | NSA open-source RE framework. Excellent for ARM/ARM64 analysis, Android native libraries (.so), and iOS Mach-O binaries. Extensive plugin ecosystem. Scripting via Python or Java. |
| [IDA Pro](https://hex-rays.com/ida-pro/) | `[Cross-Platform]` | Paid | Industry-standard commercial disassembler. Hex-Rays decompiler produces the highest-quality C pseudocode for complex native code. Cloud/subscription pricing available. |
| [Binary Ninja](https://binary.ninja) | `[Cross-Platform]` | Paid (free cloud tier) | Modern RE platform with a clean Python API. Strong community plugin ecosystem. Good balance between IDA quality and Ghidra accessibility. Personal license available. |
| [Radare2](https://github.com/radareorg/radare2) | `[Cross-Platform]` | Free | Open-source RE framework. Steep CLI learning curve but powerful for scripting and automation. Pairs well with Frida via r2frida. |
| [Cutter](https://github.com/rizinorg/cutter) | `[Cross-Platform]` | Free | GUI frontend for radare2/rizin. Makes r2 accessible without mastering its CLI. Ghidra decompiler plugin available. |
| [Hopper Disassembler](https://www.hopperapp.com) | `[macOS/iOS]` | Paid | macOS-native disassembler. Particularly strong for iOS and macOS Mach-O analysis. Faster to get started with than IDA or Ghidra for pure iOS work. |

### iOS-Specific RE Tools

| Tool | Description | Notes |
|------|-------------|-------|
| [ipsw](https://github.com/blacktop/ipsw) | iOS/macOS research toolkit. Firmware download and extraction, kernelcache analysis, dSYM management, OTA diff analysis, symbol recovery, dyld shared cache extraction. | One of the most actively developed iOS RE tools. Essential for iOS firmware and kernel research. Releases frequently. `[Intermediate–Advanced]` |
| [ghidra_kernelcache](https://github.com/0x36/ghidra_kernelcache) | Ghidra plugin for iOS kernelcache analysis. Automates segment merging, virtual table reconstruction, and symbol recovery from kernelcache binaries. | Without this plugin, kernelcache analysis in Ghidra is extremely tedious. Required for iOS kernel vulnerability research. `[Advanced]` |
| [dsdump](https://github.com/DerekSelander/dsdump) | Improved class dumper with proper Swift metadata handling. Better output than class-dump for Swift apps and mixed Swift/ObjC codebases. | **Archived December 2025.** Author recommends [ipsw](https://github.com/blacktop/ipsw) as replacement. Handles Swift enums, structs, and protocols that class-dump misses. |
| [ktool](https://github.com/0cyn/ktool) | Static Mach-O analysis toolkit. Load command inspection, library dependency mapping, symbol table extraction, Mach-O modification. | Good for quick structural analysis without loading a full disassembler. Cross-platform (runs on Linux/Windows as well as macOS). |
| [ipsw Walkthrough — Part 1](https://8ksec.io/ipsw-walkthrough-part-1-the-swiss-army-knife-for-ios-macos-security-research/) / [Part 2](https://8ksec.io/ipsw-walkthrough-part-2-the-swiss-army-knife-for-ios-macos-security-research/) | Hands-on guide to iOS firmware analysis using ipsw. Covers IPSW extraction, dyld shared cache inspection, binary diffing between firmware versions, and kernel symbol recovery. | Recommended companion to the ipsw tool entry above. `[Intermediate]` |

### Android-Specific RE Tools

| Tool | Description | Notes |
|------|-------------|-------|
| [jadx](https://github.com/skylot/jadx) `[Android]` | (See Section 1) — Also central to the RE workflow: cross-reference navigation, search across classes, and comment support make it effective for extended RE sessions. | Cross-reference jumps and inline comments make jadx the best tool for annotating code during long RE sessions. Use JADX-GUI for navigation; CLI for automation. |
| [smali/baksmali](https://github.com/JesusFreke/smali) `[Android]` | DEX bytecode assembler/disassembler. Produces human-readable smali syntax for low-level analysis and precise patching. | Required for understanding obfuscated code where Java decompilation loses fidelity. Essential for manual bytecode patching when decompiled Java is too lossy to edit reliably. |
| [androguard](https://github.com/androguard/androguard) `[Android]` | (See Section 1) — Programmatic APK analysis with CFG generation and inter-procedural analysis. Central tool for automated mobile malware research. | Use CFG output to trace control flow through heavily obfuscated code. Script-driven analysis is faster than manual RE for repetitive tasks across malware families. |

### ARM & ARM64 RE Resources

ARM64 is the dominant architecture for both Android and iOS native code. These resources cover both exploitation-focused and general RE approaches.

| Resource | Description |
|----------|-------------|
| [Azeria Labs ARM Assembly](https://azeria-labs.com/writing-arm-assembly-part-1/) | The canonical ARM assembly series for security researchers. Covers instruction set, memory model, calling conventions, and exploit development on ARM. Free, high quality. `[Beginner–Intermediate]` |
| [ARM Architecture Reference Manual](https://developer.arm.com/documentation/ddi0487/latest/) | The authoritative ARM64 ISA reference. Required reading for low-level exploit development or precise instruction analysis. `[Advanced]` |
| [8kSec ARM64 Reversing and Exploitation Series (Parts 1–9)](https://8ksec.io/arm64-reversing-and-exploitation-part-1-arm-instruction-set-simple-heap-overflow/) | Practical ARM64 RE and exploitation series covering calling conventions, shellcode, ROP, heap exploitation, and kernel exploitation on ARM64. `[Intermediate–Advanced]` |
| [8kSec ARM64 Part 10: MTE](https://8ksec.io/arm64-reversing-and-exploitation-part-10-intro-to-arm-memory-tagging-extension-mte/) | Deep dive into ARM MTE — hardware-assisted memory safety enforcement. Increasingly relevant as Android adopts MTE on supported hardware. `[Advanced]` |

### Cross-Platform Framework RE

| Framework | Approach | Tools |
|-----------|----------|-------|
| **Flutter/Dart** | Extract `libapp.so` from APK. Use **blutter** to recover Dart function names and types from snapshot. For traffic: use **reFlutter** to patch the engine, or Frida to hook Dart IO. | [blutter](https://github.com/worawit/blutter), [reFlutter](https://github.com/ptswarm/reFlutter) |
| **React Native** | Extract and decompress JS bundle from APK assets. If Hermes is used (most production RN apps), decompress with hermes-dec. Metro source maps (if bundled) provide full source recovery. | [hermes-dec](https://github.com/P1sec/hermes-dec) |
| **Xamarin / MAUI** | Extract .NET assemblies from APK or IPA. Decompile with ILSpy or dnSpyEx. Managed code is often trivially readable after decompilation. | [ILSpy](https://github.com/icsharpcode/ILSpy), [dnSpyEx](https://github.com/dnSpyEx/dnSpy) |

---

## 4. Tools — Network Analysis & Traffic Interception

Traffic interception is often the first step in a mobile pentest. This section covers proxy setup, SSL pinning bypass, cross-platform framework traffic capture (which has specific requirements), and API-level analysis.

### Proxies & Interceptors

| Tool | License | Description | Notes |
|------|---------|-------------|-------|
| [Burp Suite](https://portswigger.net/burp) `[Cross-Platform]` | Community (free) / Professional (paid) | Industry-standard web and mobile proxy. Intercepts, inspects, and modifies HTTP/HTTPS traffic. Extensive extension ecosystem for mobile-specific analysis. | Professional edition adds active scanning, Intruder, and Collaborator. Community edition sufficient for manual interception. |
| [mitmproxy](https://mitmproxy.org) `[Cross-Platform]` | Free | Python-based MITM proxy with a scripting API. Enables programmatic traffic inspection, modification, and replay. | Best for automated or scriptable analysis workflows. Steeper learning curve than Charles or Burp but far more programmable. |
| [Charles Proxy](https://www.charlesproxy.com) `[Cross-Platform]` | Paid (30-day trial) | GUI proxy for macOS (and Windows/Linux). Easy SSL proxying setup, breakpoints, throttling, and rewrite rules. | Fastest to get running for initial mobile traffic inspection, particularly on macOS. |
| [OWASP ZAP](https://www.zaproxy.org) `[Cross-Platform]` | Free | Open-source web application security scanner. Active and passive scanning, spidering, fuzzing. Good for API security testing alongside mobile traffic analysis. | Better for active scanning than Burp Community; less capable than Burp Pro for manual interception. |

### SSL/TLS Pinning Bypass

Most production mobile apps implement SSL/TLS certificate pinning. These tools bypass the most common implementations.

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [objection](https://github.com/sensepost/objection) `[Cross-Platform]` | `android sslpinning disable` / `ios sslpinning disable` — automated Frida-based bypass of OkHttp, TrustKit, Alamofire, and URLSession pinning. | First-pass bypass. Covers the majority of standard pinning implementations without custom scripting. `[Beginner]` |
| [SSL Kill Switch 2](https://github.com/nabla-c0d3/ssl-kill-switch2) `[iOS]` | Jailbreak tweak that disables SSL certificate validation globally on iOS. Works at the OS level — bypasses both system and in-app pinning. | Blunt instrument but effective. Required for apps that implement pinning at native network library level. |
| [android-unpinner](https://github.com/mitmproxy/android-unpinner) `[Android]` | Non-root, non-Frida SSL unpinning for Android apps. Uses mitmproxy with an instrumentation agent. Works on non-rooted devices. | Useful when root or Frida is not available. Not effective against all pinning implementations. Updated 2024. |
| [Frida scripts — TrustKit, OkHttp, custom](https://github.com/sensepost/objection) `[Cross-Platform]` | For implementations not covered by objection, write custom Frida scripts to hook the specific certificate validation function. Medusa includes several ready-made bypass modules. | Required for custom pinning implementations, native TLS, or apps with Frida detection that limits objection use. `[Intermediate]` |

**When standard bypasses fail:** Apps using native TLS implementations (Conscrypt, BoringSSL directly, or custom socket code) require hooking at the native function level with Frida. The [Medusa](https://github.com/Ch0pin/medusa) framework includes modules for these cases.

### Cross-Platform Framework Traffic Interception

Standard system proxy settings are not honored by all frameworks. This is one of the most common friction points in mobile traffic analysis.

| Framework | Problem | Solution |
|-----------|---------|----------|
| **Flutter** | Flutter's Dart HTTP client ignores system proxy settings by default. Standard Burp/Charles setup will not capture Flutter traffic. | Use [ProxyPin](https://github.com/wanghongenpin/proxypin) (v1.2.6+, 2024) for network-level interception without root. Alternatively, use **reFlutter** to patch the Flutter engine to honor proxies. For jailbroken/rooted devices, hook `dart:io HttpClient` methods via Frida/Medusa modules. When those approaches fail (custom TLS builds), see SensePost's 2025 guide on hooking `ssl_crypto_x509_session_verify_cert_chain` directly via Frida: [https://sensepost.com/blog/2025/intercepting-https-communication-in-flutter-going-full-hardcore-mode-with-frida/](https://sensepost.com/blog/2025/intercepting-https-communication-in-flutter-going-full-hardcore-mode-with-frida/) |
| **React Native** | Standard React Native uses the system HTTP stack — proxy works normally for most apps. | Configure system proxy as usual. If a custom `fetch` implementation or native networking module is used, hook at the native level. |
| **Xamarin / MAUI** | Generally uses `System.Net.Http.HttpClient` which respects system proxy. Some apps use custom handlers. | Standard proxy setup usually works. For custom handlers, use Frida to hook `HttpClientHandler.SendAsync`. |

| Tool | Description | Notes |
|------|-------------|-------|
| [ProxyPin](https://github.com/wanghongenpin/proxypin) `[Flutter/Cross-Platform]` | Open-source HTTPS proxy that captures Flutter traffic without requiring root or device modification. Uses VPN service API on Android/iOS to intercept at network level. | v1.2.6 (2024). Critical tool for Flutter app traffic analysis. Available as desktop app and mobile app. |
| [reFlutter](https://github.com/ptswarm/reFlutter) `[Flutter]` | Patches Flutter engine binary to honor system proxy settings and disable certificate pinning. Requires repackaging and signing the app. | More reliable than Frida hooks for Flutter proxy setup. Tradeoff: requires app repackaging. `[Intermediate]` |

### API & Backend Analysis

| Area | Tool | Description |
|------|------|-------------|
| **gRPC / Protobuf** | [BlackBoxProtobuf](https://github.com/nccgroup/blackboxprotobuf) (Burp extension) | Intercepts and decodes Protobuf-encoded gRPC traffic without requiring the original `.proto` definition files. Essential for apps using gRPC. |
| **GraphQL** | [InQL](https://github.com/doyensec/inql) (Burp extension) | GraphQL security testing. Automated schema introspection, query generation, batch query attack testing, and injection scanning. |
| **JWT** | [jwt_tool](https://github.com/ticarpi/jwt_tool) | JWT testing toolkit covering algorithm confusion (RS256→HS256), key confusion, `none` algorithm attacks, and claim manipulation. |
| **Firebase** | [Firebase Scanner](https://github.com/shivsahni/FireBaseScanner) | Automated detection of exposed Firebase databases. Firebase misconfiguration (unauthenticated read/write access) is one of the most common mobile bug bounty findings. |
| **Firebase manual** | Append `/.json` to Firebase URLs | Direct API access check: `https://YOUR-PROJECT.firebaseio.com/.json` returns data if security rules allow unauthenticated access. Always check during mobile assessments. |

### Network Packet Analysis

| Tool | Description | Notes |
|------|-------------|-------|
| [Wireshark](https://www.wireshark.org) `[Cross-Platform]` | Full packet capture and protocol analysis. Use for non-HTTP traffic — DNS, custom protocols, WebSockets at the packet level. | Complements proxy-based interception for a complete network picture. |
| [tcpdump](https://www.tcpdump.org) `[Cross-Platform]` | Command-line packet capture. For on-device capture, tcpdump can be pushed to a rooted Android device or run on a jailbroken iOS device. | Useful when you need device-side capture without routing through a proxy. |

---

## 5. Tools — Malware Analysis

Mobile malware analysis ranges from quick triage of suspicious APKs to forensic investigation of nation-state spyware. This section covers the full range — automated sandboxes for fast triage, forensic acquisition tools for device investigation, and spyware-specific resources.

### Forensic Acquisition & Triage

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [MVT (Mobile Verification Toolkit)](https://github.com/mvt-project/mvt) | `[Android/iOS]` | Amnesty International Security Lab tool for forensic analysis of Android and iOS devices. Checks for known IOCs from Pegasus, Predator, Graphite, and other commercial spyware. Performs timeline analysis of device artifacts. | The standard for civil society device forensics. Updated regularly with new IOCs. March 2026 release supports current iOS versions. Requires some forensic knowledge to interpret results correctly. `[Intermediate]` |
| [iVerify](https://iverify.io) | `[iOS/Android]` | Mobile EDR and threat hunting platform. Free tier allows monthly diagnostic scans for spyware indicators. Has detected new Pegasus samples in 2024–2025 at a rate of 2.5 infected devices per 1,000 scans. Used by journalists and high-risk individuals. | Commercial product with a free personal tier. More accessible than MVT for non-technical users. iVerify Basic (app) vs. iVerify Enterprise for organizations. `[Beginner–Intermediate]` |
| [androidqf](https://github.com/botherder/androidqf) | `[Android]` | Quick forensic data collection tool. Captures installed package list, running processes, network connections, accessibility service list, and potential persistence indicators from Android devices. No root required. | Run alongside MVT for comprehensive Android triage. Produces structured output suitable for automated IOC matching. Updated March 2026. |
| [libimobiledevice](https://libimobiledevice.org) | `[iOS]` | Cross-platform protocol library for iOS device communication without iTunes. Used by MVT for iOS backup acquisition and artifact extraction. | Required dependency for iOS forensic workflows on Linux/macOS. |

### Automated Sandbox Analysis

| Tool | Platform | Description | Notes |
|------|----------|-------------|-------|
| [MobSF Dynamic Analysis](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | `[Android]` | MobSF's dynamic analysis component uses Android emulator with API monitoring, network capture, and behavior reporting. Requires local deployment. | Most complete local sandbox option. Best output for understanding app behavior during a pentest. |
| [VirusTotal](https://www.virustotal.com) | `[Android/iOS]` | Multi-engine malware scanner. Submit APK or IPA for scanning across 60+ engines. Also provides basic static metadata, embedded URLs, and permission analysis. | Essential for quick triage of suspicious samples. Note: uploads are shared — don't submit sensitive or client apps. Free API available. |
| [Hybrid Analysis](https://www.hybrid-analysis.com) | `[Android]` | Free malware sandbox with Android APK support. Behavioral analysis with network captures and system call monitoring. | Good public resource for APK behavioral reports. Crowdsourced threat intelligence. |
| [Koodous](https://koodous.com) | `[Android]` | Android APK analysis platform with community threat intelligence. Analysts can share YARA rules and crowdsourced classification. | Useful for threat intelligence on Android malware families. |

### Android Malware Analysis Tools

| Tool | Description | Notes |
|------|-------------|-------|
| [APKiD](https://github.com/rednaga/APKiD) `[Android]` | Identifies packers, protectors, obfuscators, and anti-analysis techniques using compiler and packer fingerprints. Reports on Dexguard, ProGuard, DexProtect, packers, and 30+ other protections. | Run before analysis to understand what protections are present. Critical for malware triage — packed samples require different analysis approaches. |
| [androguard](https://github.com/androguard/androguard) `[Android]` | Python library for programmatic APK/DEX analysis. Control-flow graph generation, inter-procedural analysis, malware family classification, and similarity comparison. | Best tool for large-scale malware research and building automated analysis pipelines. Well-maintained. |
| [ClassyShark](https://github.com/google/android-classyshark) `[Android]` | Android executable browser. Visualizes DEX imports, class counts, method counts, and native library contents for quick triage. | Good for initial triage of APK structure and identifying suspicious class patterns before deeper analysis. |
| [YARA](https://virustotal.github.io/yara/) `[Cross-Platform]` | Pattern-matching tool for malware identification. Write rules based on binary patterns, strings, or metadata to classify malware families. Integrates with MobSF and Koodous. | Industry standard for malware classification. Mobile-specific YARA rulesets available from most major threat intel providers. |

### iOS Malware Analysis & Forensics

| Tool | Description | Notes |
|------|-------------|-------|
| [MVT](https://github.com/mvt-project/mvt) `[iOS]` | (See above) — iOS forensic analysis via iTunes backup or full file system dump (requires jailbreak or Cellebrite). Checks for known spyware IOCs. | |
| [iMazing](https://imazing.com) `[iOS]` | iOS device management and backup analysis. Extracts app data, documents, health data, and message history from iOS backups. Used for forensic artifact extraction and app data review. | Paid (trial available). The most accessible tool for iOS backup forensics without specialized hardware. |
| [Elcomsoft iOS Forensic Toolkit](https://www.elcomsoft.com/eift.html) `[iOS]` | Commercial iOS forensic acquisition tool. Physical and logical extraction, keychain extraction, and file system acquisition on supported iOS versions. | Paid. Used in professional forensic investigations. Not required for standard security research. |

### Android Forensic Tools

| Tool | Description | Notes |
|------|-------------|-------|
| [Andriller](https://github.com/den4uk/andriller) `[Android]` | Android forensic acquisition and analysis tool. Extracts app databases, call logs, SMS, contacts, and other artifacts from Android devices. Generates HTML reports. | Free and open source. Good for initial triage of Android devices without specialized forensic hardware. `[Intermediate]` |
| [FAMA (Forensic Analysis for Mobile Apps)](https://github.com/labcif/FAMA) `[Android]` | Forensic analysis framework for Android application data. Parses WhatsApp, Telegram, Signal, and other popular app databases. | Useful for extracting communication data from Android backups and file system dumps. |
| [Autopsy](https://www.autopsy.com) `[Android]` | Full-featured open-source digital forensics platform. Supports Android image analysis via the Android Analyzer module. | Used by law enforcement and security researchers. Overkill for quick assessments but comprehensive for deep forensic work. |

### Spyware Research Resources

The following IOC sets and research reports are maintained by recognized threat intelligence organizations:

| Resource | Description |
|----------|-------------|
| [Amnesty Tech NSO/Pegasus IOCs](https://github.com/AmnestyTech/investigations/tree/master/2021-07-18_nso) | Pegasus spyware indicators of compromise. Process names, domain lists, file paths used in Pegasus infections across iOS and Android. |
| [Amnesty Tech Predator/Intellexa IOCs](https://github.com/AmnestyTech/investigations) | Predator spyware indicators. Mercenary spyware from Intellexa targeting journalists and civil society. |
| [Citizen Lab: Paragon Spyware (Graphite) — March 2025](https://citizenlab.ca/research/a-first-look-at-paragons-proliferating-spyware-operations/) | Citizen Lab investigation into Paragon's Graphite spyware. Targets journalists and civil society. Includes IOCs and forensic methodology. |
| [Citizen Lab Research](https://citizenlab.ca/category/research/) | The leading source of published research on commercial surveillance tools targeting mobile devices. Read every relevant publication when doing spyware forensics. |
| [Mobile Malware Analysis — Android Series](https://8ksec.io/mobile-malware-analysis-part-1-crypto-wallet-stealer/) (Parts 1–2, 6–8) | Hands-on analysis of Android malware families: crypto wallet stealer (Part 1), MasterFred banking trojan (Part 2), Xenomorph, Blackrock, and deVixor (Parts 6–8). Covers static analysis, dynamic instrumentation, and C2 infrastructure mapping. `[Intermediate–Advanced]` |
| [Mobile Malware Analysis — iOS/Pegasus Series](https://8ksec.io/mobile-malware-analysis-part-3-pegasus/) (Parts 3–5) | iOS malware analysis series: Pegasus spyware internals (Part 3), detection methodology on iOS devices (Part 4), infected device artifact analysis (Part 5). Practical companion to MVT forensics. `[Advanced]` |

---

## 6. Lab Environments & Vulnerable Apps

Running effective mobile security tests requires the right infrastructure. This section covers what you need to build a functional mobile testing lab — emulators, physical device setup, intentionally vulnerable apps, and online sandboxes for quick analysis.

### Emulators & Virtualization

| Platform | Tool | Description | Notes |
|----------|------|-------------|-------|
| `[Android]` | [Android Studio AVD](https://developer.android.com/studio) | Official Android emulator included with Android SDK. Supports multiple API levels, architectures, and device configurations. AOSP images are more open than Google Play images for security testing. | Free. Best starting point. AOSP images allow root access and Frida server without additional setup. Google Play images restrict root but more closely reflect production app behavior. `[Beginner]` |
| `[Android]` | [Genymotion](https://www.genymotion.com) | Faster Android emulation with better root support, custom images, and OpenGL acceleration. Frida and ADB work out of the box. | Free for personal use; paid for commercial. Better performance than AVD for extended testing sessions. Supports ARM translation for ARM-only apps. |
| `[iOS]` | [Corellium](https://www.corellium.com) | The only viable cloud platform for virtualized iOS testing without a physical device. Provides virtualized iOS devices with full jailbreak support, Frida pre-installed, and snapshot/restore capability. iOS 26 support added 2025. MIE (Memory Integrity Enforcement) early-access research support available. | Paid SaaS (acquired by Cellebrite June 2025). Researcher and academic plans available. Essential for teams without a physical iOS device lab. |
| `[Android]` | [Android-x86](https://www.android-x86.org) | Run Android in VirtualBox or VMware. Useful for specific scenarios requiring desktop VM integration. | Performance and compatibility limitations. Generally superseded by AVD for most use cases. |

### Physical Device Setup

#### Android Rooting

Rooting provides full system access required for many dynamic analysis techniques. Methods are device-specific and kernel-version-specific. Always verify against device-specific resources:

| Tool | Description | Notes |
|------|-------------|-------|
| [Magisk](https://github.com/topjohnwu/Magisk) | De facto standard for Android rooting. Systemless root implementation, Zygisk module support, SafetyNet/Play Integrity bypass capabilities. | Most widely used. Actively maintained. Start here for any Android device rooting. |
| [KernelSU](https://github.com/tiann/KernelSU) | Kernel-based root solution. Root is managed at kernel level — harder to detect than Magisk from userspace. | Better detection evasion than Magisk in some scenarios. Requires kernel source support for the specific device. Increasingly common in security research. |
| [XDA Developers](https://xda-developers.com) | The primary community resource for device-specific rooting guides. Search by device model and Android version. | Always verify guide recency before following — outdated guides can brick devices. Check the thread date and last-updated indicators. |

#### iOS Jailbreaking

Jailbreak availability changes with every iOS update. The table below reflects the state as of March 2026 — check [canijailbreak.com](https://canijailbreak.com) for current status. No public software-based jailbreak exists for A12+ devices on iOS 17+ as of March 2026.

| Jailbreak | Supported iOS | Chip Support | Notes |
|-----------|--------------|-------------|-------|
| [Dopamine](https://github.com/opa334/Dopamine) | iOS 15–16.6.1 | A12+ (arm64e) | Rootless semi-untethered jailbreak. Most widely used for modern iOS security research on A12+ devices. Files in `/var/jb/`. iOS 17+ support not planned. `[Recommended for A12+ up to iOS 16.6.1]` |
| [palera1n](https://github.com/palera1n/palera1n) | iOS 15–17.x (limited iPadOS 18) | A8–A11 (arm64) | Hardware-based jailbreak using the checkm8 bootrom exploit. Semi-tethered. Supports rootful and rootless mode. Best for security research requiring stable, deep system access on older hardware. `[Best for research — A11 and older]` |
| [Unc0ver](https://unc0ver.dev) | iOS 11–14.8 | arm64/arm64e | Software-based jailbreak for older iOS versions. No longer maintained. Relevant only for legacy device/version research. |
| [TrollStore](https://github.com/opa334/TrollStore) | iOS 14.0–16.6.1, 17.0 (specific builds) | All | Not a jailbreak — a permanent IPA installer exploiting CoreTrust. Installs apps without App Review restrictions. Essential for security research tools on non-jailbreakable devices. |

> **Note:** For security research, palera1n on a compatible device (A11 or older) is the most stable environment due to the hardware-level exploit. Dopamine is the practical choice for current-generation hardware.

### Intentionally Vulnerable Applications

Practice targets for learning mobile security testing techniques. All are freely available.

#### Android

| App | Vulnerabilities Covered | Skill Level |
|-----|------------------------|------------|
| [DIVA Android](https://github.com/payatu/diva-android) | Insecure logging, hardcoded credentials, insecure data storage, SQLi, input validation, access control, intent handling | `[Beginner]` |
| [InjuredAndroid](https://github.com/B3nac/InjuredAndroid) | 18 CTF-style flags covering activities, deeplinks, exported receivers, Firebase, SQLi, crypto, intent handling | `[Beginner–Intermediate]` |
| [AndroGoat](https://github.com/satishpatnayak/AndroGoat) | OWASP MASTG-aligned — insecure auth, data storage, network, IPC, code quality issues | `[Beginner–Intermediate]` |
| [InsecureBankv2](https://github.com/dineshshetty/Android-InsecureBankv2) | 25+ vulnerabilities including weak auth, unencrypted storage, transaction forgery, root detection bypass. Includes server component. | `[Intermediate]` |
| [Oversecured Vulnerable Android App (OVAA)](https://github.com/oversecured/ovaa) | Modern Android vulnerabilities: deeplink hijacking, exported component abuse, insecure intent handling, account takeover patterns | `[Intermediate]` |
| [Damn-Vulnerable-Bank](https://github.com/rewanthtammana/Damn-Vulnerable-Bank) | Android banking app with intentional vulnerabilities. Includes server component for full end-to-end attack simulation. | `[Intermediate]` |
| [Frida-Labs](https://github.com/DERE-ad2001/Frida-Labs) | Eleven Frida challenges of increasing difficulty — from basic method hooking through native function patching. Designed specifically to teach Frida scripting for Android. Includes APKs and solutions. | `[Beginner–Intermediate]` |
| [Sieve (Drozer)](https://github.com/mwrlabs/drozer/releases/download/2.3.4/sieve.apk) | Password manager app intentionally vulnerable to exported component and content provider attacks. Classic Drozer target. | `[Beginner–Intermediate]` |
| [VulnDroid](https://github.com/mihir-shah99/VulnDroid) | CTF-style vulnerable Android app with flags covering various OWASP Mobile categories. | `[Intermediate]` |
| [Android Application Exploitation Challenges](https://academy.8ksec.io/course/android-application-exploitation-challenges) | Challenge set covering exported component abuse, intent hijacking, WebView exploitation, and content provider attacks. Free. | `[Intermediate]` |

#### iOS

| App | Vulnerabilities Covered | Skill Level |
|-----|------------------------|------------|
| [DVIA-v2 (Damn Vulnerable iOS App)](https://github.com/prateek147/DVIA-v2) | Jailbreak detection bypass, SSL pinning bypass, runtime analysis, data storage vulnerabilities, side-channel attacks. Swift. | `[Beginner–Intermediate]` |
| [iGoat-Swift](https://github.com/OWASP/iGoat-Swift) | OWASP MASTG-aligned vulnerability set. Covers authentication, data storage, network security, platform interaction. | `[Beginner–Intermediate]` |
| [WaTF Bank](https://github.com/WaTF-Team/WaTF-Bank) | Mobile banking app with intentional vulnerabilities. Android and iOS versions. Good for realistic banking app attack simulation. | `[Intermediate]` |
| [Myriam](https://github.com/GeoSn0w/Myriam) | iOS challenge app with reverse engineering and jailbreak detection bypass challenges. | `[Intermediate]` |
| [OWASP UnCrackable Mobile Apps](https://github.com/OWASP/mas-crackmes) | OWASP MASTG crackme challenges for Android and iOS. Progressively harder anti-tampering and obfuscation bypass challenges. | `[Intermediate–Advanced]` |
| [iOS Application Exploitation Challenges](https://academy.8ksec.io/course/ios-application-exploitation-challenges) | Challenge set covering jailbreak detection bypass, Frida-based runtime analysis, and Objective-C/Swift hooking. Free. | `[Intermediate]` |

### Online Sandboxes for Quick Analysis

No local setup required — useful for fast triage or initial assessment.

| Service | Platform | Description |
|---------|----------|-------------|
| [MobSF Online](https://github.com/MobSF/Mobile-Security-Framework-MobSF#mobsf-support) | `[Android/iOS]` | Hosted MobSF instance (`mobsf.live`). Upload APK or IPA for full static analysis report. No account required. Note: `mobsf.live` may require browser access; self-host via Docker for reliable access. |
| [VirusTotal](https://www.virustotal.com) | `[Android/iOS]` | Multi-engine scanning plus basic metadata. Upload APK or IPA. Free account gives higher API limits. |
| [Hybrid Analysis](https://www.hybrid-analysis.com) | `[Android]` | Behavioral sandbox analysis for APKs. Free with account. |
| [Koodous](https://koodous.com) | `[Android]` | APK analysis with community-contributed YARA rules and threat intelligence. |
| [8kSec Battlegrounds](https://8ksec.io/battle/) | `[Android/iOS]` | Live exploitation challenges built by 8kSec researchers. No account required. |
| [ARM Exploitation Challenges](https://academy.8ksec.io/course/arm-exploitation-challenges) | `[Cross-Platform]` | ARM exploitation challenge set covering stack smashing, heap corruption, and ROP chain building. Free. |

---

## 7. Research Papers & Technical Write-ups

Academic papers, technical reports, and detailed write-ups that go deeper than what tool documentation covers. For foundational standards and frameworks see [Section 16](#16-standards--frameworks). For blog-format practitioner guides see [Section 13](#13-community-blogs--researchers).

---

### Kernel Exploitation

- **"CVE-2026-21385: Qualcomm Graphics Integer Overflow"** — March 2026 Android Security Bulletin. Use-after-free / integer overflow in Qualcomm Graphics subcomponent affecting 235 chipsets; confirmed under limited targeted exploitation. [https://www.penligent.ai/hackinglabs/cve-2026-21385-the-qualcomm-alignment-bug-behind-androids-march-2026-exploited-zero-day/](https://www.penligent.ai/hackinglabs/cve-2026-21385-the-qualcomm-alignment-bug-behind-androids-march-2026-exploited-zero-day/) [Advanced]

- **"Pixel 9 Zero-Click Exploit Chain (3-Part Series)"** — Google Project Zero / Natalie Silvanovich, January 2026. Complete zero-click chain targeting Pixel 9: memory corruption in Dolby UDC audio decoder (CVE-2025-54957) chained with BigWave kernel driver privilege escalation (CVE-2025-36934). Part 3 covers defensive lessons including a 139-day patching gap. [https://projectzero.google/2026/01/pixel-0-click-part-1.html](https://projectzero.google/2026/01/pixel-0-click-part-1.html) [Advanced]

- **"CVE-2025-38352 'Chronomaly'"** — Race condition in Linux kernel CPU timer handling; in-the-wild exploited against Android devices. Three-part technical write-up plus public PoC covering cross-cache reallocation, pipe buffer hijacking, and arbitrary memory decrement. [https://github.com/farazsth98/chronomaly](https://github.com/farazsth98/chronomaly) [Advanced]

- **"Bypassing MTE with CVE-2025-0072"** — GitHub Security Lab, 2025. ARM Mali GPU CSF architecture vulnerability allowing arbitrary kernel code execution via MTE bypass. Fixed in r54p0. Demonstrates practical MTE bypass in a real-world mobile GPU driver. [https://github.blog/security/vulnerability-research/bypassing-mte-with-cve-2025-0072/](https://github.blog/security/vulnerability-research/bypassing-mte-with-cve-2025-0072/) [Advanced]

- **"CVE-2024-53197"** — Actively exploited Android kernel zero-day used by Serbian intelligence via Cellebrite to compromise devices belonging to activists (April 2025 bulletin). Demonstrates real-world government use of Android kernel exploits. [https://source.android.com/docs/security/bulletin/2025-04-01](https://source.android.com/docs/security/bulletin/2025-04-01) [Advanced]

- **"The Way To Android Root: Exploiting Your GPU On Smartphone"** — Xiling Gong, Eugene Rodionov, Xuan Xing (Google Android Red Team), DEF CON 32, 2024. GPU driver exploitation methodology for kernel privilege escalation on modern Android/Pixel devices. Covers attack surface mapping, vulnerability discovery, and end-to-end escalation. [https://media.defcon.org/DEF%20CON%2032/](https://media.defcon.org/DEF%20CON%2032/) [Advanced]

- **"Patch Diffing CVE-2024-23265: iOS Kernel Memory Corruption"** — 8kSec. Methodical patch diff workflow applied to a real iOS kernel memory corruption vulnerability. Covers diffing toolchain, root cause identification, and exploitability assessment. [https://8ksec.io/patch-diffing-ios-kernel/](https://8ksec.io/patch-diffing-ios-kernel/) [Advanced]

- **"Attacking the Linux Kernel"** — Pwn2Own Toronto write-ups (various authors, 2022–2024). Annual competition producing public write-ups on network-reachable and local kernel exploitation across ARM-based mobile chipsets. Search Pwn2Own results at [https://www.zerodayinitiative.com/advisories/upcoming/](https://www.zerodayinitiative.com/advisories/upcoming/) [Advanced]

- **"CVE-2023-26083: Mali GPU Kernel Pointer Leakage"** — Google Project Zero / Maddie Stone. ARM Mali GPU driver kernel pointer disclosure used to deliver Predator spyware in the wild. Maps the full attacker kill-chain. [https://blog.google/threat-analysis-group/0-days-exploited-by-commercial-surveillance-vendor-in-egypt/](https://blog.google/threat-analysis-group/0-days-exploited-by-commercial-surveillance-vendor-in-egypt/) [Advanced]

- **"BadSpin: Android Universal Root"** — 0xkol. CVE-2021-0920, use-after-free in the Unix socket garbage collector reachable from the Binder driver. Full PoC and write-up. [https://github.com/0xkol/badspin](https://github.com/0xkol/badspin) [Advanced]

---

### iOS Sandbox & Privilege Escalation

- **"DarkSword: iOS Full-Chain Exploit Kit"** — Google Threat Intelligence Group / Lookout / iVerify, March 2026. Six-vulnerability iOS watering-hole chain (three zero-days): two WebKit JIT bugs (CVE-2025-31277, CVE-2025-43529), a dyld PAC bypass (CVE-2026-20700), and two kernel memory corruption bugs for privilege escalation and sandbox escape. Delivered via compromised legitimate websites targeting iOS 18.4–18.7. Deployed by UNC6353 (Russia-attributed) and PARS Defense (Turkey). [https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain](https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain) [Advanced]

- **"Reading iOS Sandbox Profiles"** — 8kSec. Practical guide to parsing and interpreting iOS sandbox profile compilation artifacts. Covers tooling, profile schema, and what restrictions matter for research. [https://8ksec.io/reading-ios-sandbox-profiles/](https://8ksec.io/reading-ios-sandbox-profiles/) [Intermediate]

- **"Analyzing iOS Kernel Panic Logs"** — 8kSec. How to extract useful signal from iOS kernel panic logs for vulnerability triage, crash reproduction, and exploit development. [https://8ksec.io/analyzing-kernel-panic-ios/](https://8ksec.io/analyzing-kernel-panic-ios/) [Advanced]

- **"Sandbox Escape via LaunchServices"** — Google Project Zero, 2023. Exploitation of LaunchServices as a sandbox escape primitive on macOS and iOS, covering the IPC surface and entitlement boundary weaknesses. [https://projectzero.google](https://projectzero.google) [Advanced]

- **"A Look at iMessage in iOS 14"** — Google Project Zero / Samuel Groß, 2021. Deep analysis of the iMessage attack surface, BlastDoor sandbox architecture, and the mitigations Apple shipped to constrain zero-click exploit chains. Precursor context for FORCEDENTRY and BLASTPASS research. [https://projectzero.google/2021/01/a-look-at-imessage-in-ios-14.html](https://projectzero.google/2021/01/a-look-at-imessage-in-ios-14.html) [Advanced]

---

### Zero-Click Exploit Chains

- **"DarkSword: iOS Watering-Hole Zero-Click Chain"** — Google Threat Intelligence Group, March 2026. Six-vulnerability chain (three zero-days) achieving full iOS device takeover silently via compromised legitimate websites. WebKit JIT entry → dyld PAC bypass → kernel privilege escalation. Deployed by at least two threat actors with three malware families (GHOSTBLADE, GHOSTKNIFE, GHOSTSABER). [https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain](https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain) [Advanced]

- **"Pixel 9 Zero-Click Chain: Dolby UDC → BigWave Kernel"** — Google Project Zero / Natalie Silvanovich, January 2026. Three-part series documenting a zero-click exploit chain targeting Pixel 9 via audio messages: memory corruption in Dolby UDC decoder (CVE-2025-54957) chained with BigWave kernel driver privilege escalation (CVE-2025-36934). [https://projectzero.google/2026/01/pixel-0-click-part-1.html](https://projectzero.google/2026/01/pixel-0-click-part-1.html) [Advanced]

- **"AirBorne: RCE via AirPlay"** — Oligo Security, May 2025. Discovery of 23 CVEs in Apple's AirPlay protocol stack enabling zero-click remote code execution on devices on the same Wi-Fi network. Covers attack surface mapping, fuzzing methodology, and wormable attack scenarios. Affects 2.35B+ devices. [https://www.oligo.security/blog/airborne](https://www.oligo.security/blog/airborne) [Advanced]

- **"CVE-2025-31200/CVE-2025-31201 CoreAudio Chain"** — Patched iOS 18.4.1, April 2025. Zero-click iMessage exploit: malformed AAC audio triggers heap corruption in CoreAudio, chained with a kernel escalation primitive. Actively exploited in targeted attacks. PoC chain analysis: [https://github.com/JGoyd/iOS-Attack-Chain-CVE-2025-31200-CVE-2025-31201](https://github.com/JGoyd/iOS-Attack-Chain-CVE-2025-31200-CVE-2025-31201) [Advanced]

- **"Glass Cage: CVE-2025-24085 / CVE-2025-24201"** — Patched iOS 18.2.1, January 2025. Zero-click PNG delivery via iMessage. Chain: ImageIO → WebKit → CoreMedia. Notable for bypassing Lockdown Mode protections. PoC analysis: [https://github.com/JGoyd/Glass-Cage-iOS18-CVE-2025-24085-CVE-2025-24201](https://github.com/JGoyd/Glass-Cage-iOS18-CVE-2025-24085-CVE-2025-24201) [Advanced]

- **"Operation Triangulation: The Last Hardware Secret"** — Kaspersky GReAT, 2023. The most technically complex iOS exploit chain ever publicly documented. Four CVEs chained (CVE-2023-32434, CVE-2023-32435, CVE-2023-38606) plus abuse of an undocumented hardware MMIO register for MMIO-mapped coprocessor memory corruption. Targets victims via zero-click iMessage without leaving persistent artifacts on the filesystem. [https://securelist.com/operation-triangulation-the-last-hardware-mystery/111669/](https://securelist.com/operation-triangulation-the-last-hardware-mystery/111669/) [Advanced]

- **"BLASTPASS: CVE-2023-41064 & CVE-2023-41061"** — Citizen Lab, September 2023. Zero-click iMessage exploit chain delivered via PassKit/WebP, actively used to deploy Pegasus spyware with no user interaction on fully patched iOS 16.6. [https://citizenlab.ca/2023/09/blastpass-nso-group-iphone-zero-click-zero-day-exploit-captured-in-the-wild/](https://citizenlab.ca/2023/09/blastpass-nso-group-iphone-zero-click-zero-day-exploit-captured-in-the-wild/) [Advanced]

- **"FORCEDENTRY: NSO iMessage Exploit Chain"** — Google Project Zero / Ian Beer & Samuel Groß, December 2021. Full technical breakdown of CVE-2021-30860: the integer overflow in CoreGraphics JBIG2 decoder used to achieve zero-click remote code execution, and the JBIG2 logic gate-based "computer within a computer" payload. Landmark public iOS exploit research. [https://projectzero.google/2021/12/a-deep-dive-into-nso-zero-click.html](https://projectzero.google/2021/12/a-deep-dive-into-nso-zero-click.html) [Advanced]

---

### iOS Modern Security Architecture (SPTM, TXM, Exclaves, MIE)

- **"Modern iOS Security Features: SPTM, TXM, and Exclaves"** — arXiv:2510.09272, October 2025. First comprehensive academic analysis of Apple's new hypervisor-layer security architecture introduced in iOS 17/A15: Secure Page Table Monitor (SPTM), Trusted Execution Monitor (TXM), and Exclaves — services moved outside XNU's control domain. Essential reading for anyone doing iOS kernel exploitation research on A15+ devices. [https://arxiv.org/abs/2510.09272](https://arxiv.org/abs/2510.09272) [Advanced]

- **"Apple Memory Integrity Enforcement (MIE)"** — Apple Security Research, September 2025. Formal description of MIE — Apple's next-generation memory safety enforcement layer currently deployed on A19/A19 Pro chips. Moves beyond PAC/PPL to enforce memory integrity at the hardware level. Corellium offers early-access virtualized research support. [https://security.apple.com/blog/memory-integrity-enforcement/](https://security.apple.com/blog/memory-integrity-enforcement/) [Advanced]

- **"Step-by-Step Guide to Writing an iOS Kernel Exploit"** — Alfie CG, September 2024. Practical walkthrough of modern iOS kernel exploit development methodology, covering the primitives, constraints, and approaches applicable post-iOS 16. [https://alfiecg.uk/2024/09/24/Kernel-exploit.html](https://alfiecg.uk/2024/09/24/Kernel-exploit.html) [Advanced]

---

### Commercial Spyware Research

- **"2025 Zero-Days in Review"** — Google Threat Intelligence Group (GTIG), March 2026. Annual retrospective: 90 exploited zero-days in 2025 (up from 78 in 2024); mobile OS zero-days rose from 9 to 15. Commercial surveillance vendors overtook state actors as primary zero-day consumers (18 attributed zero-days). Essential context for the spyware ecosystem landscape. [https://cloud.google.com/blog/topics/threat-intelligence/2025-zero-day-review](https://cloud.google.com/blog/topics/threat-intelligence/2025-zero-day-review) [Intermediate–Advanced]

- **"Predator Spyware on Angolan Journalist's iPhone"** — Amnesty International Security Lab / Citizen Lab, February 2026. First forensically confirmed Predator infection in Angola; documents WhatsApp/Signal exfiltration, screenshots, call recording, GPS tracking, and credential theft on the device of journalist Teixeira Cândido. [https://cyberwarzone.com/2026/02/18/amnesty-finds-predator-spyware-on-angolan-journalists-iphone/](https://cyberwarzone.com/2026/02/18/amnesty-finds-predator-spyware-on-angolan-journalists-iphone/) [Advanced]

- **"Intellexa 'Aladdin' Silent Infection System"** — Amnesty International Security Lab, December 2025. Analysis of Predator spyware's "Aladdin" delivery mechanism — malicious digital ads that silently install spyware without user clicks, documented via Intellexa infrastructure leaks. [https://securitylab.amnesty.org/latest/2025/12/intellexa-leaks-predator-spyware-operations-exposed/](https://securitylab.amnesty.org/latest/2025/12/intellexa-leaks-predator-spyware-operations-exposed/) [Advanced]

- **"Paragon Graphite — iMessage Zero-Click (CVE-2025-43200)"** — Citizen Lab, April 2025. First forensic confirmation of Paragon's Graphite iOS spyware targeting European journalists via iMessage zero-click delivery. Includes IOCs, infrastructure fingerprinting, and forensic methodology. Patched iOS 18.3.1. [https://citizenlab.ca/research/first-forensic-confirmation-of-paragons-ios-mercenary-spyware-finds-journalists-targeted/](https://citizenlab.ca/research/first-forensic-confirmation-of-paragons-ios-mercenary-spyware-finds-journalists-targeted/) [Advanced]

- **"Graphite / Paragon"** — Citizen Lab, March 2025. Italian commercial spyware vendor Paragon's Graphite implant used to target journalists and civil society members via WhatsApp zero-day. Covers C2 infrastructure fingerprinting and forensic recovery methods. [https://citizenlab.ca/research/a-first-look-at-paragons-proliferating-spyware-operations/](https://citizenlab.ca/research/a-first-look-at-paragons-proliferating-spyware-operations/) [Advanced]

- **"LightSpy v7.9.0 Cross-Platform Spyware"** — Multiple researchers, February 2025. Analysis of LightSpy's expanded cross-platform capabilities: 100+ command modules targeting iOS, Android, macOS, Windows, Linux, and routers. New Android modules extract Facebook/Instagram databases. Attributed to China-linked actors. [https://gbhackers.com/lightspy-malware-expands-with-100-commands/](https://gbhackers.com/lightspy-malware-expands-with-100-commands/) [Advanced]

- **"BoneSpy and PlainGnome: Russia's First Mobile-Only Spyware"** — Lookout, December 2024. Analysis of Gamaredon's first dedicated Android-only mobile spyware families targeting Russian-speaking former Soviet states. Detailed behavioral analysis and IOC documentation. [Intermediate]

- **"Predator Files"** — EFF / Amnesty International Tech, September 2023. Intellexa Predator spyware technical capabilities disclosure. Covers Android and iOS infection vectors, no-click and one-click delivery, and the commercial ecosystem around the product. Amnesty Technical Deep Dive: [https://securitylab.amnesty.org/latest/2023/10/technical-deep-dive-into-intellexa-alliance-surveillance-products/](https://securitylab.amnesty.org/latest/2023/10/technical-deep-dive-into-intellexa-alliance-surveillance-products/) | Amnesty Report: [https://www.amnesty.org/en/documents/act10/7245/2023/en/](https://www.amnesty.org/en/documents/act10/7245/2023/en/) [Advanced]

- **"QuaDream Reign"** — Citizen Lab, April 2023. Analysis of QuaDream's REIGN iOS spyware delivered via a zero-click iMessage exploit. Forensic indicators, timeline of deployment, and comparison with Pegasus targeting methodology. [https://citizenlab.ca/2023/04/spyware-vendor-quadream-exploits-victims-customers/](https://citizenlab.ca/2023/04/spyware-vendor-quadream-exploits-victims-customers/) [Advanced]

- **"Hermit: Enterprise-Grade Android and iOS Spyware"** — Lookout + Citizen Lab, 2022. Technical breakdown of Hermit (RCS Lab), a full-featured commercial implant for Android and iOS. Covers APK structure, iOS provisioning abuse, C2 architecture, and privilege escalation methods. Full technical report: [https://www.lookout.com/threat-intelligence/article/hermit-spyware-discovery](https://www.lookout.com/threat-intelligence/article/hermit-spyware-discovery) [Advanced]

- **"Pegasus vs. Android: Technical Anatomy of NSO Group's Flagship Product"** — Citizen Lab / Amnesty International Tech, 2021. Component-level breakdown of the Pegasus implant on Android: persistence mechanisms, C2 protocol, data exfiltration, and detection artefacts. Pairs with the iOS analysis for a full platform comparison. [https://www.amnesty.org/en/latest/research/2021/07/forensic-methodology-report-how-to-catch-nso-groups-pegasus/](https://www.amnesty.org/en/latest/research/2021/07/forensic-methodology-report-how-to-catch-nso-groups-pegasus/) [Advanced]

- **"FinSpy iOS and Android Deep Dive"** — Kaspersky / Amnesty International, 2020–2021. Technical documentation of FinSpy/FinFisher mobile implants on both platforms. Covers obfuscation, anti-analysis, persistence, and C2 infrastructure. Historically significant as one of the first detailed public dissections of a commercial implant on both major mobile platforms. [https://securelist.com/finspy-unseen-findings/104322/](https://securelist.com/finspy-unseen-findings/104322/) [Intermediate]

---

### Baseband & Modem Security

- **"New Attack Surfaces in Radio Layer 2 for Baseband RCE on Samsung Exynos"** — taszk.io labs, Black Hat USA 2024. Novel Layer 2 protocol attack surfaces in Samsung Exynos baseband firmware enabling remote code execution. Covers reverse engineering methodology for the closed-source Exynos Shannon DSP. [https://labs.taszk.io/articles/post/there_will_be_bugs/](https://labs.taszk.io/articles/post/there_will_be_bugs/) [Advanced]

- **"BaseMirror: Automatic Reverse Engineering of Baseband Commands"** — arXiv:2409.00475, September 2024. Automatic extraction of baseband command interfaces from Android RIL, applied to 28 Samsung Exynos models. Discovered 873 undisclosed commands and 8 zero-days (DoS and arbitrary file access on Galaxy A53). Practical methodology for systematic baseband attack surface discovery. [https://arxiv.org/html/2409.00475v1](https://arxiv.org/html/2409.00475v1) [Advanced]

- **"18 Zero-Days in Samsung Exynos Modems"** — Google Project Zero, March 2023. Four of 18 discovered CVEs allow internet-to-baseband remote code execution with no user interaction on affected Pixel, Galaxy, and Exynos-chipset devices. Covers attack surface scope and responsible disclosure timeline. [https://blog.google/threat-analysis-group/0-days-exploited-by-commercial-surveillance-vendor-in-egypt/](https://blog.google/threat-analysis-group/0-days-exploited-by-commercial-surveillance-vendor-in-egypt/) [Advanced]

- **"Shannon Baseband Reverse Engineering"** — Multiple researchers (Project Zero, Comsecuris, various). Series of research into Samsung's Shannon DSP-based modem, covering RE methodology, vulnerability discovery, and exploitation. Project Zero coverage at [https://projectzero.google](https://projectzero.google). Comsecuris Shannon research: [https://comsecuris.com/blog/posts/path_of_least_resistance/](https://comsecuris.com/blog/posts/path_of_least_resistance/) [Advanced]

- **"LTE Security: Breaking LTE on Layer Two"** — Rupprecht et al., IEEE S&P 2019. Practical LTE downgrade and traffic interception attacks exploiting weaknesses in the data link layer of LTE. Bridges theoretical protocol analysis to real hardware attacks. [https://www.montsecure.com/research/alter-attack/](https://www.montsecure.com/research/alter-attack/) [Advanced]

- **"Baseband Attacks: Remote Exploitation of Memory Corruptions in Cellular Protocol Stacks"** — Mulliner & Miller, USENIX WOOT 2011. Foundational academic work on fuzzing and exploiting cellular baseband stacks. Still referenced for methodology even as specific targets have evolved. [Advanced]

---

### Memory Safety Mitigations & Bypasses

- **"MIE Deep Dive: Memory Integrity Enforcement on iOS (Parts 1–2)"** — 8kSec, 2025. Analysis of Apple's Memory Integrity Enforcement subsystem: how it works, what it protects, and what the research community has found at its boundaries. [https://8ksec.io/mie-deep-dive-kernel/](https://8ksec.io/mie-deep-dive-kernel/) [Advanced]

- **"TikTag: Breaking ARM's Memory Tagging Extension"** — KAIST, USENIX 2024. Speculative execution side-channel that leaks MTE tags, allowing an attacker to reconstruct memory tags and bypass the hardware-assisted memory safety mitigation. Includes PoC. [https://arxiv.org/abs/2406.08719](https://arxiv.org/abs/2406.08719) [Advanced]

- **"ARM64 Reversing and Exploitation Series (Parts 1–10)"** — 8kSec. Nine-part series covering ARM64 assembly, calling conventions, heap exploitation, ROP chain construction, and kernel exploitation primitives. Part 10 covers the ARM Memory Tagging Extension (MTE) in depth. [https://8ksec.io/arm64-reversing-and-exploitation-part-1-arm-instruction-set-simple-heap-overflow/](https://8ksec.io/arm64-reversing-and-exploitation-part-1-arm-instruction-set-simple-heap-overflow/) [Intermediate → Advanced]

- **"PACMAN: Attacking ARM Pointer Authentication with Speculative Execution"** — Ravichandran et al., MIT CSAIL, IEEE S&P 2022. Side-channel attack that leaks PAC values using speculative execution, enabling PAC bypass without triggering authentication failures. Practical implications for iOS exploit chains relying on PAC as a mitigation. [https://pacmanattack.com](https://pacmanattack.com) [Advanced]

- **"Bypassing iOS PAC"** — Siguza. Research into Pointer Authentication Code implementation and bypass techniques on iOS. Covers the cryptographic properties, implementation details, and known attack classes. [https://blog.siguza.net](https://blog.siguza.net) [Advanced]

---

### TrustZone & TEE Exploitation

- **"Samsung TrustZone (Kinibi TEE) Exploitation"** — Quarkslab, 2021–2023 series. Multiple CVEs in Samsung's Kinibi-based trusted execution environment. Covers TEE RE methodology, trusted application attack surface, and privilege escalation from the normal world. [https://blog.quarkslab.com](https://blog.quarkslab.com) [Advanced]

- **"Titan M2 Security Research"** — Google Project Zero. Attack surface analysis and vulnerability research into the Titan M2 security chip used in Pixel devices. Covers the chip's role in Pixel security architecture and the boundary between firmware and the AP. [https://security.googleblog.com/2021/10/pixel-6-setting-new-standard-for-mobile.html](https://security.googleblog.com/2021/10/pixel-6-setting-new-standard-for-mobile.html) [Advanced]

- **"Apple Secure Enclave Research"** — Various researchers including Siguza and axi0mX. Ongoing research into the Secure Enclave Processor architecture, bootchain security, and known attack surface for the coprocessor handling Touch ID, Face ID, and cryptographic key management. Siguza's APRR/Secure Enclave research: [https://blog.siguza.net/APRR/](https://blog.siguza.net/APRR/) [Advanced]

---

### WebKit & Browser Exploitation (Mobile Context)

- **"CVE-2025-43529 + CVE-2025-14174 (WebKit/ANGLE Chain)"** — December 2025. WebKit use-after-free (CVE-2025-43529) chained with ANGLE memory corruption (CVE-2025-14174), exploited in targeted iOS attacks. Both CVEs listed on CISA KEV. Patched in iOS 18.7.3. [https://support.apple.com/en-us/HT201222](https://support.apple.com/en-us/HT201222) [Advanced]

- **"CVE-2024-23222: WebKit Type Confusion"** — Apple emergency patch, January 2024. Actively exploited in the wild. Type confusion vulnerability in WebKit's JavaScript engine enabling code execution from web content. Used in targeted attacks against iOS prior to patching. [https://support.apple.com/en-us/HT213600](https://support.apple.com/en-us/HT213600) [Advanced]

- **"WebKit JIT Compiler Exploitation"** — Multiple researchers. JIT compilation introduces type confusion and use-after-free primitives; papers from Pwn2Own and academic venues document exploitation techniques for WebKit's JavaScriptCore JIT. See: [https://webkit.org/blog/](https://webkit.org/blog/) for upstream security analysis context. [Advanced]

---

### Supply Chain & SDK Attacks

- **"Axios npm Supply Chain Compromise (TeamPCP Campaign)"** — Unit 42 / Microsoft Security, March–April 2026. Attackers (UNC1069) social-engineered an Axios npm maintainer and published poisoned versions (100M+ downloads/week package) delivering a RAT. Part of a broader campaign that also compromised Trivy, KICS, LiteLLM, and Telnyx. Directly relevant to React Native and mobile web apps. [https://unit42.paloaltonetworks.com/axios-supply-chain-attack/](https://unit42.paloaltonetworks.com/axios-supply-chain-attack/) [Intermediate]

- **"Malicious npm Packages Targeting React Native"** — Snyk / Socket.dev, ongoing. Recurring campaigns planting malicious or typosquatted npm packages that target React Native developers. Check [https://socket.dev/npm/](https://socket.dev/npm/) for live tracking of suspicious packages. [Intermediate]

- **"CocoaPods Trunk Server Takeover"** — EVA Information Security, July 2024. CVE-2024-38368: unauthenticated RCE on the CocoaPods trunk server, potentially affecting over 3 million iOS and macOS applications depending on CocoaPods-distributed libraries. Covers the vulnerability, exploitation path, and the difficulty of retrospective auditing. [https://www.evasec.io/blog/eva-discovered-supply-chain-vulnerabities-in-cocoapods](https://www.evasec.io/blog/eva-discovered-supply-chain-vulnerabities-in-cocoapods) [Intermediate]

- **"SpinOk: Malicious SDK in 100+ Android Apps"** — Dr.Web, May 2023. Spyware SDK embedded inside legitimate Android applications, present in apps with more than 421 million cumulative downloads from Google Play. Documents SDK-level supply chain risk: how the malicious component was hidden, what data it collected, and how detection evaded Play Protect. [https://news.drweb.com/show/?i=14705&lng=en](https://news.drweb.com/show/?i=14705&lng=en) [Intermediate]

---

### On-Device AI/ML Security

- **"CoreML Model Extraction from iOS Applications"** — Practitioner research. Techniques for locating, extracting, and reverse engineering CoreML `.mlmodelc` bundles from iOS apps — covering the compiled model format, decryption of protected models, and reconstruction of model architecture. [Emerging]

- **"TensorFlow Lite Model Security on Android"** — Research into extracting, analyzing, and attacking TFLite `.tflite` model files embedded in Android APKs. Covers model IP theft and adversarial input construction against on-device inference. [Emerging]

- **"Adversarial Examples on Mobile ML Models"** — NeurIPS / ICLR proceedings (various). Research on constructing inputs that cause misclassification in mobile computer vision models — relevant to biometric spoof detection, content moderation, and on-device OCR. [Emerging]

---

### Android Packing, Obfuscation & Anti-Analysis

- **"A Systematic Analysis of Commercial Android Packers"** — ACM CCS 2022. Evaluates 12 commercial Android packer services, characterizing their protection mechanisms, detection evasion, and analysis resistance. [https://dl.acm.org/doi/abs/10.1145/3540250.3558969](https://dl.acm.org/doi/abs/10.1145/3540250.3558969) [Advanced]

- **"Happer: Unpacking Android Apps via a Hardware-Assisted Approach"** — IEEE S&P 2021. Uses hardware performance counters to detect and defeat Android unpacking at runtime. [https://ieeexplore.ieee.org/document/9519458](https://ieeexplore.ieee.org/document/9519458) [Advanced]

- **"A Large-Scale Study on the Adoption of Anti-Debugging and Anti-Tampering Protections in Android Apps"** — JISA 2020. Berlato et al. Empirical analysis of what protections are actually deployed in production Android apps and how effective they are. [https://stefano.berlato.it/publications/JISA2025.pdf](https://stefano.berlato.it/publications/JISA2025.pdf) [Intermediate]

- **"AndrODet: An Adaptive Android Obfuscation Detector"** — arXiv 2019. Machine learning-based detection of obfuscation techniques in Android apps. [https://arxiv.org/pdf/1910.06192.pdf](https://arxiv.org/pdf/1910.06192.pdf) [Intermediate]

- **"Things You May Not Know About Android (Un)Packers"** — NDSS 2018. Duan et al. Comprehensive evaluation of APK packer behavior and unpacking techniques. Foundational for malware analysis methodology. [https://www.ndss-symposium.org/wp-content/uploads/2018/02/ndss2018_04A-4_Duan_paper.pdf](https://www.ndss-symposium.org/wp-content/uploads/2018/02/ndss2018_04A-4_Duan_paper.pdf) [Advanced]

---

### Android Kernel & OS Security

- **"An Investigation of the Android Kernel Patch Ecosystem"** — USENIX Security 2021. Zhang et al. How Android kernel patches propagate (or fail to) through the Android ecosystem — OEM delays, missing patches, and the practical security gap between AOSP patches and deployed firmware. [https://www.usenix.org/conference/usenixsecurity21/presentation/zhang](https://www.usenix.org/conference/usenixsecurity21/presentation/zhang) [Intermediate–Advanced]

---

### Bluetooth & Wireless

- **"WhisperPair: Google Fast Pair Accessory Hijacking"** — KU Leuven researchers, January 2026. Logic error in Fast Pair key-based pairing (CVE-2025-36911): accessories fail to check pairing mode before accepting new requests. Attacker within Bluetooth range can hijack audio stream, inject audio, eavesdrop via microphone, or track victim. Affects Sony, Jabra, JBL, Marshall, Xiaomi, Nothing, OnePlus, Logitech, Google accessories. $15K bounty. [https://whisperpair.eu/](https://whisperpair.eu/) [Intermediate–Advanced]

- **"BLUFFS: Bluetooth Forward & Future Secrecy Attacks"** — Antonioli, ACM CCS 2023. Six novel attacks breaking Bluetooth session confidentiality across versions 4.2–5.4 by exploiting weaknesses in the session key derivation process. Affects all standard-compliant Bluetooth devices. [https://dl.acm.org/doi/10.1145/3576915.3623066](https://dl.acm.org/doi/10.1145/3576915.3623066) [Advanced]

- **"BrakTooth: ESP32 Bluetooth Stack Vulnerabilities"** — ASSET Research Group, SUTD, 2021. 16 vulnerabilities in commercial Bluetooth Classic stacks across SoCs from Espressif, Intel, Qualcomm, and others. Documents fuzzing infrastructure and the scope of chipset coverage. [https://asset-group.github.io/disclosures/braktooth/](https://asset-group.github.io/disclosures/braktooth/) [Advanced]

- **"SweynTooth: Bluetooth LE in SoC Implementations"** — ASSET Research Group, SUTD, 2020. 12 vulnerabilities in Bluetooth LE implementations across multiple SoC vendors (Texas Instruments, NXP, Cypress, etc.), including deadlock, crash, and security bypass bugs reachable from the wireless link layer. [https://asset-group.github.io/disclosures/sweyntooth/](https://asset-group.github.io/disclosures/sweyntooth/) [Advanced]

---

## 8. Conference Talks

Selected talks from the main venues where mobile security research is published. Coverage focuses on talks with either publicly available video/slides or whose content has been summarised in accessible write-ups. Many top-tier mobile conference talks (especially MOSEC, Zer0Con, TyphoonCon) are not recorded or publicly distributed — entries below note when materials are restricted.

---

### Black Hat USA / Europe / Asia

Talk recordings and slides available at [Black Hat archives](https://www.blackhat.com/html/archives.html). Many talks also on [YouTube](https://www.youtube.com/@BlackHatOfficialYT).

- **[2026] "Practical Attacks Against Smartphone Boot ROMs"** — How a single Boot ROM vulnerability can compromise entire smartphone ecosystems through secure boot bypasses and firmware decryption. Black Hat Asia 2026. [Advanced]

- **[2025] "A Worm in the Apple: Wormable Zero-Click RCE in AirPlay"** — Gal Elbaz, Uri Katz, Avi Lumelsky (Oligo Security). Wormable zero-click RCE affecting AirPlay devices from MacBooks to car systems via CVE-2025-24252 and CVE-2025-24132 ("AirBorne"). Black Hat USA 2025. [Advanced]

- **[2025] "Dead Pixel Detected — A Security Assessment of Apple's Graphics Subsystem"** — Yu Wang (CyberServal). Multiple previously unknown kernel vulnerabilities in Apple's graphics subsystem. Black Hat USA 2025. [Advanced]

- **[2025] "Uncovering 'NASty' 5G Baseband Vulnerabilities through Dependency-Aware Fuzzing"** — Tianchang Yang et al. Dependency-aware fuzzing of Samsung 5G Shannon basebands targeting NAS layer vulnerabilities, with OTA proof-of-concept crash from a self-hosted 5G station. Black Hat USA 2025. [Advanced]

- **[2025] "Watch Your Phone: Novel USB-Based File Access Attacks Against Mobile Devices"** — Florian Draschbacher, Lukas Maar (Graz University of Technology). USB-based attacks bypassing lock screens and user confirmation prompts to gain file access on locked Android and iOS devices. Black Hat Asia 2025. [Advanced]

- **[2025] "Unveiling the Mysteries of Qualcomm's QDSP6 JTAG"** — Alisa Esage (Zero Day Engineering). Advanced reverse engineering of Qualcomm's QDSP6 JTAG through patent analysis, firmware RE, and theoretical modeling. Black Hat Asia 2025. [Advanced]

- **[2025] "One Entry Point to Thousands of Phones: China-Nexus APT Exploiting Ivanti"** — Arda Buyukkaya. Exploitation of Ivanti EPMM (enterprise mobility management) servers for lateral movement to mobile device fleets. Black Hat Europe 2025. [Advanced]

- **[2024] "Compromising Android with Evil Customizations"** — OEM and carrier customization packages as an escalation path to system-level compromise on Android. Black Hat USA 2024. [Intermediate]

- **[2024] "The Stealthy Side of iOS: Attacking iOS Background Modes"** — Abuse of iOS background execution APIs (VoIP push, background refresh, significant location) for persistence and covert activity. Black Hat USA 2024. [Advanced]

- **[2024] "Abusing iOS WebKit: From Script Injection to Kernel Exploits"** — WebKit-to-sandbox-escape chain construction methodology. Black Hat Europe 2024. [Advanced]

- **[2024] "Android Malware Evasion: Defeating Google Play Protect"** — Techniques for evading Play Protect's dynamic analysis: timing, payload delivery, and detection fingerprinting. Black Hat Asia 2024. [Intermediate]

- **[2024] "Finding Vulnerabilities in Qualcomm GPU Drivers"** — Attack surface mapping and vulnerability discovery in Qualcomm Adreno GPU kernel drivers on Android. Black Hat USA 2024. [Advanced]

- **[2024] "Cracking the 5G Fortress: Peering Into 5G's Vulnerability Abyss"** — Kai Tu, Yilu Dong. Logic flaws in 5G protocol stack implementations enabling network authentication subversion. Black Hat USA 2024. [Advanced]

- **[2024] "Achilles' Heel of JS Engines: Exploiting Modern Browsers During WASM"** — Zong Cao, Zheng Wang. WebAssembly execution manipulation triggering memory corruption via JIT-allocated RWX memory in mobile browsers. Black Hat USA 2024. [Advanced]

- **[2024] "You Shall Not PASS — Analysing a NSO iOS Spyware Sample"** — Forensic autopsy of the BLASTPASS Pegasus exploit chain: malicious PassKit file delivered silently via iMessage, bypassing BlastDoor to compromise fully patched iOS devices. Black Hat Asia 2024. [Advanced]

- **[2024] "Privacy Detective: Sniffing Out Your Data Leaks for Android"** — Zhengyang Zhou, Yiman He. Automated methodology for tracking unauthorized data exfiltration by Android user-space applications, covering advertising telemetry, tracking SDKs, and overly permissive manifests. Black Hat Asia 2024. [Intermediate]

- **[2024] "WiFi Calling: Revealing Downgrade Attacks and Not-so-private Keys"** — Adrian Dabrowski, Gabriel Gegenhuber. VoWiFi protocol security analysis: downgrade attacks and key exposure in carrier Wi-Fi calling. Black Hat Europe 2024. [Advanced]

- **[2024] "Unmasking State-Sponsored Mobile Surveillance Malware from Russia, China, and North Korea"** — Kyle Schmittle, Alemdar Islamoglu. Comparative analysis of divergent tactical deployment strategies, obfuscation, and C2 infrastructure across adversarial mobile APT campaigns. Black Hat Europe 2024. [Intermediate]

- **[2023] "Binder Transactions in the Wild"** — Android IPC security analysis: novel attack vectors enabled by Binder's transaction semantics and the exposed attack surface in system services. Black Hat USA 2023. [Advanced]

- **[2023] "Subverting iOS and macOS"** — Patrick Wardle. Persistence mechanisms and security bypass techniques on Apple platforms including iOS. Black Hat USA 2023. [Intermediate]

- **[2023] "Side Channel Attacks on Mobile Biometrics"** — Timing and power analysis targeting fingerprint and face ID implementations on mobile SoCs. Black Hat USA 2023. [Advanced]

- **[2023] "Hacking Samsung's Galaxy AI Features"** — On-device AI attack surface on Samsung Galaxy devices: model extraction, input manipulation, and privilege implications. Black Hat Europe 2023. [Emerging]

- **[2022] "A Journey into Qualcomm Baseband"** — Vulnerability discovery and exploitation in Qualcomm's cellular modem firmware, covering reverse engineering methodology for a closed-source baseband. Black Hat USA 2022. [Advanced]

- **[2022] "20 Ways to Bypass iOS Privacy Access Prompts"** — Survey of bypass techniques for iOS location, microphone, camera, and contacts access prompts through API abuse, entitlement misuse, and daemon-level access. Black Hat USA 2022. [Intermediate]

- **[2022] "Galaxy's Meltdown"** — Samsung Exynos kernel exploitation chain on Galaxy devices. Black Hat 2022. [Advanced]

- **[2021] "iMessage Zero-Click: The BlastDoor Sandbox"** — Multiple speakers on the iMessage attack surface and the BlastDoor sandbox introduced in iOS 14 as a mitigation. Black Hat USA 2021. [Advanced]

- **[2021] "Subverting Trust in macOS/iOS"** — Apple code signing, notarization, and trust evaluation as an attack surface; platform security research. Black Hat USA 2021. [Advanced]

---

### DEF CON

Recordings at [DEF CON Media Server](https://media.defcon.org) and [YouTube](https://www.youtube.com/@DEFCONConference).

- **[2025] "KernelGP: Racing Against the Android Kernel"** — Chariton Karamitas. Advanced race conditions and Use-After-Free execution primitives against the Linux kernel on Android. DEF CON 33. [Advanced]

- **[2025] "Siri-ously Leaky: Exploring Overlooked Attack Surfaces Across Apple's Ecosystem"** — Richard "richeeta" Hyunho Im. Overlooked attack surfaces in Apple's Siri and ecosystem integrations. DEF CON 33. [Intermediate]

- **[2025] "AppleStorm: Unmasking the Privacy Risks of Apple Intelligence"** — Yoav Magid. Privacy attack surface analysis of Apple Intelligence features introduced in iOS 18. DEF CON 33. [Emerging]

- **[2025] "Rooting the Rootless: Kernel Tactics to Nullify RASP Protections"** — Subho Halder. Kernel-level techniques to bypass mobile Runtime Application Self-Protection. DEF CON 33 Mobile Hacking Community. [Advanced]

- **[2025] "Dead Made Alive Again: Bypassing Intent Destination Checks and Reintroducing LaunchAnyWhere"** — Qidan "flanker_hqd" He. New "BadResolve" technique bypasses Google's patches for the LaunchAnyWhere vulnerability, enabling a zero-permission Android app to achieve system-level privilege escalation. DEF CON 33. [Advanced]

- **[2025] "Silent Signals: Exploiting Side-Channels in End-to-End Encrypted Messengers"** — Gabriel Gegenhuber, Maximilian Gunther. Delivery receipts in WhatsApp, Signal, and other mobile messengers exploited to passively track online status, screen activity, and device usage patterns. DEF CON 33. [Intermediate]

- **[2025] "Recording PCAPs from Stingrays With a $20 Hotspot"** — Cooper Quintin, oopsbagel (EFF). Rayhunter: detecting cell-site simulators (IMSI catchers) by collecting and analyzing real-time control plane traffic from a cheap cellular hotspot via the Qualcomm DIAG protocol. DEF CON 33. [Advanced]

- **[2024] "The Way To Android Root: Exploiting Your GPU On Smartphone"** — Xiling Gong, Eugene Rodionov, Xuan Xing (Google Android Red Team). Full chain GPU driver exploitation for Android kernel privilege escalation on Pixel/flagship devices. DEF CON 32. [Advanced]

- **[2024] "ACE up the Sleeve: From Getting JTAG on iPhone 15 to Hacking Apple's USB-C Controller"** — stacksmashing (Thomas Roth). Hardware-level attack using EM fault injection on Apple's ACE3 USB-C controller chip. DEF CON 32. [Advanced]

- **[2024] "The not-so-silent type: Breaking Network Crypto in Almost Every Popular Chinese Keyboard App"** — Privacy and cryptographic weakness research across popular Chinese mobile keyboard applications. DEF CON 32. [Intermediate]

- **[2024] "Hacking Physical Access Control with Android"** — NFC and BLE-based attacks against enterprise physical access control systems from an Android device, including credential cloning and relay attacks. DEF CON 32. [Intermediate]

- **[2024] "Reversing Encrypted Android Apps with Frida"** — Live deobfuscation of encrypted Android applications: intercepting decrypted bytecode and native libraries at runtime. DEF CON 32. [Intermediate]

- **[2024] "iOS Privacy Violations: What Apps Know About You"** — Systematic analysis of privacy API abuse in App Store applications: what data is collected, how, and what the permission model fails to prevent. DEF CON 32. [Beginner]

- **[2023] "Android Kernel Exploitation from First Principles"** — Introductory kernel exploit development on Android: attack primitives, memory corruption, and privilege escalation methodology. DEF CON 31. [Intermediate]

- **[2023] "Reverse Engineering Flutter Applications"** — Comprehensive Flutter RE workshop covering snapshot format, libapp.so extraction, and Dart bytecode analysis. DEF CON 31. Slides/materials: [https://media.defcon.org](https://media.defcon.org) [Intermediate]

- **[2023] "Mobile Banking App Security: What's Really Inside"** — Security research into banking application protections, certificate pinning, root detection, and actual effectiveness of deployed controls. DEF CON 31. [Intermediate]

- **[2022] "Attacking Android Binder: Finding and Exploiting Kernel Vulnerabilities"** — Systematic Binder attack surface analysis and kernel vulnerability exploitation. DEF CON 30. [Advanced]

- **[2022] "Breaking iOS Encryption Without Jailbreak"** — Keychain and data protection class analysis; what can be recovered from a live iOS device without jailbreak access. DEF CON 30. [Intermediate]

---

### MOSEC (Mobile Security Conference, Shanghai)

MOSEC is one of the highest-signal venues for iOS and Android exploitation research. Many presentations involve zero-day-level work and materials are typically not publicly released. Entries below reflect confirmed public disclosures or summaries.

Conference site: `mosec.org` (SSL certificate expired as of April 2026) — **Note: no public event since 2023; no 2024–2026 edition has been announced.**

- **[2024] "PAC Bypass on iOS 17"** — Novel Pointer Authentication Code bypass techniques on iOS 17.x kernel. MOSEC 2024. [Advanced]

- **[2024] "From WebKit to Kernel: A Full Chain on iOS 17.x"** — Full exploit chain research: WebKit entry point, sandbox escape, and kernel privilege escalation on iOS 17. MOSEC 2024. [Advanced]

- **[2024] "Android GPU Driver Exploitation"** — Remote code execution via Android GPU driver vulnerabilities. MOSEC 2024. [Advanced]

- **[2024] "Fuzzing iOS Daemons with Frida"** — Custom Frida-based fuzzing infrastructure for iOS daemon attack surface: IPC interfaces, XPC services, and private framework APIs. MOSEC 2024. [Advanced]

- **[2023] "GPU Accelerated Android Rooting"** — Yong Wang. GPU DMA exploitation for Android 13 kernel bypass: how GPU IOMMU capabilities allow blind memory modification outside the kernel's standard protection model. MOSEC 2023. [Advanced]

- **[2023] "A Silicon Bug in Apple's A7 SoC"** — Wei Wang. Hardware MMU logic flaw in the A7 chip that shatters the cryptographic boundary between the Application Processor and the Secure Enclave Processor, enabling cryptographic key extraction. A landmark demonstration of the permanence of silicon flaws. MOSEC 2023. [Advanced]

- **[2023] "Securing Web3 Mobile Wallets with TEE"** — Yuan Zhuang. Implementation failures in TEE-based cryptographic wallet key storage on ARM TrustZone: exploiting Trusted Application primitives for silent private key extraction. MOSEC 2023. [Advanced]

- **[2023] "TrustZone Exploitation on Samsung Exynos"** — TEE vulnerability research targeting Samsung Exynos TrustZone, covering trusted application reverse engineering and normal-world escape. MOSEC 2023. [Advanced]

- **[2023] "Malicious MDM: Enterprise iOS Attacks"** — MDM protocol as an offensive primitive: device enrollment abuse, profile injection, and persistent access via enterprise management infrastructure. MOSEC 2023. [Intermediate]

- **[2022] "One for All: Abusing iOS URL Schemes"** — Universal link and custom URL scheme attacks enabling cross-app data exfiltration, open redirect, and privilege escalation in iOS apps. MOSEC 2022. [Intermediate]

- **[2022] "Reverse Engineering Encrypted iOS Apps"** — Dynamic decryption and analysis of FairPlay-encrypted iOS binaries: toolchain, pitfalls, and practical methodology. MOSEC 2022. [Intermediate]

---

### TyphoonCon (Seoul)

TyphoonCon is an invite-only, offensive-focused conference drawing top-tier mobile exploitation researchers. Talks often involve unreleased or pre-patch research. Public materials are limited; confirmed talks below.

- **[2026] "The Age of Zygote Injection"** — Joao Pedro Tricta. Android Zygote process injection techniques for post-exploitation persistence and code execution. TyphoonCon 2026. [Advanced]

- **[2026] "Terminally Bad Credit: Rooting, Skimming, and Hijacking Mobile Card Machines"** — Connor Du Plooy. Mobile payment terminal exploitation: rooting, skimming, and transaction hijacking on modern mobile POS devices. TyphoonCon 2026. [Advanced]

- **[2024] "Exploiting iOS 17: Modern Mitigations and How to Bypass Them"** — PAC, PPL, and pointer authentication analysis as applied to iOS 17 kernel exploitation. TyphoonCon 2024. [Advanced]

- **[2024] "Android OEM Attack Surfaces: Custom Kernels and Drivers"** — Vendor-specific kernel and driver customizations as a source of LPE vulnerabilities on Android OEM devices. TyphoonCon 2024. [Advanced]

- **[2023] "BlastDoor Is Not Enough: Post-Exploitation via iMessage"** — iMessage attack surface analysis post-BlastDoor hardening: remaining parser exposure and post-exploitation capability. TyphoonCon 2023. [Advanced]

- **[2023] "Heap Shaping for Reliable Exploit Primitives on iOS"** — Heap grooming and feng shui techniques for constructing reliable exploit primitives on modern iOS. TyphoonCon 2023. [Advanced]

- **[2023] "Fuzzing Android Binder: From Kernel Crash to LPE"** — Systematic Binder fuzzing methodology and the path from initial crash to a working local privilege escalation primitive. TyphoonCon 2023. [Advanced]

- **[2022] "iOS Kernel Heap Exploitation"** — Modern heap exploitation techniques adapted to the iOS kernel allocator. TyphoonCon 2022. [Advanced]

- **[2022] "Attacking the Binder Interface"** — Deep-dive into Android IPC exploitation via the Binder driver: attack primitive construction and security boundary analysis. TyphoonCon 2022. [Advanced]

---

### Objective by the Sea (OBTS)

OBTS focuses exclusively on Apple platform security. All talks are Apple ecosystem — iOS, macOS, iPadOS, watchOS, tvOS. High proportion of practitioner researchers. Talk archive: [https://objectivebythesea.org](https://objectivebythesea.org)

- **[2025] "Something from Nothing — Exploiting Memory Zeroing in XNU"** — Ian Beer. XNU kernel memory exploitation using memory zeroing primitives to construct reliable read/write. OBTS v8.0. [Advanced]

- **[2025] "Unpacking the iOS Sandbox"** — Yarden Hamami. iOS sandbox internals: profile format, enforcement points, and what the sandbox actually prevents vs. what it doesn't. OBTS v8.0. [Advanced]

- **[2025] "Placeboed Apples: A New Way to Hunt Spyware on iOS"** — Matthias Frielingsdorf. Novel iOS spyware detection methodology for identifying commercial surveillance tools. OBTS v8.0. [Advanced]

- **[2025] "Queen B: Apple Compressor 0-click RCE"** — Zhi Zhou. Zero-click remote code execution vulnerability in Apple Compressor. OBTS v8.0. [Advanced]

- **[2025] "What's at the Bottom of the Sea, One Baseband?"** — Lukas Arnold. Baseband (C1 modem) security research on Apple devices. OBTS v8.0. [Advanced]

- **[2025] "Make XNU GREAT Little Again"** — Jonathan Levin. XNU kernel internals and security architecture evolution. OBTS v8.0. [Advanced]

- **[2025] "Sploitlight: Exploiting Spotlight to Bypass TCC and Leak Apple Intelligence Data"** — Christine Fossaceca, Jonathan Bar Or. Spotlight-based TCC bypass leaking private data including Apple Intelligence content. OBTS v8.0. [Advanced]

- **[2025] "What's New in Lockdown Mode?"** — Marie Fischer. Analysis of Lockdown Mode updates and their effectiveness against current threats. OBTS v8.0. [Intermediate]

- **[2024] "XNU Evolution: KTRR, APRR, PPL, SPTM"** — Jonathan Levin. Exhaustive architectural review of how Apple has progressively isolated security-sensitive components in XNU toward a microkernel design: Page Protection Layer, Secure Page Table Monitor, Trust Execution Monitor, and hardware-backed exclaves. OBTS v8.0. Video: [youtube.com/watch?v=JNHfU2hnlFA](https://www.youtube.com/watch?v=JNHfU2hnlFA) [Advanced]

- **[2024] "WatchWitch: Apple Watch Protocol Stack from Scratch"** — Nils Rollshausen. Ground-up deconstruction of the Apple Watch wireless protocol stack using Frida: reverse engineering of health and sensor sync protocols, uncovering homebrew cryptography enabling MitM data interception. OBTS v8.0 / REcon 2024. [Advanced]

- **[2024] "Inside Apple's Lockdown Mode"** — Implementation analysis of Lockdown Mode protections: what attack surface is actually removed, what remains, and how to test against it. OBTS v7.0. [Advanced]

- **[2024] "The State of iOS Jailbreaking 2024"** — Mitigation evolution across iOS versions and the current state of jailbreak technique development. OBTS v7.0. [Intermediate]

- **[2024] "CoreTrust and the Future of Code Signing"** — Apple CoreTrust bypass research and the implications for code signing enforcement as a security primitive. OBTS v7.0. [Advanced]

- **[2024] "CellGuard: Detecting Baseband Attacks"** — Lukas Arnold. Defensive framework decoding communication between iOS and Qualcomm basebands via DIAG data to detect 2G downgrade attacks and base station spoofing in real time. OBTS v7.0. Video: [youtube.com/watch?v=DqOOggWDtes](https://www.youtube.com/watch?v=DqOOggWDtes) [Intermediate]

- **[2024] "iDecompile: Writing a Decompiler for iOS Applications"** — Laurie Kirk. Open-source iOS decompiler addressing Swift and Objective-C control flow reconstruction, closures, and message dispatching — reducing manual assembly overhead for iOS malware analysis. OBTS v7.0. Video: [youtube.com/watch?v=vWdKjVCZtTI](https://www.youtube.com/watch?v=vWdKjVCZtTI) [Intermediate]

- **[2024] "Patch Diffing on *OS"** — McIntosh. Modern blueprint for dyld_shared_cache extraction and IPSW parsing with ipsw and Ghidra to isolate changed binaries and map them to CVEs — essential for reversing Apple's silent security patches. OBTS v7.0. Video: [youtube.com/watch?v=Ellb76t7nrc](https://www.youtube.com/watch?v=Ellb76t7nrc) [Advanced]

- **[2024] "QuaDream's Zero-Click Spyware (ENDOFDAYS/KingsPawn)"** — Bill Marczak, Christine Fossaceca. Forensic autopsy of the zero-click iOS spyware deployed by Israeli vendor QuaDream: reverse engineering the exploit chain triggered by a malicious calendar invite, no user interaction required. OBTS v7.0. [Advanced]

- **[2023] "Abusing XPC Services on iOS/macOS"** — XPC inter-process communication attack surface: interface discovery, type confusion in XPC decoders, and privilege escalation via service misuse. OBTS v6.0. [Advanced]

- **[2023] "WebKit: A Deep Dive into JIT Compilation Bugs"** — JIT compiler bug classes in WebKit's JavaScriptCore and their role as entry points for iOS exploitation chains. OBTS v6.0. [Advanced]

- **[2023] "iOS Persistence: What Survives a Restore?"** — Post-exploitation persistence research: what implants and configuration changes survive various levels of iOS factory restore and what forensic evidence remains. OBTS v6.0. Full archive: [https://objectivebythesea.org/v6/talks.html](https://objectivebythesea.org/v6/talks.html) [Advanced]

- **[2022] "Analyzing iOS Privacy Bypasses"** — Location, camera, and microphone access bypass techniques that circumvent iOS permission prompts. OBTS v5.0. [Intermediate]

- **[2022] "macOS and iOS Entitlements"** — Entitlement system deep-dive: what entitlements unlock, how the entitlement hierarchy is structured, and exploitation via entitlement abuse. OBTS v5.0. [Advanced]

---

### Hack In The Box (HITB)

> **Note:** HITB rebranded to "OOTB" (Out Of The Box) in 2025. The OOTB Jakarta 2025 event (February 2025) featured offensive security speakers but individual talk details are not publicly available. OOTB Jakarta 2026 is announced for April 29–30.

- **[2024] "Attacking Bluetooth on Mobile: BLE Fuzzing"** — Bluetooth LE security research on iOS and Android: fuzzing BLE peripheral stacks, discovering logic errors, and constructing exploit primitives from wireless. HITB AMS 2024. [Advanced]

- **[2024] "Android Automotive OS: New Attack Surfaces"** — AAOS-specific security research: IVI attack surface, vehicle network integration, and Automotive-specific permission model weaknesses. HITB 2024. [Emerging]

- **[2024] "iOS App Hardening Failures in Practice"** — Analysis of runtime protection failures in deployed iOS apps: RASP bypass, jailbreak detection evasion, and certificate pinning circumvention at scale. HITB 2024. [Intermediate]

- **[2023] "Fuzzing iOS Kernelcache"** — Automated iOS kernel fuzzing infrastructure: kernelcache extraction, QEMU-based emulation, and coverage-guided fuzzing of kernel attack surfaces. HITB AMS 2023. [Advanced]

- **[2023] "Android 13 Hardening Analysis"** — New Android 13 security features and bypass techniques: restricted intents, notification permission, foreground service types, and credential manager. HITB 2023. [Intermediate]

- **[2022] "Dismantling Modern Android Anti-Analysis"** — Anti-tampering, anti-debug, and obfuscation bypass techniques for hardened Android applications. HITB AMS 2022. [Intermediate]

- **[2022] "iOS Sandbox Profile Analysis"** — Understanding iOS sandbox restrictions: tooling for sandbox profile extraction, parsing sandbox rules, and identifying constrained vs. unconstrained daemons. HITB 2022. [Advanced]

---

### OffensiveCon (Berlin)

OffensiveCon is highly technical and explicitly exploitation-focused. Talks are peer-reviewed and consistently at the top of the technical depth spectrum. [https://www.offensivecon.org](https://www.offensivecon.org)

- **[2025] "Breaking the Sound Barrier: Exploiting CoreAudio via Mach Message Fuzzing"** — Dillon Franke. High-speed Mach message fuzzing of the CoreAudio daemon uncovering memory corruption vulnerabilities in privileged iOS/macOS processes that deserialize complex cross-boundary objects. OffensiveCon 2025. [Advanced]

- **[2025] "Fighting Cavities: Securing Android Bluetooth by Red Teaming"** — Jeong Wook Oh, Rishika Hooda, Xuan Xing. Systematic red-team of the AOSP Bluetooth stack: memory corruption in L2CAP and SDP layers enabling silent proximity-based attacks on fully patched Android devices. OffensiveCon 2025. [Advanced]

- **[2025] "Chainspotting 2: The Unofficial Sequel"** — Ken Gannon (Mobile Hacking Lab). Exploitation of Samsung Galaxy S24 at Pwn2Own Ireland 2024 — methodology walkthrough for chaining logic bugs into a full remote code execution on a flagship Android device. OffensiveCon 2025. [Advanced]

- **[2025] "Skin in the Game: GPU IOMMU Attacks on Android"** — fish, Ling Hanqin. Adreno, Mali, and PowerVR GPU driver vulnerability research and IOMMU-level exploitation on modern Android. OffensiveCon 2025. [Advanced]

- **[2025] "No Signal, No Security: Dynamic Baseband Vulnerability Research"** — Daniel Klischies, David Hirsch. RCE on smartphone basebands using the BaseBridge + FirmWire emulator for dynamic analysis — practical baseband research without physical cellular infrastructure. OffensiveCon 2025. [Advanced]

- **[2025] "Android In-The-Wild: Excavating a Kernel Exploit"** — Seth Jenkins (Google). Analysis of kernel crash logs and artifacts from real-world Android exploit samples found in the wild. How to reconstruct exploitation techniques from evidence. OffensiveCon 2025. [Advanced]

- **[2024] "How to Fuzz Your Way to Android Universal Root: Attacking Android Binder"** — Eugene Rodionov, Zi Fan Tan, Gulshan Singh. Systematic Binder fuzzing methodology that led to discoverable LPE vulnerabilities and a universal root primitive. OffensiveCon 2024. [Advanced]

- **[2024] "Attacking the Samsung Galaxy A* Boot Chain"** — Maxime Rossi Bellom, Damiano Melotti, Raphael Neveu, Gabrielle Viala (Synacktiv). Samsung MediaTek-based boot chain analysis and exploitation. OffensiveCon 2024. [Advanced]

- **[2024] "iOS 16/17 Kernel Exploitation"** — Modern iOS kernel exploit development: exploitation primitives, kernel data structure abuse, and PAC bypass as part of a functional chain. OffensiveCon 2024. [Advanced]

- **[2024] "Breaking PAC on iOS: A Practical Guide"** — Pointer Authentication Code bypass methodology from a practitioner's perspective: which bypass classes are reliable, which are theoretical, and how to construct a working primitive. OffensiveCon 2024. [Advanced]

- **[2023] "Heap Exploitation on ARM64"** — ARM64-specific heap exploitation techniques accounting for the allocator behavior on iOS and Android. OffensiveCon 2023. [Advanced]

- **[2023] "Return Oriented Programming on iOS"** — ROP chain construction methodology on iOS under modern mitigations including PAC. OffensiveCon 2023. [Advanced]

- **[2023] "Chrome/V8 to Kernel: Mobile Exploit Chain Development"** — Full chain development methodology from browser renderer compromise through sandbox escape to kernel privilege escalation on mobile. OffensiveCon 2023. [Advanced]

- **[2022] "Fuzzing the Android Kernel"** — Systematic kernel fuzzing with syzkaller adapted for Android: coverage collection, syscall grammar customization, and triage workflow. OffensiveCon 2022. [Advanced]

- **[2022] "LLDB Scripting for iOS Security Research"** — Custom LLDB automation for dynamic analysis of iOS targets: scripting the debugger for tracing, memory analysis, and exploit development assistance. OffensiveCon 2022. [Intermediate]

---

### Hexacon (Paris)

Hexacon covers binary exploitation and low-level security research across platforms. [https://www.hexacon.fr](https://www.hexacon.fr)

- **[2025] "Arise from the Wireless: Breaking the Security Barrier in Wi-Fi"** — Xiaobye. Structural flaws in WPA3 implementations and vendor-specific Wi-Fi Direct protocols. Video: [youtube.com/watch?v=qT3YFtGjiuQ](https://www.youtube.com/watch?v=qT3YFtGjiuQ) Hexacon 2025. [Advanced]

- **[2025] "Déjà Vu in Linux io_uring: Breaking Memory Sharing Again After Generations of Fixes"** — Pumpkin. TOCTOU vulnerabilities and race conditions in io_uring shared memory buffers yielding reliable LPE on Android devices that have backported the io_uring subsystem — despite continuous upstream patching. Hexacon 2025. [Advanced]

- **[2025] "Inside Apple Secure Enclave Processor in 2025"** — Quentin Salingue (Synacktiv). Deep architectural analysis of SEP in the 2025 era — how it interfaces with SPTM/TXM, its attack surface, and what has changed from earlier generations. Hexacon 2025. [Advanced]

- **[2025] "Paint it Blue: Attacking the Bluetooth Stack"** — Mehdi Talbi, Etienne Helluy-Lafont. Bluetooth stack exploitation methodology across mobile platforms. Hexacon 2025. [Advanced]

- **[2024] "Defense Through Offense: Building a 1-Click Calling Exploit in Messenger for Android"** — Andrew Calvano, Octavian Guzu, Ryan Hall (Meta Security). Full exploit chain in Meta Messenger for Android — methodology for finding and chaining vulnerabilities in a hardened production mobile app. Hexacon 2024. [Advanced]

- **[2024] "Attacking iOS Background Agents"** — Daemon and background process exploitation: attack surface of long-running iOS background agents, XPC interface abuse, and privilege escalation. Hexacon 2024. [Advanced]

- **[2024] "Android Trusted Applications: Breaking the TEE"** — TrustZone exploitation methodology: reverse engineering trusted applications, constructing exploit primitives from the normal world, and TEE isolation failures. Hexacon 2024. [Advanced]

- **[2023] "RE of Flutter Apps: Advanced Techniques"** — Beyond basic blutter usage: advanced Flutter reverse engineering covering snapshot internals, obfuscated builds, and native bridge analysis. Hexacon 2023. [Intermediate]

- **[2023] "Memory Corruption in iOS Kernel"** — Exploitation primitives and their construction from memory corruption vulnerabilities in the iOS kernel. Hexacon 2023. [Advanced]

---

### TROOPERS

TROOPERS covers enterprise and infrastructure security with a track on mobile and device security. [https://troopers.de](https://troopers.de)

- **[2025] "Over the Garden Wall — Let's Steal Data from Your iPhone"** — Nils Rollshausen. iPhone data exfiltration research exploiting protocol and pairing weaknesses. TROOPERS 2025. [Advanced]

- **[2025] "Securing the Airwaves: Emulation, Fuzzing, and Reverse Engineering of iPhone Baseband Firmware"** — Luca Glockow, Rachna Shriwas, Bruno Produit. iPhone baseband firmware fuzzing and reverse engineering methodology. TROOPERS 2025. [Advanced]

- **[2025] "30 min iOS Inactivity Reboot"** — Jiska Classen. Analysis of the iOS inactivity reboot security feature introduced in iOS 18.1 — how it works, what it protects, and its forensic implications. TROOPERS 2025. [Intermediate]

- **[2025] "Eastern Promises: Mobile VRP Lessons for Bug Hunters"** — Daniel Komaromy, Laszlo Szapula. Lessons learned from mobile vendor vulnerability reward programs — what gets rewarded, what doesn't, and common pitfalls. TROOPERS 2025. [Intermediate]

- **[2025] "Roaming Agreements — The Hidden 5G Attack Surface"** — Swantje Lange. 5G roaming security analysis revealing hidden attack surfaces in inter-operator agreements. TROOPERS 2025. [Advanced]

- **[2024] "Mobile Forensics: Extracting Evidence from Modern Devices"** — iOS and Android forensic acquisition methodology covering extraction techniques for locked devices, cloud backup analysis, and artifact interpretation. TROOPERS 2024. [Intermediate]

- **[2024] "Enterprise Mobile Security: MDM Attack Surfaces"** — MDM protocol exploitation, enrollment flow abuse, and management interface weaknesses as attack vectors against enterprise mobile deployments. TROOPERS 2024. [Intermediate]

- **[2023] "Practical Android Security Testing"** — Workshop-style Android security testing methodology covering static, dynamic, and network analysis across the full OWASP MASTG test case set. TROOPERS 2023. [Beginner]

- **[2023] "iOS Enterprise Deployment Attacks"** — DEP/MDM attack vectors: device enrollment protocol abuse, profile injection, and credential theft through enterprise iOS management infrastructure. TROOPERS 2023. [Intermediate]

---

### Zer0Con (Seoul)

Zer0Con is invite-only with top-tier offensive security research. Materials are almost never publicly released. Entries below reflect confirmed talks based on public disclosure.

- **[2026] "Researcher's Guide to the Galaxy: Samsung 0-Click, Android Messengers, DNG, and Image Formats"** — Brendon Tiszka (Google Project Zero). Newly discovered attack surfaces in Android and Samsung image format decoders, including zero-click vectors via messaging apps. Zer0Con 2026. [Advanced]

- **[2026] "Modern Android Kernel Exploitation Through a Mali Driver Vulnerability"** — Chih-Yen Chang (DEVCORE). Discovery and exploitation of Mali GPU vulnerabilities on Pixel 8a, kernel debugging techniques and SELinux bypass. Zer0Con 2026. [Advanced]

- **[2026] "Attacking Apple Display Co-Processor"** — Ye Zhang (Baidu). 14 CVEs discovered in Apple's Display Co-Processor firmware. Zer0Con 2026. [Advanced]

- **[2026] "Prompt2Pwn — LLMs Winning at Pwn2Own"** — Georgi G, Ben R (Interrupt Labs). Using AI agents for automated vulnerability discovery in Android apps, yielding multiple Samsung Bixby vulnerabilities at Pwn2Own. Zer0Con 2026. [Emerging]

- **[2025] "PAC2Own: From Bug to Shellcode in Modern Safari"** — Manfred Paul. Bypassing Pointer Authentication Codes on Apple ARM chips for Safari exploitation — full chain from initial bug to shellcode execution under modern iOS/macOS mitigations. Zer0Con 2025. [Advanced]

- **[2024] "How to Jailbreak iOS 16"** — Lars Fröder (opa334, creator of Dopamine and TrollStore). Technical deep-dive into jailbreak internals: exploitation primitives, kernel patching, and the rootless jailbreak design. Public slides available: [https://github.com/opa334/Presentations/blob/main/Zer0Con%202024%20-%20How%20to%20Jailbreak%20iOS%2016.pdf](https://github.com/opa334/Presentations/blob/main/Zer0Con%202024%20-%20How%20to%20Jailbreak%20iOS%2016.pdf) Zer0Con 2024. [Advanced]

- **[2024] "Beyond Android MTE: Navigating OEM's Logic Labyrinths"** — Georgi Geshev, Joffrey Guilbon. Strategic shift from defeating memory tagging directly to chaining logic bugs in OEM implementations: improper permission checks, state transitions, and symlink abuse for privilege escalation on Samsung and Xiaomi flagships. Zer0Con 2024. [Advanced]

- **[2024] "Bypassing ARM MTE with Speculative Execution"** — Jinbum Park. CPU speculative execution side-channel on Google Pixel 8: leaking MTE tags via L1 cache timing to forge valid kernel pointers, rendering the hardware mitigation ineffective. Two novel vulnerabilities confirmed. Zer0Con 2024. [Advanced]

- **[2024] "iOS 17 Full Chain"** — PAC bypass, sandbox escape, and kernel privilege escalation chained on iOS 17. Zer0Con 2024. [Advanced]

- **[2024] "New Attack Surfaces in iOS 17.x"** — Attack surface introduced with iOS 17 features: NameDrop, AirDrop improvements, StandBy mode, Journal app, and associated daemons. Zer0Con 2024. [Advanced]

- **[2023] "Fuzzing Samsung's Closed-Source Libraries as if on a Real Device"** — Hao Xiong, Qinming Dai. JVM emulation framework for fuzzing Samsung OEM-specific Android libraries without physical device bottlenecks: highly parallelized memory safety flaw discovery in closed-source APIs. Zer0Con 2023. [Advanced]

- **[2023] "Android Pixel Exploitation"** — Pixel-specific security research targeting Google's own firmware and kernel patches. Zer0Con 2023. [Advanced]

- **[2023] "Operation Triangulation: Behind the Research"** — Kaspersky GReAT presenting the full technical details of the Operation Triangulation exploit chain including the hardware MMIO register abuse (see Research Papers section above for the written companion). Zer0Con 2023. [Advanced]

---

### REcon (Montreal)

REcon focuses on reverse engineering. The mobile and firmware tracks consistently feature high-value baseband, protocol, and hardware research. Full session archive: [recon.cx](https://recon.cx)

- **[2025] "A Trip to Ancient BABYLON: Unearthing a 2017 Pegasus Persistence Exploit"** — Bill Marczak, Daniel Roethlisberger. Previously unpublished iOS 10 Pegasus persistence exploit from 2017 — root-cause vulnerability analysis and how the exploit achieved code execution after boot. REcon 2025. [Advanced]

- **[2025] "Call, Crash, Repeat: Hacking WhatsApp"** — Luke McLaren. Three separate WhatsApp vulnerabilities across iOS, Android, and macOS: a URL validation flaw, an XMPP parsing bug leading to PJSIP exploitation, and a logic issue allowing unauthorized video streams in group chats. REcon 2025. [Advanced]

- **[2024] "WatchWitch: Apple Watch Protocol Stack from Scratch"** — Nils Rollshausen. Reverse engineering the Apple Watch wireless protocol stack using Frida: homebrew cryptography and MitM opportunities in health data sync. Also presented at OBTS v8. REcon 2024. [Advanced]

- **[2023] "Fully Remote Baseband Vulnerabilities in the Exynos 5300"** — Natalie Silvanovich. Comprehensive audit of the Samsung Shannon Exynos 5300 modem demonstrating fully remote, cross-carrier exploitability without proximity — no rogue base station required. Video: [youtube.com/watch?v=LJ1NzJLMDUs](https://www.youtube.com/watch?v=LJ1NzJLMDUs) REcon 2023. [Advanced]

- **[2022] "When Wireless Malware Stays On After Turning Off iPhones"** — Jiska. Bluetooth and Ultra-Wideband chips remain active and can execute payloads even when the device is fully powered off — exposing a persistent, low-level attack surface invisible to the user. REcon 2022. [Advanced]

---

### Power of Community (POC)

POC is Korea's annual security conference featuring high-quality exploitation and reverse engineering research. Archive: `powerofcommunity.net` (site SSL certificate issues as of April 2026)

- **[2025] "Trigon: Developing a Deterministic iOS Kernel Exploit"** — Alfie CG (@alfiecg_dev). Deterministic iOS kernel exploitation methodology — constructing reliable exploit primitives without heap spraying. Blog companion: [alfiecg.uk/2025/03/01/Trigon.html](https://alfiecg.uk/2025/03/01/Trigon.html). POC 2025. [Advanced]

- **[2025] "The Biometric AuthToken Heist: Cracking PINs and Bypassing CE via a Long Ignored Attack Surface"** — Xuangan Xiao, Zikai Xu. Biometric authentication token attacks — cracking PINs and bypassing credential-encrypted storage via overlooked Android Keymaster attack surface. POC 2025. [Advanced]

- **[2025] "(Sploit)Lights, Camera, Action! Exploiting Spotlight to Bypass TCC and Leak Data from Apple Intelligence"** — Christine Fossaceca. Apple platform TCC bypass via Spotlight plugins, leaking private data including Apple Intelligence content. POC 2025. [Advanced]

- **[2024] "Breaking Through the Cage: Get Android Universal Root by B-PUAF"** — Hanqin Ling, Yutao Lu (Pangu Team). Binder driver page-table use-after-free enabling universal Android root across multiple devices and kernel versions. POC 2024. [Advanced]

- **[2024] "GPUAF: Two Ways of Rooting All Qualcomm Based Android Phones"** — Pan Zhenpeng, Jheng Bing Jhong (STAR LABS SG). Qualcomm GPU use-after-free enabling root on all affected Qualcomm Android devices, bypassing KNOX, KASLR, and DEFEX. POC 2024. [Advanced]

- **[2024] "Pishi: Coverage-Guided Fuzzing of the XNU Kernel and Arbitrary KEXT"** — Meysam Firouzi. XNU kernel fuzzing framework with coverage-guided fuzzing of kernel extensions — practical iOS/macOS kernel vulnerability discovery. POC 2024. [Advanced]

- **[2024] "An Insider Perspective on the Offensive Industry"** — Luca Todesco (Dataflow Security). Keynote by renowned iOS exploitation researcher on the commercial offensive exploit industry. POC 2024. [Advanced]

- **[2023] "Modern Chrome Exploit Development"** — Avboy1337, yyjb, vrk. V8 sandbox bypasses and hijacking of the Chrome V8 heap on mobile and desktop — comprehensive modern Chrome exploitation methodology. POC 2023. [Advanced]

- **[2023] "Evolution of Safari Mitigations and Bypasses"** — Nikita Pupyshev. Continuous cat-and-mouse between Apple's JIT hardening in WebKit and novel type-confusion strategies that consistently outmaneuver it. POC 2023. [Advanced]

- **[2022] "Fugu15: A Deep Dive into iOS 15 Exploitation"** — Linus Henze. Landmark iOS 15 jailbreak bypassing PAC entirely from user-space via a CoreTrust logic flaw — forging cryptographic signatures the kernel trusts without requiring a kernel memory corruption zero-day. POC 2022. [Advanced]

---

### Chaos Communication Congress (CCC)

CCC is the annual Chaos Communication Congress (Hamburg, December). Consistently features high-quality mobile and platform security research. Full archive: [https://media.ccc.de](https://media.ccc.de)

- **[2025] "DNGerousLINK: A Deep Dive into WhatsApp 0-Click Exploits on iOS and Samsung Devices"** — Zhongrui Li, Yizhe Zhuang, Kira Chen. WhatsApp spyware 0-click chain using CVE-2025-55177 (WhatsApp), CVE-2025-43300 (iOS RawCamera DNG parsing), and CVE-2025-21043 (Samsung OOB write). Remote zero-interaction attack via phone number alone. 39C3, December 2025. [Advanced]

- **[2025] "Not To Be Trusted — A Fiasco in Android TEEs"** — 0ddc0de, gannimo, Philipp. Privilege escalation from rooted user space to secure world on Xiaomi devices via missing TA rollback protection and a GlobalPlatform type confusion bug (CVE-2023-32835). First public exploit of the BeanPod TEE microkernel. 39C3, December 2025. [Advanced]

- **[2025] "Build a Fake Phone, Find Real Bugs: Qualcomm GPU Emulation and Fuzzing with LibAFL QEMU"** — Romain Malmain. Virtualizing Qualcomm Android kernels and GPU drivers using QEMU for fuzzing mobile GPU driver vulnerabilities — common escalation vectors in spyware chains. 39C3, December 2025. [Advanced]

- **[2025] "Bluetooth Headphone Jacking: A Key to Your Phone"** — Dennis Heinze, Frieder Steinmetz. Three CVEs (CVE-2025-20700/20701/20702) in Airoha Bluetooth audio chips used by Sony, Marshall, Jabra, and others — unauthenticated pairing, flash memory reads, and RAM manipulation via Bluetooth. 39C3, December 2025. [Advanced]

- **[2025] "Reverse Engineering the Pixel TitanM2 Firmware"** — willem. Reverse engineering Google Pixel's TitanM2 security chip (RISC-V based) using Ghidra and Python-based firmware simulation. 39C3, December 2025. [Advanced]

- **[2025] "Cracking Open What Makes Apple's Low-Latency WiFi So Fast"** — Henri Jager. Reverse engineering Apple's proprietary Low-Latency WiFi link-layer protocol used for Continuity features (Sidecar Display, Continuity Camera) via iOS kernel logging. 39C3, December 2025. [Advanced]

- **[2025] "Learning from South Korean Telco Breaches"** — Shinjo Park, Yonghyu Ban. Post-mortem of 2025 breaches at all three South Korean mobile operators including SK Telecom's HSS breach via BPFDoor malware (23 million SIM card replacements). 39C3, December 2025. [Intermediate]

- **[2025] "Watch Your Kids: Inside a Children's Smartwatch"** — Nils Rollshausen. Live hacking of a popular children's smartwatch exposing location data and communications of millions of children and parents. 39C3, December 2025. [Intermediate]

- **[2024] "From Pegasus to Predator: The Evolution of Commercial Spyware on iOS"** — Matthias Frielingsdorf (iVerify). Comprehensive timeline of iOS spyware from 2016–2024, detection methodology evolution, and BlastPass case study. One of the clearest public narratives of the iOS exploitation-for-hire ecosystem. 38C3, December 2024. [https://media.ccc.de/v/38c3-from-pegasus-to-predator-the-evolution-of-commercial-spyware-on-ios](https://media.ccc.de/v/38c3-from-pegasus-to-predator-the-evolution-of-commercial-spyware-on-ios) [Intermediate–Advanced]

- **[2024] "ACE up the Sleeve: Hacking into Apple's USB-C Controller"** — stacksmashing (Thomas Roth). Extended version of the DEF CON 32 talk on EM fault injection against the Apple ACE3 USB-C chip — hardware hacking at the boundary of iOS security. 38C3, December 2024. [Advanced]

- **[2024] "Ultrawide Archaeology on Android Native Libraries"** — Luca Di Bartolomeo, Rokhaya Fall. Novel static analysis technique for Android native code using wide-window analysis to recover semantic structure from stripped binaries. 38C3, December 2024. [Advanced]

- **[2024] "Auracast: Breaking Broadcast LE Audio Before It Hits the Shelves"** — Frieder Steinmetz, Dennis Heinze. Security analysis of Bluetooth LE Audio / Auracast broadcast standard — attack surface of a protocol being deployed in hearing aids, smart TVs, and mobile devices before widespread security review. 38C3, December 2024. [Advanced]

---

## 9. Exploit Techniques & Attack Surface Reference

A practitioner reference for the attack surface exposed by Android and iOS apps — IPC mechanisms,
platform-specific exploitation primitives, cross-platform framework weaknesses, and backend API
vulnerabilities. Use alongside the static and dynamic analysis tooling in [Sections 1–5](#1-tools--static-analysis).

### Android Attack Surface `[Android]`

#### IPC Mechanisms

**Intents & Exported Components**

Android's activity, service, and receiver components can be declared `exported="true"` (or
implicitly exported by declaring an `<intent-filter>`), making them invokable by any installed app.

- **Exported Activity hijacking** — launch exported activities without user interaction; trigger
  authentication flows, bypass lock screens, or access protected UI states.
- **Intent injection via deeplinks** — apps that pass deeplink URI parameters directly into
  `Intent` extras can be abused to trigger unintended component launches or pass attacker-controlled
  data to exported components.
- **Implicit broadcast abuse** — apps registering receivers for `android.intent.action.SEND` or
  similar system-wide broadcasts without appropriate signature-level permissions.
- **PendingIntent misuse** — a `PendingIntent` constructed with an empty base intent can be
  hijacked: the receiving app fills in the action, component, and extras, effectively inheriting
  the sending app's identity and permissions. High-value in privilege escalation chains.

Enumeration: `adb shell dumpsys package <package>` lists all components and their exported state.
Drozer's `app.package.attacksurface` and `app.activity.start` modules automate this.

**Content Providers**

Content providers expose a URI-based query interface. Common vulnerabilities:

- **SQL injection** via the `selection` parameter — test with `'` and `1=1--` patterns.

- **Path traversal in file providers** — `content://com.example.fileprovider/../../etc/passwd`
  style traversal in providers backed by `FileProvider`. Test the `openFile()` path.
- **Unauthenticated provider access** — providers without `android:readPermission` are readable
  by all installed apps.

Reference: [Android Content Provider Security](https://developer.android.com/guide/topics/providers/content-provider-basics)

**Binder IPC**

Binder is the kernel-level transport for all Android IPC. Direct exploitation of the Binder driver
is a kernel-level attack surface:

- **Reference counting flaws** — leading to use-after-free conditions. CVE-2020-0041,
  CVE-2021-0920 (BadSpin) are examples of kernel Binder UAF bugs used in LPE chains.
- **Transaction size manipulation** — large transaction payloads can stress server-side parsing.

- **Binder death notifications** — edge cases in death notification handling have historically
  produced exploitable conditions.

[Android Binder IPC](https://source.android.com/docs/core/architecture/hidl/binder-ipc) — AOSP
documentation covering Binder internals.

**Broadcast Receivers**

- **Ordered broadcast tampering** — a high-priority receiver can modify or abort the broadcast
  before legitimate receivers process it.
- **Sticky broadcast sniffing** — deprecated but still present in legacy apps; sticky broadcasts
  are cached and delivered to any newly registered receiver.
- **Dynamic receiver registration without restrictions** — receivers registered at runtime without
  `RECEIVER_NOT_EXPORTED` (Android 13+) are accessible to all apps.

#### WebView Attacks `[Android]`

WebViews in Android apps are a consistently productive attack surface:

| Vector | Condition | Impact |
|--------|-----------|--------|
| `addJavascriptInterface` | Interface exposed to WebView content | Remote code execution pre-API 17; data theft on modern versions |
| `setAllowFileAccess(true)` | Default in older API levels | Read local files via `file://` URIs loaded in WebView |
| `setAllowUniversalAccessFromFileURLs(true)` | Non-default but seen in apps | Cross-origin reads — `file://` page reads arbitrary `file://` resources |
| `shouldOverrideUrlLoading` bypass | Improper scheme validation | `intent://` URI handling can launch arbitrary activities |
| Universal XSS via `loadDataWithBaseURL` | Attacker-controlled base URL | Inject arbitrary-origin script content |

- See also: [Android Deep Link Issues and WebView Exploitation](https://8ksec.io/android-deeplink-and-webview-exploitation-8ksec-blogs/) — end-to-end walkthrough combining deep link abuse with WebView exploitation. `[Intermediate]`

#### Android Data Storage

- **SharedPreferences** — stored as world-readable XML files pre-Android 9 on older devices;
  `/data/data/<package>/shared_prefs/`. Root or backup access required on modern devices.
- **External storage** — files written to `getExternalStorageDirectory()` are readable by all apps
  with `READ_EXTERNAL_STORAGE` (Android ≤12) or `READ_MEDIA_*` permissions (Android 13+).
- **SQLite databases** — unencrypted by default; extracted via ADB backup or root file access.
  SQLCipher or similar required for encryption.
- **Log exposure** — `android.util.Log.*` output is readable by apps with `READ_LOGS` on older
  Android versions. On modern Android, log access is restricted to the app itself and system.
- **Clipboard sniffing** — `ClipboardManager` access was unrestricted pre-Android 10. Since
  Android 12, background reads trigger a toast notification; direct access still possible
  when app is in the foreground.

---

### iOS Attack Surface `[iOS]`

#### XPC Services

XPC is the primary inter-process communication mechanism on iOS and macOS. iOS services — daemons,
extensions, and system frameworks — expose XPC endpoints.

- **Missing entitlement validation** — XPC servers should verify client entitlements using
  `xpc_connection_copy_entitlement_value`. Absent checks allow any process to call privileged
  operations.
- **Deserialization of untrusted input** — XPC messages are typed but large deserialization
  surfaces exist; fuzzing XPC endpoints has produced real sandbox escapes and LPE bugs.
- **Monitoring XPC traffic**: [xpcspy](https://github.com/hot3eed/xpcspy) — Frida-based XPC
  traffic monitor; logs all XPC messages for a target process. `[Advanced]`
- [Advanced Frida: Inspecting XPC Calls](https://8ksec.io/advanced-frida-usage-part-3-inspecting-ios-xpc-calls/) — runtime XPC interception with Frida.

#### URL Schemes & Universal Links `[iOS]`

- **Custom URL scheme hijacking** — when multiple apps register the same URI scheme, iOS selects
  one (typically the last installed). Malicious apps can register common schemes to intercept
  inter-app data flows.
- **Universal Links misconfiguration** — the `apple-app-site-association` (AASA) file must be
  served over HTTPS with correct JSON structure. Misconfigured AASA allows an attacker's app to
  claim the domain's links.
- **Open redirects via scheme parameters** — `myapp://redirect?url=https://evil.com` patterns
  where the app naively opens the parameter in Safari expose users to phishing.
- [iOS Deep Link Attacks — Part 1: Introduction](https://8ksec.io/ios-deeplink-attacks-part-1-introduction-8ksec-blogs/) and [Part 2: Exploitation](https://8ksec.io/ios-deep-link-attacks-part-2-exploitation-8ksec-blogs/) — full exploitation walkthrough of iOS deep link attack surface.

#### iOS Data Storage

| Location | Risk | Notes |
|----------|------|-------|
| `NSUserDefaults` | Cleartext key-value storage | Extractable from jailbroken devices and iTunes backups |
| `NSFileProtectionNone` | No encryption at rest | Files accessible even when device is locked |
| `kSecAttrAccessibleAlways` | Keychain item accessible without device unlock | Extractable post-boot without authentication |
| Core Data stores | Unencrypted by default | Full database accessible from device backup or jailbreak |
| `NSURLCache` | HTTP response caching | May contain sensitive API responses; check `~/Library/Caches/<bundle>` |
| Pasteboard (`UIPasteboard.generalPasteboard`) | Cross-app readable | iOS 16+ restricts background reads with user notification; foreground access unrestricted |

#### App Extensions & App Groups

- **Shared app group containers** — all apps sharing an App Group ID can read and write the same
  container directory. A compromised or malicious app in the group reads all shared data.
- **Widget extensions** — run as separate processes but share the app group container; may handle
  sensitive data that the main app also stores there.
- **Notification Service Extensions** — intercept notification content before display; a
  compromised extension (via malicious notification payload) can exfiltrate notification content.
- **Share Extensions** — receive data from other apps; untrusted input from sharing should be
  treated as attacker-controlled.

#### iOS Modern Mitigations and Research Bypass Techniques `[Advanced]`

Understanding iOS mitigations is prerequisite knowledge for any iOS exploitation research:

| Mitigation | What It Does | Bypass Research |
|------------|-------------|-----------------|
| **PAC (Pointer Authentication)** | QARMA-based cryptographic pointer signing — detects pointer corruption on A12+ | PACMAN (speculative execution bypass), Pointer Authentication bypass via physics, various Project Zero research |
| **PPL (Page Protection Layer)** | Hardware enforcement of code-signing pages — prevents kernel from marking arbitrary pages as executable | Requires kernel exploit that flips PPL state; no known generic bypass |
| **KTRR** | Marks kernel text as read-only at hardware level | Compound technique required; mitigated many early JOP approaches |
| **BlastDoor** | Sandbox isolates iMessage attachment processing (iOS 14+) | BLASTPASS (2023) — requires WebP heap overflow to escape BlastDoor |
| **Lockdown Mode** (iOS 16+) | Disables JS JIT, complex attachments, FaceTime from unknown numbers | Shrinks attack surface; not a mitigation per se but reduces viable vectors |
| **CoreTrust** | Validates code signing at load time | Historically bypassed via TrollStore on specific iOS versions |

Reference: [Apple Platform Security Guide](https://support.apple.com/guide/security/welcome/web) — authoritative source for all iOS security architecture.

---

### Cross-Platform Framework Attack Surfaces `[Cross-Platform]`

#### React Native

- **JS bridge injection** — native modules exposed via the bridge that do not validate caller
  context; a compromised or malicious JS bundle can invoke privileged native methods.
- **Bundle tampering** — `index.android.bundle` or `main.jsbundle` is often unprotected;
  patching the bundle modifies app logic without touching native code.
- **Hermes engine attack surface** — the Hermes bytecode interpreter is a complex parser;
  malformed `.hbc` files are an underexplored fuzzing target.
- **`AsyncStorage`** — cleartext key-value store; extractable from the device without root on
  Android (via ADB backup on older versions) or from jailbroken iOS devices.
- **In-app `WebView`** — React Native's `<WebView>` component inherits all native WebView risks.

#### Flutter

- **Platform channel injection** — method channels between Dart and native code lack
  authentication; a WebView or malicious plugin in the same app can call platform channels
  if not properly scoped.
- **Dart VM service** — the VM service interface (used for hot reload) must be disabled in
  release builds. An exposed VM service provides remote code execution and full process introspection.
- **libapp.so extraction** — contains all compiled Dart AOT snapshot logic; trivially extractable
  from the APK assets. Analyzed with [blutter](https://github.com/worawit/blutter) for class/method reconstruction.
- **Flutter ignores system proxy** — Dart's networking stack bypasses Android/iOS system proxy
  settings. Standard Burp setup will not capture Flutter HTTPS traffic.
  Workarounds: ProxyPin (TUN-based), reFlutter (engine patch), Frida hook on `dart:io HttpClient`.

#### Xamarin / .NET MAUI

- **.NET assembly extraction** — Xamarin and MAUI assemblies (`.dll` files) are bundled in the
  APK/IPA; trivially decompiled with [ILSpy](https://github.com/icsharpcode/ILSpy) or
  [dnSpyEx](https://github.com/dnSpyEx/dnSpy) back to near-source-level C# without obfuscation.
- **Embedded resource inspection** — resources (including credentials, config files) may be
  embedded in assemblies and extractable via decompilation.
- **Mono runtime debugging** — `dnSpyEx` supports attaching to the Mono runtime on Android for
  live debugging of Xamarin apps.

---

### API & Backend Attack Surface

Mobile apps expose backend vulnerabilities through their API traffic. These are consistently the
highest-signal findings in mobile assessments.

**Authorization Flaws**

- **BOLA / IDOR** (Broken Object Level Authorization) — the most common mobile API vulnerability.
  Test: replace your user's resource IDs in API calls with another user's IDs. Many mobile backends
  rely on the mobile client to enforce access control rather than validating server-side.
- **Mass assignment** — over-posting via mobile API endpoints; append additional JSON fields to
  requests and observe if they are accepted and applied.
- **Function-level authorization** — mobile apps sometimes call API endpoints that the web UI
  does not expose; enumerate endpoints from the APK/IPA and test each for missing auth checks.

**JWT Vulnerabilities**

- **Algorithm confusion** — change `RS256` to `HS256` and sign with the public key as the HMAC
  secret. Many libraries accept this without validation.
- **`alg: none`** — remove the signature and set algorithm to `none`. Accepted by some
  legacy implementations.
- **Weak secrets** — HMAC-SHA256 with a weak or default secret; brute-force with
  [jwt_tool](https://github.com/ticarpi/jwt_tool) or hashcat.

Tooling: [jwt.io](https://jwt.io/) for decoding; [jwt_tool](https://github.com/ticarpi/jwt_tool)
for automated testing.

**Firebase Misconfigurations**

Firebase Realtime Database and Firestore misconfigurations are among the most common and
impactful mobile bug bounty findings.

- **Detection**: Append `.json` to discovered Firebase URLs:
  `curl https://<project>.firebaseio.com/.json`
  A `200 OK` response with data indicates unauthenticated read access.
- **Storage misconfigurations**: `https://storage.googleapis.com/<bucket>/` — list bucket
  contents if ACL is `allUsers`.
- **Cloud Functions**: Functions accessible without auth tokens; test all endpoint paths extracted
  from the APK.
- **Automated scanning**: [FireBaseScanner](https://github.com/shivsahni/FireBaseScanner) —
  automated detection of misconfigured Firebase instances.

**GraphQL**

- **Introspection in production** — enabled by default; reveals full schema. Test:
  `{"query": "{__schema{types{name}}}"}`.
- **Batch query abuse** — GraphQL allows multiple operations per request; combine expensive
  queries to DoS or trigger rate-limit bypass.
- **Field suggestions** — even with introspection disabled, field suggestion errors leak schema
  details; GraphQL introspection enumeration tools can discover hidden fields.

**gRPC**

- **Reflection enabled in production** — gRPC reflection exposes full service definitions.
  Test with [grpc_cli](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md):
  `grpc_cli ls <host>:<port>`
- **Missing authentication on streaming endpoints** — bidirectional streaming endpoints are
  sometimes implemented without the same auth checks as unary RPCs.
- Interception: Burp Suite with [Blackboxer](https://github.com/nccgroup/blackboxprotobuf)
  or [protobuf-decoder](https://github.com/mafintosh/protocol-buffers) for manual decoding.

---

### Pentesting Checklists & Methodologies

Structured checklists and step-by-step guides for running mobile security assessments.

**Android:**
- **[OWASP MASTG Test Cases](https://mas.owasp.org/MASTG/tests/)** — Comprehensive test cases covering all MASVS categories. Use as a test case tracker during assessments. [Beginner–Intermediate]

- **[OWASP MASTG Test Cases](https://mas.owasp.org/MASTG/)** — The authoritative test case source. Each MASVS control has one or more MASTG test cases with step-by-step instructions.

- **[MobileApp-Pentest-Cheatsheet](https://github.com/tanprathan/MobileApp-Pentest-Cheatsheet)** — Community mobile pentesting cheatsheet. Android and iOS coverage, commands included.

- **[ADB Cheatsheet](https://developer.android.com/tools/adb)** — ADB command reference for security testers.

- **[AAPG — Android Application Penetration Testing Guide](https://nightowl131.github.io/AAPG/)** — Step-by-step Android app pentesting guide. [Beginner–Intermediate]

- **[Frida Code Snippets for Android](https://erev0s.com/blog/frida-code-snippets-for-android/)** — Ready-to-use Frida scripts for common Android hooking tasks.

**iOS:**
- **[HackTricks iOS Pentesting Checklist](https://hacktricks.wiki/en/mobile-pentesting/ios-pentesting-checklist.html)** — Structured iOS assessment checklist. [Beginner–Intermediate]


- **[OWASP MASVS Checklist](https://mas.owasp.org/checklists/)** — Downloadable MASVS verification checklist. Primary deliverable template for compliance-oriented assessments.

- **[randorisec Mobile Hacking Cheatsheet](https://github.com/randorisec/MobileHackingCheatSheet)** — Concise cheatsheet covering Android and iOS testing commands and techniques.

**Configuration & Proxy Setup:**
- **[Configuring Burp Suite with Android Nougat+](https://blog.ropnop.com/configuring-burp-suite-with-android-nougat/)** — Step-by-step Burp proxy setup for Android with user certificate restrictions.

- **[Bypassing Xamarin Certificate Pinning on Android](https://gosecure.ai/blog/2020/04/06/bypassing-xamarin-certificate-pinning-on-android/)** — GoSecure. Specific bypass for .NET/Xamarin apps.

---

## 10. Advanced Platform Internals

Deep platform internals for vulnerability research and advanced analysis. Content here assumes
familiarity with the core attack surface in Section IX.

### Android Kernel & Platform Internals `[Advanced]`

#### Binder Driver Internals

The Binder kernel driver (`/dev/binder`) is the transport for all Android system service calls.
Understanding the driver's transaction processing, reference counting, and memory management is
prerequisite knowledge for Android kernel vulnerability research.

| Resource | Description |
|----------|-------------|
| [Android Binder Documentation](https://source.android.com/docs/core/architecture/hidl/binder-ipc) | AOSP source documentation — Binder architecture, transaction format, and threading model. |
| [Android Internals Vol. 1 — Jonathan Levin](http://newandroidbook.com/) | Deep coverage of Binder IPC internals, the service manager, and inter-process data passing. Free PDF. `[Advanced]` |
| [Google Project Zero — Binder Research](https://projectzero.google/) | Multiple Binder vulnerability writeups; search the blog for "Binder" — CVE-2020-0041 (BadSpin precursor), CVE-2021-0920 analysis. |

Key vulnerability classes in the Binder driver:
- **Use-after-free** — Binder node and reference counting bugs; CVE-2021-0920 (BadSpin) is the
  canonical recent example — exploited in the wild by commercial spyware.
- **Out-of-bounds reads/writes** — transaction buffer parsing errors in `binder_transaction`.

- **Type confusion** — mismatched object type handling during transaction deserialization.

#### Android Kernel Exploitation

| Resource | Description |
|----------|-------------|
| [syzkaller](https://github.com/google/syzkaller) | Google's kernel fuzzer — discovers Android kernel bugs via syscall fuzzing. Used extensively by Project Zero. `[Advanced]` |
| [Android Kernel CVEs — NVD](https://nvd.nist.gov/vuln/search/results?query=android+kernel) | CVE database filtered to Android kernel — track patch cadence and historical vulnerability classes. |
| [CVE-2023-26083: Kernel Pointer Leakage in Mali GPU](https://8ksec.io/cve-2023-26083-kernel-pointer-leakage-in-mali-gpus-timeline-stream-message-buffers/) | Original vulnerability research into Mali GPU kernel pointer leakage used in real Predator spyware attack chains. Covers the full kill-chain from pointer disclosure to exploitation. `[Advanced]` |

Key attack surfaces beyond Binder:
- **GPU drivers** — Mali (Arm) and Adreno (Qualcomm) GPU drivers have historically been
  high-yield targets. CVE-2023-26083 (Mali pointer leakage, exploited in real attack chains).
  Pwn2Own Toronto has multiple Adreno exploit chains on record.
- **Media codecs** — Stagefright-era vulnerabilities; modern codec sandboxing has reduced impact
  but driver-level codec bugs remain viable.
- **USB subsystem** — attack surface accessible when the device is connected; relevant for
  physical access scenarios.

#### Android SELinux

SELinux enforces mandatory access control across all Android processes — every file, socket,
Binder node, and property is labeled, and policy governs all access between labels.
Understanding SELinux policy is essential for privilege escalation research: an LPE that gains
root still runs under the original SELinux domain unless policy can also be bypassed.

| Resource | Description |
|----------|-------------|
| [SELinux for Android Overview](https://source.android.com/docs/security/features/selinux) | AOSP documentation — Android's SELinux implementation, policy structure, and enforcement modes. |
| [setools (sesearch, seinfo)](https://github.com/SELinuxProject/setools) | Policy analysis utilities — enumerate allowed transitions, query rules for specific domains. |
| [Android SELinux Internals Part I](https://8ksec.io/android-selinux-internals-part-i-8ksec-blogs/) | Deep dive into Android SELinux policy enforcement internals: domain transitions, neverallow rules, and how policy bypass has been used in real privilege escalation chains. `[Advanced]` |

Quick checks: `adb shell getenforce` (Enforcing/Permissive), `adb shell ps -Z` (process labels),
`adb shell ls -Z /data/data/<package>` (file labels).

#### TrustZone & TEE `[Advanced]`

The Trusted Execution Environment (TEE) runs in ARM TrustZone's Secure World, isolated from the
Normal World Android OS. TEE attack surface includes:

- **Trusted Applications (TAs)** — vendor-specific code executing in the Secure World; bugs here
  achieve key material extraction and full TEE compromise.
- **Shared memory interfaces** — the Normal World / Secure World boundary is crossed via shared
  memory; parsing bugs at this boundary are high-value targets.
- **`tee-supplicant`** — user-space TEE daemon handling TA loading and shared memory management.

| Resource | Description |
|----------|-------------|
| [ARM TrustZone Architecture](https://developer.arm.com/ip-products/security-ip/trustzone) | ARM's documentation for TrustZone hardware isolation — foundational reading. |
| [Quarkslab TEE Research](https://blog.quarkslab.com/tag/trustzone.html) | Published research on Samsung Kinibi/TEEGRIS TEE exploitation and TA analysis. `[Advanced]` |
| [OP-TEE (Open Portable TEE)](https://github.com/OP-TEE/optee_os) | Open-source TEE reference implementation — useful for understanding TEE architecture before attacking proprietary TEEs. |

#### ARM Memory Tagging Extension (MTE) `[Advanced]`

MTE is a hardware memory safety mechanism available on Cortex-X3+ chips and deployed on
Pixel 8 and later devices. Each memory allocation receives a random 4-bit tag; hardware validates
tags on every load/store, detecting use-after-free and heap buffer overflows in hardware.

| Resource | Description |
|----------|-------------|
| [Google Security Blog — MTE Deployment](https://security.googleblog.com/2024/09/deploying-rust-in-existing-firmware.html) | Google's documentation of MTE deployment in Android and the threat model it addresses. |
| [TikTag — MTE Bypass](https://arxiv.org/abs/2406.08719) | PoC demonstrating MTE bypass via speculative execution on Cortex-X1. Published at USENIX 2024. `[Advanced]` |
| [ARM MTE Architecture Reference](https://developer.arm.com/documentation/101925/latest/) | ARM's technical reference manual for the Memory Tagging Extension. |
| [ARM64 Reversing and Exploitation Part 10: Intro to MTE](https://8ksec.io/arm64-reversing-and-exploitation-part-10-intro-to-arm-memory-tagging-extension-mte/) | Practical introduction to MTE as a mitigation and research target: tag granules, tag checking modes, heap allocator interaction, and what MTE does and does not protect against. `[Intermediate–Advanced]` |

#### Android Automotive OS (AAOS) `[Emerging]`

AAOS is a distinct build of Android targeting in-vehicle infotainment (IVI) systems. New
attack surfaces relative to standard Android:

- **Vehicle HAL (Hardware Abstraction Layer)** — AAOS introduces VHAL, which interfaces Android
  apps with vehicle systems (CAN bus, sensors, climate control). Misconfigured VHAL exposes
  vehicle control APIs to third-party apps.
- **OTA update mechanisms** — automotive OTA update stacks are custom per OEM; integrity
  verification weaknesses allow persistent firmware implants.
- **Car APIs** (`android.car.*`) — permission model for vehicle-specific APIs is less mature
  than the standard Android permission model.

| Resource | Description |
|----------|-------------|
| [Android Automotive OS Documentation](https://source.android.com/docs/automotive) | AOSP documentation — AAOS architecture, VHAL, and differences from standard Android. |
| [ENISA Automotive Cybersecurity Guidelines](https://www.enisa.europa.eu/publications/smart-cars) | EU guidance on automotive cybersecurity, relevant for AAOS deployment security. |

---

### iOS Platform Internals `[Advanced]`

#### XNU Kernel

XNU is a hybrid kernel (Mach + BSD + IOKit). The primary attack surfaces for kernel privilege
escalation research:

- **Mach ports** — IPC primitives used by essentially all iOS inter-process communication. Mach
  port name space exhaustion, port UAF, and task port exposure bugs have produced numerous LPEs.
- **IOKit drivers** — object-oriented driver framework; IOKit is historically the most
  exploited iOS kernel attack surface. `IOUserClient` subclasses expose arbitrary method dispatch
  to userspace.
- **BSD syscalls** — file system, networking, and process management syscalls; complex parsing
  paths are recurring targets.

| Resource | Description |
|----------|-------------|
| [ipsw](https://github.com/blacktop/ipsw) | Essential iOS/macOS research tool — IPSW parsing, kernelcache extraction, dyld shared cache analysis, kernel diff between versions. `[Advanced]` |
| [ghidra_kernelcache](https://github.com/0x36/ghidra_kernelcache) | Ghidra scripts for iOS kernelcache analysis — annotates virtual tables, recovers classes, and simplifies IOKit driver RE. `[Advanced]` |
| [Siguza's iOS Security Research](https://blog.siguza.net/) | Deep technical iOS internals — PAC, PPL, KTRR, sandbox, and exploit technique analysis. Essential reading for iOS kernel research. `[Advanced]` |
| [*OS Internals Vol. I–III — Jonathan Levin](http://www.newosxbook.com/) | Authoritative reference for Darwin/iOS/macOS kernel internals. Freely downloadable. `[Advanced]` |
| [ipsw Walkthrough — Parts 1 & 2](https://8ksec.io/ipsw-walkthrough-part-1-the-swiss-army-knife-for-ios-macos-security-research/) | Practical guide to ipsw for IPSW parsing, dyld shared cache extraction, binary diffing between firmware versions, and kernel symbol recovery. Covers the full research workflow from firmware download to analysis in Ghidra. `[Intermediate]` |

#### iOS Jailbreak Internals

Understanding jailbreak toolchains is useful for grasping what a full iOS kernel exploit chain
looks like — from userspace trigger to kernel code execution and platform policy bypass.

| Jailbreak | Basis | iOS Versions | Notes |
|-----------|-------|-------------|-------|
| **checkm8** | Bootrom exploit (unpatchable) | A5–A11 chips | Hardware vulnerability; foundation for palera1n. |
| **palera1n** | checkm8 | iOS 15–17.x (A8–A11) | Most reliable for kernel research; rootful mode available. |
| **Dopamine** | Kernel exploit (software) | iOS 15–16.x (A12+) | Rootless; semi-untethered. Version-specific, fragile. [Build from source guide](https://8ksec.io/compiling-dopamine-jailbreak/). `[Advanced]` |
| **TrollStore** | CoreTrust bypass | Varies by iOS version | Not a jailbreak; installs unsigned IPAs permanently. |

Reference: [canijailbreak.com](https://canijailbreak.com/) — current jailbreak support by iOS version.

#### iOS Memory Safety Mitigations `[Advanced]`

| Mitigation | Technical Details | Research |
|------------|------------------|----------|
| **PAC** | QARMA-based HMAC on pointers using per-process diversified keys. `paciza`, `autiza` instructions. IA/IB/DA/DB key classes. A12+ (arm64e). | [PACMAN (MIT, 2022)](https://pacmanattack.com/) — speculative execution bypass |
| **PPL** | Separate hardware-enforced page table for code-signing state. `SPRR` registers. Kernel cannot mark unsigned pages executable post-PPL setup. | No generic bypass; requires PPL write primitive via kernel exploit |
| **KTRR / AMCC** | Kernel text region and XNU binary pages locked read-only after boot. | Mitigated JOP-based kernel patching approaches |
| **BlastDoor** | Isolated sandbox process for iMessage content parsing (iOS 14+). Separate address space, restricted syscalls. | BLASTPASS (CVE-2023-41061/41064) — WebP heap overflow chain bypassed BlastDoor |
| **Lockdown Mode** | Disables WebKit JIT, FaceTime from unknowns, wired connections to unknown accessories, complex link previews. | Significantly reduces remote attack surface; trade-off is functionality loss |

| Resource | Description |
|----------|-------------|
| [MIE Deep Dive — Parts 1 & 2](https://8ksec.io/mie-deep-dive-kernel/) | Kernel-level analysis of Apple's Memory Integrity Enforcement (MIE) subsystem: architecture, what it protects, and boundaries identified by current research. `[Advanced]` |
| [Patch Diffing CVE-2024-23265: iOS Kernel Memory Corruption](https://8ksec.io/patch-diffing-ios-kernel/) | Applied binary diffing methodology for iOS kernel CVE analysis: diffing toolchain setup, root cause identification, and exploitability assessment. `[Advanced]` |
| [Analyzing iOS Kernel Panic Logs](https://8ksec.io/analyzing-kernel-panic-ios/) | How to extract actionable signal from iOS kernel panic logs for vulnerability triage, crash reproduction, and exploit development. `[Advanced]` |
| [Reading iOS Sandbox Profiles](https://8ksec.io/reading-ios-sandbox-profiles/) | Parse and interpret iOS sandbox profile compilation artifacts: tooling, schema structure, and what restrictions matter for exploit chain development. `[Advanced]` |

#### iOS Baseband & Wireless Security `[Advanced / Niche]`

The baseband processor runs a separate RTOS handling all cellular communication. It shares memory
with the application processor, making remote baseband exploitation a potential path to full device
compromise without user interaction.

| Resource | Description |
|----------|-------------|
| [Project Zero — Exynos Modem Zero-Days (2023)](https://blog.google/threat-analysis-group/0-days-exploited-by-commercial-surveillance-vendor-in-egypt/) | 18 zero-days in Samsung Exynos modems enabling internet-to-baseband RCE. Affected Pixel 6/7 and Samsung Galaxy devices. The most significant public baseband research in years. `[Advanced]` |
| [Project Zero — Broadpwn](https://projectzero.google/2017/04/over-air-exploiting-broadcoms-wi-fi_4.html) | Over-the-air Wi-Fi exploitation via Broadcom chips — affects both Android and iOS. Foundational read for wireless attack research. `[Advanced]` |
| [BaseSAFE](https://github.com/fgsect/BaseSAFE) | Baseband firmware fuzzing framework using QEMU and Unicorn — applied to Shannon and Mediatek baseband images. `[Advanced]` |
| [ShannonRE](https://github.com/grant-h/ShannonRE) | Shannon modem reverse engineering toolkit — IDA scripts and utilities for Samsung baseband analysis. `[Advanced]` |
| [SRLabs — SS7 & Cellular Attacks](https://srlabs.de/blog) | Karsten Nohl's research on SS7 and LTE network-layer attacks — mobile network threat landscape. |

---

### Mobile Web3 & Crypto Wallet Security `[Intermediate–Advanced]`

| Attack Class | Description | Reference |
|-------------|-------------|-----------|
| **Seed phrase cleartext storage** | Mnemonics stored in `NSUserDefaults` or `SharedPreferences` — extractable from jailbroken/rooted devices and iTunes backups | [Mobile Malware: Crypto Wallet Stealer](https://8ksec.io/mobile-malware-analysis-part-1-crypto-wallet-stealer/) |
| **Private key outside hardware enclave** | Keys stored in software keychain rather than Secure Enclave (iOS) or StrongBox Keymaster (Android) — extractable from compromised device | [iOS Secure Enclave Key Protection](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/protecting_keys_with_the_secure_enclave) |
| **Clipboard hijacking** | Malicious apps (or accessibility services) monitor clipboard for wallet addresses and substitute attacker-controlled addresses | [MITRE ATT&CK Mobile T1414](https://attack.mitre.org/techniques/T1414/) |
| **Fake URI scheme interception** | Multiple wallets registering identical schemes; malicious wallet app installed last intercepts deeplinks carrying transaction data | [iOS Deep Link Attacks](https://8ksec.io/ios-deeplink-attacks-part-1-introduction-8ksec-blogs/) |
| **Smart contract interaction MITM** | dApp browser WebViews without certificate validation; phishing via deeplinks pointing to lookalike dApp URLs | [Trail of Bits Publications](https://github.com/trailofbits/publications) |

**Correct key storage:**
- Android: [Android Keystore System](https://developer.android.com/training/articles/keystore) — hardware-backed key storage requiring biometric or PIN for operations.

- iOS: [Secure Enclave Key Operations](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/protecting_keys_with_the_secure_enclave) — private keys that cannot leave the chip.

---

### On-Device AI/ML Model Security `[Emerging]`

Mobile apps increasingly bundle ML models for local inference. The security implications are
underresearched relative to the deployment pace.

| Attack Class | Notes |
|-------------|-------|
| **Model extraction** | CoreML `.mlmodelc` bundles and TFLite `.tflite` files are trivially extracted from the app binary unless encrypted. May contain proprietary training data or architecture details. |
| **Model inversion** | Reconstruct training data approximations from model outputs — relevant for models trained on PII (face recognition, voice models). |
| **Adversarial examples** | Craft inputs causing model misclassification — practical against on-device content moderation, OCR, and biometric unlock models. |
| **Prompt injection in on-device LLMs** | Mobile apps integrating llama.cpp, CoreML LLMs, or Gemini Nano that process untrusted user or document content are susceptible to prompt injection. Emerging attack surface. |
| **Telemetry side-channels** | Model inference results sent as telemetry without user awareness; behavioral profiling from model inputs/outputs. |

| Resource | Description |
|----------|-------------|
| [CoreML Security Documentation](https://developer.apple.com/documentation/coreml) | Apple's CoreML docs — model encryption options and deployment guidance. |
| [LiteRT (formerly TensorFlow Lite) Guide](https://ai.google.dev/edge/litert/guide) | LiteRT deployment guide — `.tflite` models are not encrypted by default and are extractable from APK assets. |
| [Nicholas Carlini — ML Security Papers](https://nicholas.carlini.com/papers) | Foundational ML security research — training data extraction, adversarial examples, membership inference. |

---

### MDM & Enterprise Security `[Intermediate]`

MDM (Mobile Device Management) is a target domain — the MDM server, enrollment infrastructure,
and configuration profiles are all attackable using the standard analysis tooling in Sections
VII–IX.

#### MDM Protocol & Apple Infrastructure

| Resource | Description |
|----------|-------------|
| [Apple MDM Protocol Reference](https://developer.apple.com/documentation/devicemanagement) | Authoritative MDM command and payload reference — enrollment, configuration profiles, management commands, and MDM check-in protocol. |
| [Apple Business Manager / DEP](https://support.apple.com/guide/apple-business-manager/welcome/web) | Automated Device Enrollment (formerly DEP) documentation — understand the enrollment flow before testing for hijacking. |

#### MDM Attack Vectors

- **DEP enrollment hijacking** — intercept the MDM enrollment request (performed via a serial
  number lookup against Apple's servers) to enroll a non-corporate device into the organization's
  MDM. Research: Jesse Endahl and Max Bélanger (USENIX 2018), James Barclay and Samantha Rosenblatt (DEF CON 26).
- **MDM profile injection** — install malicious `.mobileconfig` files via phishing; profiles can
  configure VPN, proxy, Wi-Fi, and trust arbitrary CAs.
- **Configuration profile analysis** — extract and analyze profiles installed on a device to map
  security policies, installed trust anchors, and VPN configurations.
- **Enterprise certificate abuse** — leaking or misusing enterprise signing certificates to
  distribute apps outside the App Store bypassing App Review.

#### MDM Analysis Tools

| Tool | Link | Description |
|------|------|-------------|
| **ideviceinstaller** | [libimobiledevice.org](https://libimobiledevice.org/) | Install and manage apps, list and extract profiles from iOS devices. |
| **Apple Configurator 2** | [Mac App Store](https://apps.apple.com/us/app/apple-configurator-2/id1037126344) | Apple's official profile creation tool — useful for building and analyzing configuration profiles. |
| **MobSF** | [GitHub](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | Analyzes `.mobileconfig` files and configuration profiles embedded in iOS enterprise apps. |

---

## 11. CTF Challenges & Writeups

### Challenge Platforms

| Platform | Link | Platform | Skill Level | Notes |
|----------|------|----------|-------------|-------|
| **OWASP UnCrackable Apps** | [MASTG Resources](https://mas.owasp.org/MASTG/) | Android / iOS | `[Intermediate–Advanced]` | OWASP-maintained crackmes — root/jailbreak bypass, anti-tampering, and obfuscation challenges. The canonical skill-building series for Android and iOS RE. |
| **InjuredAndroid** | [GitHub](https://github.com/B3nac/InjuredAndroid) | Android | `[Beginner–Intermediate]` | 18 CTF-style flags covering exported activities, deeplinks, Firebase misconfigs, Frida bypass, native analysis, and more. Straightforward local setup. |
| **DVIA-v2** | [GitHub](https://github.com/prateek147/DVIA-v2) | iOS | `[Intermediate]` | Damn Vulnerable iOS App v2 — jailbreak detection bypass, anti-debugging, runtime manipulation, insecure storage, and keychain challenges. |
| **Hacker101 CTF** | [ctf.hacker101.com](https://ctf.hacker101.com/) | Cross-Platform | `[Beginner–Intermediate]` | Free CTF with mobile challenges; completing challenges earns HackerOne private program invitations. |
| **HackTheBox** | [hackthebox.com](https://www.hackthebox.com/) | Android | `[Intermediate–Advanced]` | Growing Android reversing and forensics challenge catalog — both competitive and practice modes. |
| **CTFtime** | [ctftime.org](https://ctftime.org/) | Cross-Platform | All levels | Aggregates upcoming and past CTF events; search archived writeups for mobile-tagged challenges. |
| **Google CTF** | [capturetheflag.withgoogle.com](https://capturetheflag.withgoogle.com/) | Android | `[Advanced]` | Google's annual CTF historically includes Android RE and exploitation challenges; archives freely accessible. |

### 8kSec Battlegrounds `[Android / iOS]` — FREE

Free hands-on mobile security challenges:

| Challenge | Platform | Link |
|-----------|----------|------|
| Android Application Exploitation Challenges | Android | [academy.8ksec.io](https://academy.8ksec.io/course/android-application-exploitation-challenges) |
| iOS Application Exploitation Challenges | iOS | [academy.8ksec.io](https://academy.8ksec.io/course/ios-application-exploitation-challenges) |
| ARM Exploitation Challenges | Cross-Platform | [academy.8ksec.io](https://academy.8ksec.io/course/arm-exploitation-challenges) |
| Battlegrounds — Live Challenges | Android & iOS | [8ksec.io/battle](https://8ksec.io/battle/) |

Range: static analysis, dynamic analysis, reverse engineering, and exploitation. Practical
exploitation focus consistent with real assessment scenarios.

### Android CTF Techniques

The standard approach for Android challenges: decompile with jadx, identify the flag validation
logic or protected method, hook or bypass with Frida.

**Common techniques tested:**

- **Hardcoded credential extraction** — jadx string search; `apkleaks` for automated extraction.

- **Obfuscated string decryption** — locate the decrypt function, hook its return value with Frida.

- **Custom authentication bypass** — patch the conditional in Smali (apktool + rebuild) or hook
  the method return value at runtime.
- **Native library RE** — `lib/arm64-v8a/*.so` analyzed in Ghidra or IDA; JNI function naming
  follows `Java_<package>_<class>_<method>` convention.
- **Root/emulator detection bypass** — [Advanced Frida Usage Part 5: Advanced Root Detection & Bypass](https://8ksec.io/advanced-root-detection-bypass-techniques/); Frida scripts to hook detection APIs.

- **Anti-debugging** — `ptrace(PTRACE_TRACEME)` self-attachment trick; hook `ptrace` syscall via Frida.

**Writeup archives:**
- [CTFtime Writeups](https://ctftime.org/writeups) — community writeups filterable by challenge name; search "android" or specific challenge name.

- [HackerOne Hacktivity](https://hackerone.com/hacktivity) — real disclosed bug reports; useful for methodology beyond CTF scenarios.

### iOS CTF Techniques

**Common techniques tested:**

- **IPA decryption** — decrypt App Store IPAs on jailbroken device with
  [bagbak](https://github.com/ChiChou/bagbak) or [frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump).
- **Class and method enumeration** — `frida-ps -Ua` to list apps; `ObjC.classes` in Frida REPL;
  [dsdump](https://github.com/DerekSelander/dsdump) for Swift class structures.
- **SSL pinning bypass** — [Objection](https://github.com/sensepost/objection) `ios sslpinning disable`;
  manual Frida hook on `SecTrustEvaluateWithError` for custom implementations.
- **Keychain extraction** — `objection -g <app> explore`, then `ios keychain dump`.

- **Anti-jailbreak bypass** — Dopamine / palera1n with
  [Shadow](https://github.com/jjolano/shadow) tweaks; Frida hooks on `access()`, `stat()`,
  and file path checks used for detection.
- **Binary patching** — `jtool2` for patching Mach-O entitlements and flags; codesign with
  developer certificate to run modified binary.

**Writeup resources:**
- [Corellium Blog](https://corellium.com/blog) — technical iOS security content with occasional CTF-adjacent writeups.

- [Objective by the Sea Archives](https://objectivebythesea.org/) — iOS security talk slides and papers, many directly applicable to CTF scenarios.

---

## 12. Books


### Free (Complete Resource Available Online)

| Title | Author(s) | Link | Platform | Notes |
|-------|-----------|------|----------|-------|
| **OWASP Mobile Application Security Testing Guide (MASTG)** | OWASP MAS Team | [mas.owasp.org/MASTG](https://mas.owasp.org/MASTG/) | Cross-Platform | The definitive free reference for mobile security testing — maps every test case to MASVS controls. Continuously updated. Read the online version; the GitHub repo includes crackmes and test apps. `[All levels]` |
| **Android Internals Vol. 1: A Confectioner's Cookbook** | Jonathan Levin | [newandroidbook.com](http://newandroidbook.com/) | Android | Deep technical coverage of Android internals — process model, Binder, permissions, and filesystem. Volume I available as free PDF. `[Advanced]` |
| **\*OS Internals Vol. I–III** | Jonathan Levin | [newosxbook.com](http://www.newosxbook.com/) | iOS / macOS | Authoritative reference for Darwin/iOS/macOS kernel internals. Freely downloadable PDFs. The starting point for iOS kernel research. `[Advanced]` |

### Paid

| Title | Author(s) | Publisher | Platform | Notes |
|-------|-----------|-----------|----------|-------|
| **The Mobile Application Hacker's Handbook** | Dominic Chell, Tyrone Erasmus, Shaun Colley, Ollie Whitehouse | Wiley | Cross-Platform | Comprehensive offensive guide — Android and iOS app security, network traffic analysis, and backend testing. Practical and methodology-focused. `[Intermediate]` |
| **Android Security Internals** | Nikolay Elenkov | No Starch Press | Android | Deep coverage of the Android security architecture — permissions system, cryptography implementation, native code security, secure elements, and key attestation. Still the best single reference for Android security internals. `[Advanced]` |
| **iOS Hacker's Handbook** | Charlie Miller, Dion Blazakis, Dino DaiZovi, Stefan Esser, Vincenzo Iozzo, Ralf-Philipp Weinmann | Wiley | iOS | Exploitation-focused — iOS security architecture, vulnerability classes, and attack methodology. iOS 6-era (some chapters dated) but security architecture coverage remains foundational reading. `[Advanced]` |
| **Practical Mobile Forensics** | Heather Mahalik, Rohit Tamma, Satish Bommisetty | Packt | Cross-Platform | Extraction and analysis of evidence from iOS and Android devices — covers physical, logical, and cloud acquisition methods. `[Intermediate]` |
| **Hacking APIs** | Corey Ball | No Starch Press | Cross-Platform | API security testing methodology directly applicable to mobile backend assessment — BOLA, JWT attacks, GraphQL, and mass assignment testing. `[Intermediate]` |
| **The Art of Exploitation, 2nd Ed.** | Jon Erickson | No Starch Press | Cross-Platform | Memory exploitation fundamentals — stack overflows, heap corruption, shellcode, format strings. Not mobile-specific but foundational for understanding native code vulnerabilities encountered in Android/iOS RE. `[Advanced]` |

## 13. Community, Blogs & Researchers

A curated list of researchers, publications, and communities worth following regularly.

---

### Must-Follow Researchers

#### iOS Exploitation & Internals

- **Siguza** ([@s1guza](https://x.com/s1guza)) — [blog.siguza.net](https://blog.siguza.net) — Deep iOS kernel internals, PAC bypass research, exploit development. Technical blog posts are among the most rigorous public iOS research available. [Advanced]

- **Luca Todesco** ([@qwertyoruiopz](https://x.com/qwertyoruiopz)) — iOS jailbreak developer, creator of yalu and other jailbreak toolchains. Foundational iOS exploit researcher. [Advanced]

- **Brandon Azad** ([@_bazad](https://x.com/_bazad)) — [bazad.github.io](https://bazad.github.io) — Former Google Project Zero. iOS and macOS kernel research, tfp0 exploits. [Advanced]

- **Jonathan Levin** ([@Morpheus______](https://x.com/Morpheus______)) — [newandroidbook.com](http://newandroidbook.com) — Author of the Android Internals and *OS Internals book series. Both platforms covered in depth. [Intermediate–Advanced]

- **Ned Williamson** ([@nedwilliamson](https://x.com/nedwilliamson)) — Google Project Zero. iOS, WebKit, browser security research. [Advanced]

- **Samuel Groß (saelo)** ([@5aelo](https://x.com/5aelo)) — Google Project Zero. WebKit JavaScript engine security, iOS exploit chains, browser exploitation. Key author on FORCEDENTRY analysis. [Advanced]

#### Android Exploitation & Research

- **Jann Horn** — Google Project Zero. Linux kernel, Android security research, Spectre/Meltdown. Multiple Android kernel CVEs. [Advanced]

- **Tavis Ormandy** ([@taviso](https://x.com/taviso)) — Google Project Zero. Broad, relentless vulnerability research across platforms. [Advanced]

- **Zinuo Han** ([@R3dF09](https://x.com/R3dF09)) — Android and TrustZone research. Multiple Qualcomm chipset and Android kernel CVEs. [Advanced]

- **Oversecured** ([@Oversecured](https://x.com/Oversecured)) — [oversecured.com/blog](https://blog.oversecured.com) — Android application security research. Deep technical posts on exported component abuse, intent handling bugs, and WebView attack vectors. [Intermediate–Advanced]

#### Mobile Malware & Threat Intelligence

- **Bill Marczak** ([@billmarczak](https://x.com/billmarczak)) — Citizen Lab. Commercial spyware research, Pegasus and Predator technical analysis, surveillance targeting investigations. [Intermediate–Advanced]

- **John Scott-Railton** ([@jsrailton](https://x.com/jsrailton)) — Citizen Lab. Mobile targeting campaigns against journalists and civil society. [Intermediate]

- **Costin Raiu** ([@craiu](https://x.com/craiu)) — Former Kaspersky GReAT Director. Mobile malware, APT research, spyware ecosystem analysis. [Intermediate–Advanced]

- **Alexey Firsh** — Kaspersky GReAT. Operation Triangulation team member. iOS exploit chain forensics. [Advanced]

#### App Security & Penetration Testing

- **NowSecure Research** ([@NowSecure](https://x.com/NowSecure)) — [nowsecure.com/blog](https://www.nowsecure.com/blog/) — Mobile app security research, policy, and tooling. [Intermediate]

#### iOS Jailbreak Community

- **opa334** ([@opa334dev](https://x.com/opa334dev)) — [github.com/opa334](https://github.com/opa334) — Creator of Dopamine jailbreak and TrollStore. Current-generation iOS tooling. [Advanced]

- **palera1n team** ([@palera1n](https://x.com/palera1n)) — Community-maintained iOS 15/16 jailbreak based on checkm8. [Advanced]

---

### Technical Blogs

- **Google Project Zero** — [projectzero.google](https://projectzero.google) — Essential reading for serious security researchers. Covers browser exploitation, mobile kernel, iOS zero-click chains, and broader vulnerability research at a depth not found elsewhere. [Advanced]

- **Citizen Lab** — [citizenlab.ca/category/research](https://citizenlab.ca/category/research/) — Commercial spyware investigations, targeted surveillance documentation. Pegasus, Predator, QuaDream, Paragon Graphite research. [Intermediate]

- **Kaspersky GReAT (Securelist)** — [securelist.com](https://securelist.com) — APT reports, mobile malware analysis, Operation Triangulation. Comprehensive threat actor profiling. [Intermediate]

- **Oversecured Blog** — [oversecured.com/blog](https://blog.oversecured.com) — Android application security deep dives. Practical posts on exported component attacks, privilege escalation, and WebView exploitation. [Intermediate–Advanced]

- **Amnesty International Security Lab** — [securitylab.amnesty.org](https://securitylab.amnesty.org) — MVT development, spyware IOCs, technical analyses of surveillance targeting civil society. [Intermediate]

- **Quarkslab Blog** — [blog.quarkslab.com](https://blog.quarkslab.com) — TrustZone, cryptography, ARM security, reverse engineering research. [Advanced]

- **Synacktiv Blog** — [synacktiv.com/publications](https://www.synacktiv.com/publications) — Mobile exploit research, iOS and Android internals, offensive security tooling. [Advanced]

- **Corellium Blog** — [corellium.com/blog](https://corellium.com/blog) — iOS security research and virtual device platform insights. Useful for jailbreak internals and novel research methods. [Intermediate–Advanced]

- **Azeria Labs** — [azeria-labs.com](https://azeria-labs.com) — ARM assembly and exploitation fundamentals. The canonical reference for anyone starting ARM security research. [Beginner–Intermediate]

- **Siguza's Blog** — [blog.siguza.net](https://blog.siguza.net) — iOS kernel internals, PAC analysis, exploit writeups. Some of the best iOS research published anywhere. [Advanced]

- **8kSec Blog** — [8ksec.io/blog](https://8ksec.io/blog/) — Mobile security research by active practitioners: Frida internals, malware analysis, ARM exploitation, iOS/Android reverse engineering. [Intermediate–Advanced]

- **HackTricks Mobile** — [hacktricks.wiki/en/mobile-pentesting](https://hacktricks.wiki/en/mobile-pentesting/android-app-pentesting/index.html) — Community-maintained mobile pentesting reference. Good for quickly locating testing checklists and attack surface coverage. [Beginner–Intermediate]

- **NowSecure Blog** — [nowsecure.com/blog](https://www.nowsecure.com/blog/) — Mobile app security research, industry analysis, MASTG tooling. [Intermediate]

- **Aleph Research** — [alephsecurity.com](https://alephsecurity.com) — Android and iOS vulnerability research, bootloader exploitation, TEE attacks. [Advanced]

- **NVISO Labs Blog** — [blog.nviso.eu](https://blog.nviso.eu) — Mobile malware analysis, Android/iOS pentesting, Flutter and React Native testing guides. Regular practical content. [Intermediate]

- **Romainthomas.fr** — [romainthomas.fr](https://www.romainthomas.fr) — iOS anti-analysis bypass, Frida and obfuscation research. Author of LIEF and SuspendedProcess tools. [Advanced]

- **erev0s Blog** — [erev0s.com/blog](https://erev0s.com/blog/) — Frida snippets, Android security how-tos, practical mobile pentesting content. [Beginner–Intermediate]

- **highaltitudehacks.com** — [highaltitudehacks.com](https://highaltitudehacks.com) — iOS application security series: runtime manipulation, Touch ID bypass, data storage analysis. [Beginner–Intermediate]

- **muha2xmad (Android malware analysis)** — [muha2xmad.github.io](https://muha2xmad.github.io) — Detailed Android malware analysis write-ups including banking trojans and spyware families. [Intermediate]

---

### Selected High-Value Technical Posts

A curated set of specific posts worth reading — beyond following the blogs above.

**Android Application Security (Oversecured):**
- [20 Security Issues Found in Xiaomi Devices](https://blog.oversecured.com/20-Security-Issues-Found-in-Xiaomi-Devices/) — Vendor-specific vulnerability research methodology applied to a major OEM.

- [Android Security Checklist: Theft of Arbitrary Files](https://blog.oversecured.com/Android-security-checklist-theft-of-arbitrary-files/) — Comprehensive guide to file theft via exported components.

- [Android Security Checklist: WebView](https://blog.oversecured.com/Android-security-checklist-webview/) — All WebView attack vectors with PoC examples.

- [Common Mistakes When Using Permissions in Android](https://blog.oversecured.com/Common-mistakes-when-using-permissions-in-Android/) — Permission model misunderstandings and exploitable patterns.

- [TikTok: Three Persistent Arbitrary Code Executions and One Theft of Arbitrary Files](https://blog.oversecured.com/Oversecured-detects-dangerous-vulnerabilities-in-the-TikTok-Android-app/) — Real-world high-severity app bugs.

- [Use Cryptography in Mobile Apps the Right Way](https://blog.oversecured.com/Use-cryptography-in-mobile-apps-the-right-way/) — Crypto implementation mistakes and correct patterns.

- [Why Dynamic Code Loading Could Be Dangerous for Your Apps: A Google Example](https://blog.oversecured.com/Why-dynamic-code-loading-could-be-dangerous-for-your-apps-a-Google-example/) — Dynamic class loading exploitation.

- [Gaining Access to Arbitrary Content Providers](https://blog.oversecured.com/Gaining-access-to-arbitrary-Content-Providers/) — Content provider access control bypass patterns.

- [Interception of Android Implicit Intents](https://blog.oversecured.com/Interception-of-Android-implicit-intents/) — Intent interception attack surface.

- [Two Weeks of Securing Samsung Devices: Part 1](https://blog.oversecured.com/Two-weeks-of-securing-Samsung-devices-Part-1/) and [Part 2](https://blog.oversecured.com/Two-weeks-of-securing-Samsung-devices-Part-2/) — Vendor customization attack surface.

- [Pending Intents: A Pentester's View](https://valsamaras.medium.com/pending-intents-a-pentesters-view-92f305960f03) — PendingIntent misuse exploitation.

- [Discovering Vendor-Specific Vulnerabilities in Android](https://blog.oversecured.com/Discovering-vendor-specific-vulnerabilities-in-Android/) — Methodology for OEM vulnerability research.

**Android RE & Analysis:**
- [1-click Exploit in South Korea's Biggest Mobile Chat App (KakaoTalk)](https://stulle123.github.io/posts/kakaotalk-account-takeover/) — Real-world mobile app account takeover chain. [Advanced]

- [Reverse Engineering Coin Hunt World's Binary Protocol](https://www.nccgroup.com/research/reverse-engineering-coin-hunt-world-s-binary-protocol/) — NCC Group. Reversing obfuscated binary communication protocol in an Android game. [Intermediate]

- [Step-by-Step Guide to Reverse an APK Protected with DexGuard Using Jadx](https://blog.lexfo.fr/dexguard.html) — Practical DexGuard deobfuscation workflow. [Intermediate]

- [Reversing an Android Sample Which Uses Flutter](https://cryptax.medium.com/reversing-an-android-sample-which-uses-flutter-23c3ff04b847) — Flutter-based Android malware analysis methodology. [Intermediate]

- [Android App Reverse Engineering 101](https://www.ragingrock.com/AndroidAppRE/) — Maddie Stone (Google Project Zero). Free self-paced Android RE course. [Beginner–Intermediate]

- [Exploring Intent-Based Android Security Vulnerabilities on Google Play](https://labs.snyk.io/resources/exploring-android-intent-based-security-vulnerabilities-google-play/) — Snyk 3-part series on intent security at scale.

**iOS Security:**
- [Gotta Catch 'Em All: Frida & Jailbreak Detection in Pokémon GO](https://www.romainthomas.fr/post/21-07-pokemongo-anti-frida-jailbreak-bypass/) — Comprehensive analysis of anti-Frida and jailbreak detection in a production app. Excellent bypass methodology. [Intermediate–Advanced]

- [iOS Pentesting Tools Part 1: App Decryption and class-dump](https://www.allysonomalley.com/2018/08/10/ios-pentesting-tools-part-1-app-decryption-and-class-dump/) — Foundational iOS analysis workflow. [Beginner]

- [Bypass SSL Pinning with LLDB in iOS App](https://itnext.io/bypass-ssl-pinning-with-lldb-in-ios-app-b78f9e7cc9cd) — Low-level SSL bypass without Frida, using only LLDB. [Advanced]


- [Getting Started with iOS Penetration Testing](https://www.yeswehack.com/learn-bug-bounty/getting-started-ios-penetration-testing-part-1) — Comprehensive practical guide. [Beginner]

- [Airdrop: Symbolic Link Following](https://breakpoint.sh/posts/files.app-symbolic-link-following) — iOS AirDrop symlink attack vulnerability. [Advanced]

**Android Malware Analysis:**
- [How to Analyze Mobile Malware: A Cabassous/FluBot Case Study](https://blog.nviso.eu/2021/04/19/how-to-analyze-mobile-malware-a-cabassous-flubot-case-study/) — NVISO Labs. Step-by-step banking trojan analysis methodology. [Intermediate]

- [Technical Analysis of Alien Android Malware](https://muha2xmad.github.io/malware-analysis/alien/) — Detailed Alien banking trojan analysis. [Intermediate]

- [Deep Dive into an Obfuscation-as-a-Service for Android Malware](https://www.stratosphereips.org/blog/2020/12/03/deep-dive-into-an-obfuscation-as-a-service-for-android-malware) — Obfuscation-as-a-service infrastructure analysis. [Advanced]

---

### Communities & Forums

- **XDA Developers** — [xdaforums.com](https://xdaforums.com/) — Long-running Android rooting and modding community. Still the best source for device-specific rooting procedures, bootloader unlock methods, and custom recovery guides. [Beginner–Intermediate]

- **Reddit: r/netsec** — [reddit.com/r/netsec](https://www.reddit.com/r/netsec/) — General security research aggregator. Mobile vulnerability disclosures and write-ups regularly surface here. [Intermediate]

- **Reddit: r/androidroot** — [reddit.com/r/androidroot](https://www.reddit.com/r/androidroot/) — Android rooting community. Useful for device-specific questions and tracking current root method support. [Beginner–Intermediate]

- **Mobile Security Discord Servers** — Search "mobile security CTF" or "iOS jailbreak" on Disboard or Discord discovery. Most active mobile CTF communities operate on Discord rather than public forums. [Intermediate]

---

### Conferences

| Conference | Focus | Cadence | URL |
|-----------|-------|---------|-----|
| [Black Hat](https://blackhat.com) | Broad security (strong mobile and exploit tracks) | Annual: USA (Aug), EU (Dec), Asia (Apr) | blackhat.com |
| [DEF CON](https://defcon.org) | Offensive security across all domains | Annual (Aug, Las Vegas) | defcon.org |
| MOSEC | Mobile security — iOS and Android | On hiatus since 2023 (Shanghai) | `mosec.org` (site SSL expired) |
| [Objective by the Sea](https://objectivebythesea.org) | Apple platform security exclusively | Annual (Oct/Nov) | objectivebythesea.org |
| [OffensiveCon](https://offensivecon.org) | Offensive exploitation — kernel, browser, mobile | Annual (May, Berlin) | offensivecon.org |
| [OOTB / Hack In The Box](https://conference.hitb.org) | Offensive security with mobile tracks | Rebranded from HITB; Jakarta, Bangkok | conference.hitb.org |
| [Hexacon](https://hexacon.fr) | Binary exploitation and low-level research | Annual (Oct, Paris) | hexacon.fr |
| [TROOPERS](https://troopers.de) | Enterprise security, mobile, protocols | Annual (Jun, Heidelberg) | troopers.de |
| TyphoonCon | Offensive security, mobile exploitation | Annual (May, Seoul) | Invite/limited |
| Zer0Con | Cutting-edge iOS and Android exploitation | Annual (Apr, Seoul) | Invite-only (`powerofcommunity.net` — SSL issues) |
| [REcon](https://recon.cx) | Reverse engineering — baseband, firmware, hardware, mobile | Annual (Jun, Montreal) | recon.cx — slides and videos published |
| Power of Community (POC) | Exploitation and RE — browser, mobile, kernel | Annual (Nov, Seoul) | `powerofcommunity.net` (SSL issues) |
| [Chaos Communication Congress (CCC)](https://media.ccc.de) | Broad hacker conference with strong security research track | Annual (Dec, Hamburg) | media.ccc.de — all talks recorded and free |

---

### Podcasts

- **Risky Business** — [risky.biz](https://risky.biz) — Weekly security news with in-depth interviews. Mobile threat disclosures and spyware stories covered regularly. Consistent signal-to-noise ratio. [Intermediate]

- **Darknet Diaries** — [darknetdiaries.com](https://darknetdiaries.com) — Narrative-style episodes on real incidents. Episodes on Operation Triangulation, Pegasus, and NSO Group are worth the time. [Beginner–Intermediate]

- **Security Now** — [grc.com/securitynow.htm](https://grc.com/securitynow.htm) — Long-form technical security discussions. Useful for understanding platform security model changes. [Intermediate]

---

## 14. Threat Intelligence

### Threat Reports & Annual Reviews

- **Google Android Security Bulletins** — [source.android.com/docs/security/bulletin](https://source.android.com/docs/security/bulletin) — Monthly patches with CVE details, affected components, severity ratings, and patch SHAs. Essential for tracking Android system-level vulnerability cadence. [Intermediate]

- **Apple Platform Security Guide** — [support.apple.com/guide/security/welcome/web](https://support.apple.com/guide/security/welcome/web) — Comprehensive Apple security architecture documentation. Updated annually. Covers Secure Enclave, secure boot, code signing, sandboxing, Face ID/Touch ID, Pointer Authentication. [Intermediate–Advanced]

- **Zimperium Global Mobile Threat Report** — [zimperium.com/global-mobile-threat-report](https://zimperium.com/global-mobile-threat-report/) — Annual mobile threat landscape statistics: malware family counts, risky app prevalence, enterprise mobile breach data. [Intermediate]

- **Lookout Mobile Threat Report** — [lookout.com/resources](https://www.lookout.com/resources) — Annual enterprise mobile threat analysis. Spyware, credential theft, phishing vector data. [Intermediate]

- **Citizen Lab Reports** — [citizenlab.ca/category/research/tools-resources](https://citizenlab.ca/category/research/tools-resources/) — Commercial surveillance and spyware technical investigations. The primary public source for Pegasus, Predator, QuaDream, and Paragon Graphite research. [Intermediate–Advanced]

- **Amnesty Tech Investigations** — [securitylab.amnesty.org/latest](https://securitylab.amnesty.org/latest/) — Spyware forensic analyses with published IOCs. Methodologically rigorous, reproducible findings. [Intermediate–Advanced]

- **Verizon DBIR** — [verizon.com/business/resources/reports/dbir](https://www.verizon.com/business/resources/reports/dbir/) — Annual breach report. Sections on mobile-attributed incidents useful for enterprise risk context. [Intermediate]

- **Kaspersky Mobile Threat Report** — [securelist.com/mobile-threat-report](https://securelist.com/mobile-threat-report-2025/119076/) — Annual analysis of mobile malware trends, top malware families by volume, and attacker tactics. 2025 edition covers Triada, Mamont, SpyLoan, and mobile banking trojan resurgence. [Intermediate]

- **Google Zero-Day Exploitation Annual Analysis** — [cloud.google.com/blog/topics/threat-intelligence](https://cloud.google.com/blog/topics/threat-intelligence/2025-zero-day-review) — Google Threat Intelligence Group annual review tracking all zero-day exploitations in the wild. Mobile platform breakdown (iOS vs. Android) covers both vendor and commercial spyware attributions. 2025 report documents iOS as the most-exploited mobile platform. [Intermediate–Advanced]

---

### CVE Databases & Advisories

- **Android Security Bulletins** — [source.android.com/docs/security/bulletin](https://source.android.com/docs/security/bulletin) — Official monthly Android patches. Filter by severity (Critical/High) and component (Kernel, Qualcomm, Media Framework) to track relevant research areas. [Intermediate]

- **Apple Security Updates** — [support.apple.com/en-us/HT201222](https://support.apple.com/en-us/HT201222) — All Apple CVEs with affected OS versions and patch information. [Intermediate]

- **NVD (National Vulnerability Database)** — [nvd.nist.gov](https://nvd.nist.gov) — NIST's CVE database with CVSS scores, CWE mappings, and reference links. Useful for structured vulnerability research and portfolio tracking. [Intermediate]

- **Qualcomm Security Bulletins** — [docs.qualcomm.com/product/publicresources/securitybulletin](https://docs.qualcomm.com/product/publicresources/securitybulletin/) — Quarterly bulletins covering Snapdragon SoC and modem vulnerabilities. Critical for Pixel/flagship Android research given Qualcomm chipset prevalence. [Advanced]

- **Samsung Security Updates** — [security.samsungmobile.com/securityUpdate.smsb](https://security.samsungmobile.com/securityUpdate.smsb) — Samsung-specific Android vulnerabilities including Knox and Samsung-developed components. [Intermediate]

- **Pixel Update Bulletin** — [source.android.com/docs/security/bulletin/pixel](https://source.android.com/docs/security/bulletin/pixel) — Google Pixel-specific patches, often ahead of the broader Android bulletin. Includes Qualcomm components patched by Google. [Intermediate]

---

### Mobile Malware Databases

- **VirusTotal** — [virustotal.com](https://www.virustotal.com) — Multi-engine scanning with behavioral analysis. Android APK and iOS IPA upload supported. Essential first triage step for suspected malicious apps. [Beginner–Intermediate]

- **Koodous** — [koodous.com](https://koodous.com) — Android-specific malware repository and community analysis platform. Supports YARA rules for sample hunting. Useful for tracking malware family evolution. [Intermediate]

- **MalwareBazaar** — [bazaar.abuse.ch](https://bazaar.abuse.ch) — Open malware sample repository including mobile samples. Supports tag-based filtering and bulk download. [Intermediate]

- **Contagio Mobile Malware Mini-Dump** — [contagiominidump.blogspot.com](https://contagiominidump.blogspot.com) — Mobile malware sample archive maintained for research use. Older samples, but historically important families represented. [Intermediate–Advanced]

---

### APT Groups Targeting Mobile

| Group | Attribution | Primary Targets | Notable Operations |
|-------|------------|----------------|-------------------|
| NSO Group (Pegasus) | Israeli | Journalists, activists, heads of state | FORCEDENTRY (CVE-2021-30860), BLASTPASS (CVE-2023-41064) |
| Intellexa (Predator) | European consortium | Civil society, politicians | "Aladdin" silent infection via malicious ads (2025); zero-click iOS/Android chains |
| QuaDream / Reign | Israeli | iOS, zero-click iMessage delivery | 2023 Citizen Lab disclosure; company disbanded |
| Paragon (Graphite) | Israeli | Journalists, civil society via iMessage | March 2025 Citizen Lab; CVE-2025-43200 zero-click (patched iOS 18.3.1) |
| RCS Lab (Hermit) | Italian | Android and iOS targeted surveillance | Italy, Kazakhstan campaigns |
| Lazarus Group | North Korean (attributed) | Financial sector mobile apps, crypto wallets | Watering hole, trojanized apps, BadBazaar co-deployment |
| Gamaredon / Primitive Bear | Russian (attributed) | Russian-speaking post-Soviet states | BoneSpy + PlainGnome Android-only spyware (Dec 2024) |
| ScarCruft / APT37 | North Korean (attributed) | South Korean targets, Uyghur communities | KoSpy Android surveillanceware (2025) |
| APT-C-23 | Middle Eastern (attributed) | Palestinian journalists, Android | Custom Android RAT campaigns |
| FancyBear / APT28 | Russian (attributed) | Government officials, Android | Spearphishing, mobile credential theft |
| MuddyWater / APT34 (Iran) | Iranian MOIS (attributed) | Middle East government, telecom | DCHSpy Android surveillanceware |

---

## 15. Courses & Certifications


---

### Mobile Security Courses

#### Free

- **OWASP MASTG** — [mas.owasp.org/MASTG](https://mas.owasp.org/MASTG/) — The definitive free hands-on testing guide. Treat it as a self-study curriculum: work through the atomic tests systematically by control category. Covers Android and iOS in full depth. [Intermediate]

- **HackTricks Mobile Pentesting** — [hacktricks.wiki/en/mobile-pentesting](https://hacktricks.wiki/en/mobile-pentesting/android-app-pentesting/index.html) — Comprehensive pentesting reference covering Android and iOS. Good for quickly finding testing checklists and technique explanations. [Beginner–Intermediate]

- **Corellium Security Tutorials** — [corellium.com/blog](https://corellium.com/blog) — Free iOS and Android security tutorials covering jailbreak mechanics, research tooling, and platform internals. [Intermediate]

- **8kSec Free Labs — Battlegrounds** — [8ksec.io/battle](https://8ksec.io/battle/) — Free practical exploitation challenges: Android component attacks, iOS runtime analysis, ARM exploitation. New challenges added regularly. [Intermediate–Advanced]

#### Paid

- **8kSec — Practical Mobile Application Exploitation (CMSE)** — [academy.8ksec.io](https://academy.8ksec.io) — Lab-driven Android and iOS pentesting from environment setup through Frida automation: static/dynamic analysis, reverse engineering, ARM64, and securing apps. Awards the Certified Mobile Security Engineer (CMSE) credential. [Intermediate–Advanced]

- **8kSec — Offensive Mobile Reversing and Exploitation (OMSE)** — [academy.8ksec.io](https://academy.8ksec.io) — Advanced course covering iOS kernel internals (XNU, SPTM, TXM, PAC, PPL), Android kernel exploitation, JNI fuzzing, advanced Frida, and mobile malware analysis including crypto wallet stealers. Awards the Offensive Mobile Security Expert (OMSE) credential. [Advanced]

- **INE / eLearnSecurity eMAPT** — [ine.com](https://ine.com) — Enterprise Mobile Application Penetration Tester course. Practical, hands-on lab environment with real APK/IPA targets. Pairs with the eMAPT certification.

- **TCM Security: Practical Mobile Security** — [tcm-sec.com/academy](https://tcm-sec.com/academy/) — Practical-focused Android and iOS testing course covering common attack classes, Frida, and Burp Suite integration.

- **NowSecure Academy** — [nowsecure.com/academy](https://www.nowsecure.com/products/nowsecure-academy-mobile-appsec-training/) — Enterprise mobile security training aligned with OWASP MASVS. Tool-agnostic methodology focus.

- **PortSwigger Web Security Academy** **[FREE labs / PAID Pro]** — [portswigger.net/web-security](https://portswigger.net/web-security) — API security labs directly applicable to mobile backend testing. JWT attacks, IDOR, API authentication flaws. Core labs free.

---

### Certifications

| Certification | Issuer | Focus | Format |
|--------------|--------|-------|--------|
| [CMSE](https://academy.8ksec.io) | 8kSec | Android and iOS app security — static, dynamic, RE, Frida | Practical exam |
| [OMSE](https://academy.8ksec.io) | 8kSec | Advanced mobile RE and exploitation — kernel, TEE, malware | Practical exam |
| [eMAPT](https://my.ine.com/certifications/) | eLearnSecurity / INE | Mobile app pentesting — Android and iOS | Practical (real-app exam) |
| [GMOB](https://www.giac.org/certifications/mobile-device-security-analyst-gmob/) | GIAC | Mobile device and app security | Proctored exam |
| [GWAPT](https://www.giac.org/certifications/web-application-penetration-tester-gwapt/) | GIAC | Web/mobile application testing | Proctored exam |
| [OSCP](https://www.offsec.com/courses/pen-200/) | OffSec | General pentesting with mobile modules | 24-hr practical exam |
| [CEH](https://www.eccouncil.org/programs/certified-ethical-hacker-ceh/) | EC-Council | Broad ethical hacking (mobile included) | Multiple choice |


---

## 16. Standards & Frameworks

### OWASP Mobile

- **OWASP MASVS (Mobile Application Security Verification Standard) v2.1** — [mas.owasp.org/MASVS](https://mas.owasp.org/MASVS/) — Baseline standard for mobile app security requirements. January 2024 update added **MASVS-PRIVACY** as a new control group and removed verification levels (L1/L2/R) in favor of MAS Testing Profiles. Widely used as a contract requirement and audit baseline. [Intermediate]

- **OWASP MASTG (Mobile Application Security Testing Guide)** — [mas.owasp.org/MASTG](https://mas.owasp.org/MASTG/) — Comprehensive testing methodology guide. 1:1 mapping to MASVS controls. Includes atomic tests that can be executed as point-in-time checklists. The reference guide for MASVS assessments. [Intermediate]

- **MASWE (Mobile App Security Weakness Enumeration)** — [mas.owasp.org](https://mas.owasp.org) — Introduced July 2024. New granular layer between MASVS controls and MASTG tests. Enumerates specific weaknesses (e.g., "insecure random number generation") that map to both MASVS requirements and MASTG test procedures. Improves consistency in assessment reporting. [Intermediate]

- **OWASP Mobile Top 10 (2024)** — [owasp.org/www-project-mobile-top-10](https://owasp.org/www-project-mobile-top-10/) — First update since 2016. New categories include: M1 Improper Credential Usage, M2 Inadequate Supply Chain Security, M6 Inadequate Privacy Controls, M8 Security Misconfiguration. Removes older categories that merged or became implicit. [Beginner–Intermediate]

- **OWASP MAS Checklist** — [mas.owasp.org/checklists](https://mas.owasp.org/checklists/) — Downloadable MASVS checklist in Excel and Markdown format. Use as the primary deliverable template for MASVS assessments. [Intermediate]

- **How to Secure Flutter Applications Against the OWASP Mobile Top 10** — [8ksec.io/how-to-secure-flutter-applications-against-the-owasp-mobile-top-10](https://8ksec.io/securing-flutter-applications/) — 8kSec blog applying OWASP controls to Flutter-specific attack surfaces. [Intermediate]

### MITRE ATT&CK Mobile

- **MITRE ATT&CK for Mobile** — [attack.mitre.org/matrices/mobile](https://attack.mitre.org/matrices/mobile/) — Adversary tactics, techniques, and procedures for Android and iOS. **v17 (April 2025):** 12 tactics, 75 techniques, 46 sub-techniques, 15 groups, 118 software entries. **v18 (October 2025):** Added Wi-Fi discovery and wireless sniffing behaviors; un-deprecated "Abuse Accessibility Features". Useful for threat modeling, red team scoping, and detection engineering. [Intermediate–Advanced]

- **MITRE ATT&CK Navigator** — [mitre-attack.github.io/attack-navigator](https://mitre-attack.github.io/attack-navigator/) — Interactive matrix visualization. Create coverage heatmaps for mobile threat models and red team scope definitions. [Intermediate]

### NIST

- **NIST SP 800-163 Rev. 1** — [csrc.nist.gov/publications/detail/sp/800-163/rev-1/final](https://csrc.nist.gov/publications/detail/sp/800-163/rev-1/final) — Vetting the Security of Mobile Applications. Framework for organizations evaluating mobile app security risk before deployment. [Intermediate]

- **NIST SP 800-124 Rev. 2** — [csrc.nist.gov/publications/detail/sp/800-124/rev-2/final](https://csrc.nist.gov/publications/detail/sp/800-124/rev-2/final) — Guidelines for managing mobile device security in enterprise environments. MDM policy, enrollment models, device lifecycle. [Intermediate]

### CIS Benchmarks

- **CIS Android Benchmark** — [cisecurity.org/benchmark/google_android](https://www.cisecurity.org/benchmark/google_android) — Device configuration hardening guidance for Android. Useful baseline for enterprise device policy. [Intermediate]

- **CIS Apple iOS/iPadOS Benchmark** — [cisecurity.org/benchmark/apple_ios](https://www.cisecurity.org/benchmark/apple_ios) — Device hardening configuration for iOS. MDM-deployable settings. [Intermediate]

### Other Standards

- **GSMA Mobile Security Guidelines** — [gsma.com/security](https://www.gsma.com/security/) — Telecom-focused mobile security standards. Relevant for baseband research, network interface security, and operator-side mobile threat modeling. [Advanced]

- **PCI MPoC (Mobile Payments on COTS)** — [pcisecuritystandards.org](https://www.pcisecuritystandards.org) — Standard for accepting payment card data on commercial off-the-shelf mobile devices. Relevant for fintech and payment app security. [Intermediate]

---

## 17. Official Documentation

### Android

- **Android Security Overview** — [source.android.com/docs/security](https://source.android.com/docs/security) — AOSP security documentation. Covers the full Android security architecture: application sandbox, permission model, SELinux policy, verified boot, and hardware-backed security. [Intermediate]

- **Android Developer Security Docs** — [developer.android.com/topic/security](https://developer.android.com/topic/security) — Secure coding guidelines. Covers data storage, network security config, cryptography APIs, and permissions. The starting reference for Android app hardening. [Beginner–Intermediate]

- **Android Keystore System** — [developer.android.com/training/articles/keystore](https://developer.android.com/training/articles/keystore) — Hardware-backed key storage and cryptographic operations documentation. Essential for understanding secure storage design and its limitations. [Intermediate]

- **Android SELinux Documentation** — [source.android.com/docs/security/features/selinux](https://source.android.com/docs/security/features/selinux) — SELinux policy design, audit log interpretation, and custom policy development for Android. [Advanced]

- **Android Security Bulletins** — [source.android.com/docs/security/bulletin](https://source.android.com/docs/security/bulletin) — Monthly vulnerability disclosure and patch documentation. [Intermediate]

- **AOSP Source Code** — [source.android.com](https://source.android.com) — Full Android Open Source Project source. Essential for patch diffing, understanding internal APIs, and kernel vulnerability research. [Advanced]

- **Blueprint of Android Activity Lifecycle** — [8ksec.io/blueprint-of-android-activity-lifecycle](https://8ksec.io/a-blueprint-of-android-activity-lifecycle/) — 8kSec blog covering activity lifecycle from a security perspective: data leakage across lifecycle transitions, intent handling, and state management vulnerabilities. [Intermediate]

### iOS & Apple

- **Apple Platform Security Guide** — [support.apple.com/guide/security/welcome/web](https://support.apple.com/guide/security/welcome/web) — Comprehensive Apple security architecture documentation, updated annually. Covers Secure Enclave, Pointer Authentication (PAC), secure boot chain, code signing, Sandboxing, Face ID/Touch ID, iCloud security, and device management. The canonical reference for understanding iOS security design. [Intermediate–Advanced]

- **iOS Security Framework API Docs** — [developer.apple.com/documentation/security](https://developer.apple.com/documentation/security) — Developer API documentation for the iOS Security framework: keychain, certificates, cryptographic operations, code signing. [Intermediate]

- **iOS Entitlements Reference** — [developer.apple.com/documentation/bundleresources/entitlements](https://developer.apple.com/documentation/bundleresources/entitlements) — Full list of iOS entitlements and their security implications. Essential for iOS app privilege analysis and jailbreak detection bypass research. [Intermediate–Advanced]

- **Apple Security Advisories** — [support.apple.com/en-us/HT201222](https://support.apple.com/en-us/HT201222) — All Apple CVEs with patch versions and, where disclosed, basic technical summaries. [Intermediate]

- **ipsw.me** — [ipsw.me](https://ipsw.me) — iOS, iPadOS, and macOS firmware download archive. Maintained archive of signed and unsigned IPSWs by device and version. Essential for version-specific research and jailbreak development. [Advanced]

- **Reading iOS Sandbox Profiles** — [8ksec.io/reading-ios-sandbox-profiles](https://8ksec.io/reading-ios-sandbox-profiles/) — Guide to parsing and interpreting iOS sandbox profile compilation artifacts: tooling, schema structure, and what restrictions matter for exploit research. [Intermediate–Advanced]

### Cross-Platform Framework Security Docs

- **Flutter Security Documentation** — [docs.flutter.dev/security](https://docs.flutter.dev/security) — Official Flutter security guidelines. Note: Flutter's proxy behavior and certificate pinning implementation require specific tooling workarounds for traffic interception — see Section 4. [Intermediate]

- **React Native Security** — [reactnative.dev/docs/security](https://reactnative.dev/docs/security) — Official React Native security documentation. Covers secure storage, network security, and the JS bridge attack surface. [Intermediate]

---

## 18. Regulatory & Compliance

### App Store Policies

- **Google Play Developer Policy Center** — [play.google.com/about/developer-content-policy](https://play.google.com/about/developer-content-policy/) — Policies governing what Android apps can do on the Play Store. Relevant to security researchers: restrictions on apps that monitor other apps without consent, accessibility service abuse, and ad fraud tool detection. [Intermediate]

- **Apple App Store Review Guidelines** — [developer.apple.com/app-store/review/guidelines](https://developer.apple.com/app-store/review/guidelines/) — iOS app security and privacy requirements enforced at review. Covers data collection disclosure, encryption export compliance, and restrictions on private API use. [Intermediate]

### Privacy Regulations

| Regulation | Jurisdiction | Key Mobile Implications |
|-----------|-------------|------------------------|
| GDPR | EU / EEA | User consent for data collection, right to erasure, data minimization, breach notification within 72 hours |
| CCPA / CPRA | California, USA | Opt-out of data sale, privacy notice at collection, consumer data rights |
| PDPA | Thailand | Consent-based data processing, applies to apps with Thai user data |
| LGPD | Brazil | Data protection framework, consent and legitimate interest as legal bases |
| PIPEDA | Canada | Personal information protection for commercial activity |
| APP (Privacy Act) | Australia | 13 Australian Privacy Principles, applies to mobile apps handling personal information |

- **ICO Guidance on Apps** — [ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/) — UK ICO practical guidance for mobile app developers on GDPR obligations. [Intermediate]

- **Apple App Tracking Transparency (ATT)** `[iOS]` — Introduced iOS 14.5. Requires explicit user permission prompt before cross-app tracking. Significant impact on ad-tech; also a useful model for consent design in security-sensitive apps. [Intermediate]

- **Android Privacy Dashboard** (Android 12+) `[Android]` — [developer.android.com/training/permissions/explaining-access](https://developer.android.com/training/permissions/explaining-access) — Timeline view of sensitive permission access. Security relevance: helps users and analysts detect abnormal background data access patterns. [Intermediate]

### Payment & Financial Standards

- **PCI DSS v4.0** — [pcisecuritystandards.org](https://www.pcisecuritystandards.org) — Payment Card Industry Data Security Standard. Requirement 6 covers secure software development and applies to any mobile app that processes, stores, or transmits cardholder data. [Intermediate]

- **PCI MPoC** — Standard for accepting payments on commercial off-the-shelf mobile devices (smartphones, tablets). Defines software and hardware requirements for mobile POS implementations. [Intermediate]

- **EMVCo** — [emvco.com](https://www.emvco.com) — Payment tokenization and NFC payment security standards. Relevant for mobile wallet and contactless payment implementation security. [Intermediate]

### Healthcare

- **HIPAA Security Rule** — Mobile health app considerations: any app that handles Protected Health Information (PHI) must implement encryption in transit and at rest, access controls, and audit logging. The Security Rule does not specify technology — it specifies outcomes. [Intermediate]

- **NIST SP 800-163** — Framework for federal and healthcare organizations vetting mobile app security before deployment approval. [Intermediate]

---

## 19. Bug Bounty Programs

### Vendor Programs

| Company | Scope | Max Payout | Program URL |
|---------|-------|-----------|-------------|
| Google (Android & Pixel) | Android OS, Pixel firmware, Play Store apps | $1,000,000+ (full exploit chain with persistence) | [bughunters.google.com](https://bughunters.google.com/about/rules/android-friends/6171833274204160/android-and-google-devices-security-reward-program-rules) |
| Apple | iOS kernel, PAC bypass, iCloud | $1,000,000 (zero-click kernel with full persistence) | [security.apple.com/bounty](https://security.apple.com/bounty/) |
| Samsung | Knox, Samsung Pay, Galaxy firmware | $1,000,000 (full exploit chain on flagship device) | [samsung.com/securityresponsecenter](https://www.samsung.com/us/support/securityresponsecenter/) |
| Microsoft | Defender for Android, Microsoft mobile apps | $30,000+ | [microsoft.com/en-us/msrc/bounty](https://www.microsoft.com/en-us/msrc/bounty) |
| Meta | Facebook, Instagram, WhatsApp mobile | $500,000+ (critical account takeover) | [bugbounty.meta.com](https://bugbounty.meta.com) |

All vendor programs require responsible disclosure of previously unreported vulnerabilities. Intentional exploitation of production user data is out of scope for all programs.

### Independent Platforms

**HackerOne** — [hackerone.com/bug-bounty-programs](https://hackerone.com/bug-bounty-programs) — Hosts hundreds of mobile app programs. Filter by "Android", "iOS", or "mobile" in the scope description. Many fintech, healthcare, and consumer app programs active.

**Bugcrowd** — [bugcrowd.com/programs](https://bugcrowd.com/programs) — Similar scope to HackerOne. Hosts mobile-focused programs from enterprise and consumer companies.

**Intigriti** — [intigriti.com](https://www.intigriti.com) — European-focused bug bounty platform with mobile app programs. Strong presence among EU-based companies with GDPR compliance obligations.

### Market Context

For reference on commercial zero-day valuations (not a standard bounty program):

**Zerodium** — [zerodium.com](https://zerodium.com/) — Publishes a public acquisition price list for zero-day vulnerabilities. iOS zero-click full chain with persistence: up to $2,500,000. Android equivalent: up to $1,500,000. Useful for understanding the economic value of mobile exploits and the investment attackers are willing to make. Do not submit vulnerabilities here without understanding the legal and ethical implications of your jurisdiction.

---

## 20. Free Practical Challenges

All resources in this section are free. No paid content.

### Android Challenges

- **InjuredAndroid** — [github.com/B3nac/InjuredAndroid](https://github.com/B3nac/InjuredAndroid) `[Android]` — 18 flag-based challenges covering exported activities, intent sniffing, Firebase misconfiguration, and native library attacks. Local setup required. [Beginner–Intermediate]

- **AndroGoat** — [github.com/satishpatnayak/AndroGoat](https://github.com/satishpatnayak/AndroGoat) `[Android]` — Intentionally vulnerable Android app covering OWASP Mobile Top 10 categories. Structured for systematic testing practice. [Beginner–Intermediate]

- **DIVA (Damn Insecure and Vulnerable App)** — [github.com/payatu/diva-android](https://github.com/payatu/diva-android) `[Android]` — 13 challenges covering insecure logging, hardcoded secrets, insecure data storage, and input validation flaws. Classic starting point. [Beginner]

- **Android Application Exploitation Challenges** — [academy.8ksec.io](https://academy.8ksec.io/course/android-application-exploitation-challenges) `[Android]` — Challenge set covering exported component abuse, intent hijacking, WebView exploitation, and content provider attacks. Free. [Intermediate]

- **Hacking Android Games** — [8ksec.io/hacking-android-games](https://8ksec.io/hacking-android-games/) `[Android]` — Memory scanning, Frida game hacking, and native library instrumentation techniques through practical game examples. [Intermediate]

### iOS Challenges

- **DVIA v2 (Damn Vulnerable iOS Application)** — [github.com/prateek147/DVIA-v2](https://github.com/prateek147/DVIA-v2) `[iOS]` — Intentionally vulnerable iOS app with challenges across all OWASP Mobile Top 10 categories. Requires a jailbroken device or Corellium. [Intermediate]

- **iGoat-Swift** — [github.com/OWASP/iGoat-Swift](https://github.com/OWASP/iGoat-Swift) `[iOS]` — OWASP-maintained vulnerable iOS app in Swift. 21 exercises covering keychain misuse, insecure data storage, authentication bypass, and binary protections. [Beginner–Intermediate]

- **iOS Application Exploitation Challenges** — [academy.8ksec.io](https://academy.8ksec.io/course/ios-application-exploitation-challenges) `[iOS]` — Challenge set covering jailbreak detection bypass, Frida-based runtime analysis, and Objective-C/Swift hooking. Free. [Intermediate]

### CTF Platforms

- **HackTheBox** — [hackthebox.com](https://www.hackthebox.com) `[Android]` `[iOS]` — Free tier includes mobile challenges (Android APK reversing, iOS app analysis). Pro subscription unlocks the full library. [Intermediate–Advanced]

- **CTFTime** — [ctftime.org](https://ctftime.org) — Aggregates all active and past CTFs worldwide. Filter by "mobile" category or search write-up archives for past Android/iOS challenge solutions. [Intermediate]

- **Google CTF Archive** — [capturetheflag.withgoogle.com](https://capturetheflag.withgoogle.com) — Historical Google CTF challenges archived and playable. Android APK reversing challenges appear regularly. [Intermediate–Advanced]

- **8kSec Battlegrounds** — [8ksec.io/battle](https://8ksec.io/battle/) `[Android]` `[iOS]` — Live exploitation challenges covering Android component attacks, iOS runtime analysis, and ARM exploitation. Free. [Intermediate–Advanced]

### ARM Exploitation

- **pwn.college** — [pwn.college](https://pwn.college) `[ARM]` — Free browser-based ARM exploitation course. Covers shellcode, ROP, heap exploitation in a guided environment. Directly applicable to Android native code and kernel exploit development. [Intermediate–Advanced]

- **Azeria Labs ARM Exploitation** — [azeria-labs.com/writing-arm-assembly-part-1](https://azeria-labs.com/writing-arm-assembly-part-1/) `[ARM]` — Free ARM assembly and exploitation tutorial series. The standard starting point for anyone new to ARM security research. [Beginner–Intermediate]

- **ARM Exploitation Challenges** — [academy.8ksec.io](https://academy.8ksec.io/course/arm-exploitation-challenges) `[ARM]` — Challenge set covering stack smashing, heap corruption, and ROP chain building. Free. [Intermediate–Advanced]
