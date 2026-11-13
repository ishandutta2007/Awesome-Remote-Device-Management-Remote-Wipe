# Awesome-Remote-Device-Management-Remote-Wipe

# Awesome Remote Device Management & Remote Wipe

> A curated list of top Remote Device Management and Remote Wipe platforms — both SaaS/Hosted commercial UEM/MDM solutions and open-source alternatives for securing, managing, and remotely wiping endpoints across a distributed workforce.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Microsoft Intune](#microsoft-intune)
  - [VMware Workspace ONE (Omnissa)](#vmware-workspace-one-omnissa)
  - [Jamf Pro](#jamf-pro)
  - [ManageEngine Endpoint Central](#manageengine-endpoint-central)
  - [Druva Endpoint Data Protection](#druva-endpoint-data-protection)
- [Open-Source Software](#-open-source-software)
  - [OpenUEM](#openuem)
  - [linDM](#lindm)
  - [SVT MDM](#svt-mdm)
  - [Headwind MDM](#headwind-mdm)
  - [Sextant (DAWO)](#sextant-dawo)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Remote Device Management and Remote Wipe platforms. These are not open-source but are included as reference points for feature comparison.

### Microsoft Intune
**Type:** Unified endpoint management with factory reset wipe  
Intune provides the **Wipe** action to factory reset a device, removing all personal and organizational data, apps, and configurations [citation:2]. It supports Android Enterprise (COSU, COBO, COPE), AOSP, ChromeOS, iOS/iPadOS, macOS, tvOS, visionOS, and Windows [citation:2]. Platform-specific behaviors include preserving eSIMs by default on Android Enterprise devices, and **Zebra Android devices only removing corporate data** rather than performing a full factory reset [citation:2]. A tenant can submit up to 500 Wipe actions per day, and macOS devices without a T2 chip require a 6-digit Recovery PIN to reinstall the OS [citation:2].  
**Best for:** Organizations already standardized on Microsoft 365 wanting native, platform-aware remote wipe with granular control over what is preserved.

### VMware Workspace ONE (Omnissa)
**Type:** UEM with enterprise wipe and full device wipe  
Workspace ONE offers two distinct wipe modes: **Enterprise Wipe** removes all business data without touching personal data, while **Device Wipe** performs a full factory reset [citation:3][citation:12]. Enterprise Wipe is only supported on devices enrolled through typical enrollment, not standalone enrollment, and it removes all personal accounts from Workspace ONE Boxer, requiring end users to re-authenticate [citation:3][citation:12]. **Critical caveat:** If compliance policies are enabled for standalone-enrolled devices, the UEM console may report them as violating policies and trigger unintended device wipes [citation:3].  
**Best for:** Enterprises needing a clear separation between corporate-data-only wipe and full device wipe, accepting the enrollment-method constraints.

### Jamf Pro
**Type:** Apple-focused MDM with remote wipe for Mac and iOS  
Jamf Pro provides a **Wipe Computer** remote command that permanently erases all data, requiring a passcode to reinstall the OS . For mobile devices, an **Erase Mobile Device** action sends a remote wipe command that erases all data on next check-in [citation:13]. Both actions are irreversible and should be guarded with manual approval before execution in automated workflows [citation:13].  
**Best for:** Apple-centric organizations needing deep macOS and iOS device management with native remote wipe capabilities.

### ManageEngine Endpoint Central
**Type:** UEM with complete and corporate wipe options  
Endpoint Central distinguishes between **Complete Wipe** (factory reset, making the device brand-new) and **Corporate Wipe** (removing only corporate data) [citation:5]. The mobile app allows IT teams to lock, wipe, or restart devices remotely, alongside inventory actions like remote alarm, clear/reset passcode, and lost mode [citation:14]. This dual-wipe model is useful for BYOD scenarios where personal data must be preserved.  
**Best for:** Organizations needing both full factory reset and corporate-only wipe from a single UEM platform, particularly those managing mixed BYOD and corporate-owned fleets.

### Druva Endpoint Data Protection
**Type:** Data-centric remote wipe with offline capability  
Druva's remote wipe capability is specifically designed to protect data on lost or stolen devices, with a key differentiator: **data can be wiped even without an internet connection** on the stolen device [citation:1]. It also supports wiping data when a new SIM card is inserted [citation:10]. Druva offers **device remote-wipe with backup**, ensuring critical data is preserved before the device is wiped, and DLP policies can automate remote deletion of shared data as well [citation:1][citation:10].  
**Best for:** Organizations prioritizing data protection over device management, particularly for remote workers with sensitive data on endpoints that may be lost or stolen.

---

## 🟩 Open-Source Software

Self-hosted, community-driven Remote Device Management and Remote Wipe tools. The open-source ecosystem is **limited and fragmented**, with most projects focused on specific platforms (Linux, Android) or in early-stage development.

### OpenUEM
**Stack:** Go / NATS / Web UI | **License:** Apache-2.0  
OpenUEM is an open-source, self-hosted **Unified Endpoint Manager** recognized with a SourceForge Rising Star award in January 2026 [citation:9]. It uses agents for Windows, Linux, and macOS, with a clean and concise web interface for managing IT assets. The project is structured as multiple repositories (agent, console, worker, NATS service, cert-manager) under Apache-2.0 [citation:9]. **Important limitation:** Search results do not explicitly confirm remote wipe capabilities—the project emphasizes asset management and endpoint visibility, and wipe functionality should be verified against current documentation before relying on it for lost-device scenarios.  
**Best for:** IT teams wanting a self-hosted, actively maintained open-source UEM foundation for cross-platform endpoint management, with the caveat that remote wipe capabilities need validation.

### linDM
**Stack:** Linux (native tools) / Self-hosted server | **License:** Open Source  
linDM is a self-hosted MDM for **Linux fleets**, supporting enrollment, profiles, disk encryption/TPM, **lock and wipe**, OS updates, and remote assistance using each distribution's native tools [citation:15]. Wipe erases encryption keys rather than physical drives, requiring an encrypted installation to function [citation:15]. Destructive actions like lock and wipe require independent approval, and the platform supports a broad range of distributions including Ubuntu, Debian, Fedora, RHEL, Rocky, AlmaLinux, Arch, and openSUSE [citation:15].  
**Best for:** Linux-centric organizations needing native remote wipe (via encryption key erasure) across diverse distributions, accepting that wipe requires encrypted installs.

### SVT MDM
**Stack:** Android (Kotlin) / MQTT / HTTPS | **License:** Open Source  
SVT MDM is an open-source Android MDM agent with a tiered capability model: **plain** (location/inventory), **device_admin** (adds force-lock and **factory wipe**), and **device_owner** (adds silent grants, Phase 3) [citation:7]. Instant commands arrive over MQTT with HTTPS polling fallback, and commands are de-duplicated by ID with a 10-minute expiration for destructive actions like wipe and lock [citation:7]. The device token is stored in EncryptedSharedPreferences, and the agent only connects to TLS brokers [citation:7].  
**Best for:** Android-only fleets needing an open-source MDM with factory wipe via Device Admin, accepting that Device Owner features are still in development.

### Headwind MDM
**Stack:** Java / Android | **License:** Apache-2.0 (open core)  
Headwind MDM is an open-source Android MDM platform where the community edition allows implementing custom functionality under Apache-2.0 [citation:8]. **Important licensing caveat:** The **factory reset (including REST API) is available only in Headwind MDM Premium**, not the open-source edition [citation:8]. While the open-source version can be modified to add features, remote factory wipe is not included out of the box.  
**Best for:** Android fleets where the organization is prepared to implement custom wipe logic or purchase the Premium edition, accepting that remote factory reset is not an open-source feature.

### Sextant (DAWO)
**Stack:** NixOS / Rust / Go | **License:** Open Source (proof of concept)  
Sextant is a declarative fleet control-plane for NixOS with remote lock and wipe modeled as **config-as-data** rather than live RCE, with every action an audited commit . **Critical limitation:** The destructive wipe execution is **deliberately not implemented** in the current build—the agent spools and acknowledges a wipe intent but performs no destruction, and the project explicitly states it is "not MDM: declarative pull, no remote wipe" . It is a proof of concept seeking contributors.  
**Best for:** NixOS fleet operators interested in contributing to a declarative management model, not for production remote wipe needs.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Remote Wipe | Wipe Granularity | Offline Wipe | Self-Hosted | Best For |
|----------|---------|-------|-------------|------------------|--------------|-------------|----------|
| **Microsoft Intune** | Proprietary | Cloud | ✅ Factory reset | Full + eSIM preserve | ❌ | ❌ | Microsoft 365 organizations |
| **Workspace ONE** | Proprietary | Cloud | ✅ Enterprise + Device | Corporate vs full | ❌ | ❌ | Enterprise BYOD separation |
| **Jamf Pro** | Proprietary | Cloud | ✅ Erase | Full (Mac/iOS) | ❌ | ❌ | Apple-centric fleets |
| **Endpoint Central** | Proprietary | Cloud/On-prem | ✅ Complete + Corporate | Full vs corporate | ❌ | (On-prem) | Mixed BYOD/corporate fleets |
| **Druva** | Proprietary | Cloud | ✅ Data wipe | Data (with backup) | ✅ | ❌ | Data protection focus |
| **OpenUEM** | Apache-2.0 | Go/NATS | (Unverified) | (Check docs) | ❌ | ✅ | Cross-platform UEM foundation |
| **linDM** | Open Source | Linux | ✅ Key erasure | Encryption keys | ❌ | ✅ | Linux fleets (encrypted) |
| **SVT MDM** | Open Source | Android | ✅ Device Admin | Full factory | ❌ | ✅ | Android-only fleets |
| **Headwind MDM** | Apache-2.0 | Android | ❌ (Premium only) | N/A in OSS | ❌ | ✅ | Android (custom or Premium) |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Remote Device Management or Remote Wipe tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the open-source ecosystem for **remote wipe specifically** is **extremely limited and fragmented**. **linDM** is the only project with confirmed, production-oriented remote wipe for Linux (via encryption key erasure) [citation:15], while **SVT MDM** offers Android factory wipe through Device Admin [citation:7]. **OpenUEM** is a promising cross-platform UEM but its wipe capabilities are unverified [citation:9]. **Headwind MDM** gates remote factory reset behind its Premium tier [citation:8], and **Sextant** explicitly does not implement wipe execution . Commercial platforms like Intune, Workspace ONE, and Jamf Pro remain the only practical options for production-grade, cross-platform remote wipe.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
