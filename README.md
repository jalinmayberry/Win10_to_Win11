# Preparing for Windows 11: Legacy BIOS to UEFI Migration

This guide summarizes the steps taken to transition from **Legacy BIOS** to **UEFI** and enable features like **Secure Boot**, **TPM 2.0**, and **Windows 11 compatibility**, all without data loss.

---

## Objectives
- Convert disk from **MBR** to **GPT**.
- Switch from Legacy BIOS to **UEFI Firmware**.
- Enable Secure Boot and TPM 2.0 for Windows 11.

---

## Tools Used
- Windows Built-in Tools: `DiskPart`, `MBR2GPT.exe`
- BIOS Configuration: Adjustments for **MSI Z370 Gaming M5 motherboard**.
- Windows 11 Installation Assistant.

---

## Process Summary
1. **Disk Conversion**: Used `MBR2GPT` to convert the disk to GPT.
2. **UEFI Configuration**: Updated BIOS to enable **UEFI Boot Mode** and set **Windows Boot Manager** as the priority.
3. **Secure Boot & TPM**: Enabled Secure Boot and Intel PTT for TPM 2.0 in BIOS.
4. **Windows 11 Installation**: Upgraded via the Windows 11 Installation Assistant.

---

For detailed instructions, check the full [guide](https://github.com/jalinmayberry/Win10_to_Win11/blob/main/PROCEDURE.md) in this repository.
