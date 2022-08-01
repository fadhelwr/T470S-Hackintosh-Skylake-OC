# ThinkPad T470s running macOS (OpenCore bootloader)

<img align="right" src="/Images/t460s-monterey.png" alt="Lenovo Thinkpad T460s macOS Hackintosh OpenCore" width="300">

[![macOS](https://img.shields.io/badge/macOS-11.6.7-blue)](https://developer.apple.com/documentation/macos-release-notes)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.8.1-green)](https://github.com/acidanthera/OpenCorePkg)
[![Model](https://img.shields.io/badge/Model-20F9*-lightgrey)](https://psref.lenovo.com/Product/ThinkPad_T460s)
[![BIOS](https://img.shields.io/badge/BIOS-1.53-yellow)](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t460s/downloads/driver-list/component?name=BIOS%2FUEFI)
[![License](https://img.shields.io/badge/license-MIT-purple)](/LICENSE)

**DISCLAIMER:**  
- Baca seluruh README sebelum Anda mulai
- Pengembang tidak bertanggung jawab atas segala kerusakan yang mungkin Anda sebabkan.
- Jika Anda menemukan kesalahan atau memperbaiki apa pun — baik dalam konfigurasi atau dalam dokumentasi — harap pertimbangkan untuk membuka masalah atau pull request.

## Pengenalan

<details>  
<summary><strong>Spesifikasi 💻</strong></summary>
</br>

| Model              | Thinkpad T470s\*\*                                                                               |
| :----------------- | :-------------------------------------------------------------------------------------------------------- |
| Processor          | Core i5-6300U (2C, 2.4 / 3.0GHz, 3MB)                                                                     |
| Graphics           | Integrated Intel HD Graphics 520                                                                          |
| Memory             | 4GB Soldered + 4GB DIMM 2133MHz DDR4, dual-channel                                                        |
| Display            | 14" Full HD (1920x1080) IPS, Touch (read [Post-install>Enable Touchscreen](https://github.com/simprecicchiani/ThinkPad-T460s-macOS-OpenCore#post-install-optional))    |
| Storage            | Samsung 256GB NVMe SSD                                                                |
| Ethernet           | Intel Ethernet Connection I219-LM (Jacksonville)                                                          |
| WLAN + Bluetooth   | Broadcom Apple Wifi Device BCM43XX                                                   |
| Camera             | HD720p resolution, low light sensitive, fixed focus                                                       |
| Audio support      | HD Audio, Realtek ALC3245 codec, stereo speakers 1Wx2, dual array microphone, combo audio/microphone jack |
| Keyboard           | 6-row, spill-resistant, multimedia Fn keys, LED backlight                                                 |
| Battery            | Front Li-Polymer 3-cell (23Wh) and rear Li-Ion 3-cell (26Wh), both Integrated                             |

</details>

## Installation

<details>  
<summary><strong>Cara install macOS (Hackintosh)</strong></summary>
</br>

1. [Buat bootable](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)
1. Unduh [EFI release terakhir](https://github.com/fadhelwr/T470S-Hackintosh-Skylake-OC/releases/) dan salin ke partisi ESP
1. Ubah setelan BIOS seperti tabel di bawah.
1. Boot melalui installer USB (tekan F12 untuk memilih perangkat boot) and [start the installation process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html#booting-the-opencore-usb)

| Menu     |                   |                                 | Setting     |
| -------- | ----------------- | ------------------------------- | ----------- |
| Config   | USB               | UEFI BIOS Support               | `Enable`    |
|          | Power             | Intel SpeedStep Technology      | `Enable`    |
|          |                   | CPU Power Management            | `Enable`    |
|          | CPU               | Hyper-Threading Technology      | `Enable`    |
| Security | Security Chip     |                                 | `Disable`   |
|          | Memory Protection | Execution Prevention            | `Enable`    |
|          | Virtualization    | Intel Virtualization Technology | `Enable`    |
|          |                   | Intel VT-d Feature              | `Enable`    |
|          | Anti-Theft        | Computrace                      | `Disable`   |
|          | Secure Boot       |                                 | `Disable`   |
|          | Intel SGX         |                                 | `Disable`   |
|          | Device Guard      |                                 | `Disable`   |
| Startup  | UEFI/Legacy Boot  |                                 | `UEFI Only` |
|          | CSM Support       |                                 | `No`        |
|          | Boot Mode         |                                 | `Quick`     |

</details>

<details>  
<summary><strong>Mengaktifkan Layanan Apple</strong></summary>
</br>

1. Jalankan script berikut pada terminal

```bash
git clone https://github.com/corpnewt/GenSMBIOS && cd GenSMBIOS && chmod +x GenSMBIOS.command && ./GenSMBIOS.command
```

2. Ketik `3` untuk generate SMBIOS, kemudian ENTER
3. Ketik `MacbookPro13,3 5`, kemudian ENTER. Biarkan jendela Terminal terbuka.
4. Buka `/EFI/OC/Config.plist` dengan editor apapun dan arahkan ke `PlatformInfo -> Generic`
5. Tambahkan/Edit data pada `MLB, SystemSerialNumber dan SystemUUID`

```diff
<key>PlatformInfo</key>
<dict>
   <key>Generic</key>
   <array>
      </dict>
         <key>AdviseWindows</key>
         <false/>
         <key>SystemMemoryStatus</key>
         <string>Auto</string>
         <key>MLB</key>
+        <string>M0000000000000001</string>
         <key>ProcessorType</key>
         <integer>0</integer>
         <key>ROM</key>
         <data>ESIzRFVm</data>
         <key>SpoofVendor</key>
         <true/>
         <key>SystemProductName</key>
         <string>MacBookPro16,3</string>
         <key>SystemSerialNumber</key>
+        <string>W00000000001</string>
         <key>SystemUUID</key>
+        <string>00000000-0000-0000-0000-000000000000</string>
      </dict>
   </array>
</dict>
```

6. Simpan dan muat ulang perangkat

</details>

## Other tweaks

<details>  
<summary><strong>Setup Hibernatemode & Sleep</strong></summary>
</br>
<a href="https://www.tonymacx86.com/threads/release-sleeponlowbattery-solb.264785">Script that performs auto sleep/hibernate at low battery</a>
<br><br>
1.Buka terminal
<br>
2.Masukkan command berikut satu persatu
<br>
Pengaturan untuk AC:

```
sudo pmset -c standby 1
sudo pmset -c hibernatemode 0
```

Pengaturan untuk Baterai:

```
sudo pmset -b standby 1
sudo pmset -b standbydelayhigh 900
sudo pmset -b standbydelaylow 60
sudo pmset -b hibernatemode 25
sudo pmset -b highstandbythreshold 70
```

Pengaturan untuk baterai dan AC:

```
sudo pmset -a acwake 0
sudo pmset -a lidwake 1
sudo pmset -a powernap 0
```

Untuk mengembalikkan pengaturan awal jalankan `pmset restoredefaults` pada terminal.

## Status

<details>  
<summary><strong>What's working ✅</strong></summary>
</br>
 
- [x] CPU Power Management `~1W on IDLE`
- [x] Intel HD 520 Graphics `incuding graphics acceleration`
- [x] USB ports
- [x] Internal camera `working fine on FaceTime, Skype, Zoom and others`
- [x] Sleep / Hibernatemode `25 or 3` / Wake / Shutdown / Reboot
- [x] Intel Gigabit Ethernet
- [x] Wifi, Bluetooth, Airdrop, Handoff, Continuity, Sidecar wireless `some functionalities may be buggy or broken on Intel WLAN cards`
- [x] iMessage, FaceTime, App Store, iTunes Store `Please generate your own SMBIOS`
- [x] Speakers and headphones combo jack 
- [x] Batteries
- [x] Keyboard map and hotkeys with [YogaSMC](https://github.com/zhen-zen/YogaSMC)
- [x] Touchscreen
- [x] [Trackpad, Trackpoint and physical buttons](/Images/VoodooRMI-T460s-trackpad-gestures.gif) `all macOS gestures working thanks to VoodooRMI`
- [x] SIP and FileVault 2 can be turned on
- [x] HDMI `with digital audio passthrough`
- [x] SD Card Reader `slow r/w speed but works`

</details>

<details>  
<summary><strong>What's not working ⚠️</strong></summary>
</br>

- [ ] Some users reported Mini DisplayPort is broken for them with latest updates, but it's working for me just fine
- [ ] Safari DRM `Use Chromium engine to watch Apple TV+, Amazon Prime Video, Netflix and others`
- [ ] WWAN (needs to be implemented)

</details>

## Thanks to
- [simprecicchiani](https://github.com/simprecicchiani)
- The hackintosh community on GitHub
- [InsanelyMac](https://www.insanelymac.com/forum/)
- [r/hackintosh](https://www.reddit.com/r/hackintosh/).
