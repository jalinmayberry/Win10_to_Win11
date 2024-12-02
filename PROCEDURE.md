### Preparing for Windows 11: A Step-by-Step Guide

Transitioning from Legacy BIOS to UEFI is a crucial step in modernizing your system and unlocking features such as Secure Boot, TPM 2.0, and compatibility with Windows 11. If you find yourself lagging behind on this migration, as I did, this blog outlines how I successfully converted my disk from MBR (Master Boot Record) to GPT (GUID Partition Table) and enabled UEFI without losing any data. I have documented the entire process, including the tools I used, the commands I executed, and the valuable lessons I learned along the way.

### Initial Problem

I wanted to prepare my system for Windows 11, which requires the following:

- UEFI Boot Mode
- Secure Boot
- TPM 2.0

My system was initially set up in Legacy BIOS mode with an MBR disk. Since UEFI requires a GPT partition table, I needed to:

1. Convert the disk to GPT.
2. Switch the firmware to UEFI mode.
3. Enable Secure Boot and TPM 2.0.

### Tools and Prerequisites

**Windows Built-in Tools:**

- Disk Management
- DiskPart
- MBR2GPT.exe

**Recovery Options:**

- A Windows Recovery USB (optional for troubleshooting).

**Backup:**
Although MBR2GPT is non-destructive, I would create a full backup of my important files before proceeding.

**NOTE**: BIOS configurations will be configured on an MSI Z370 Series Board so utilize equivalent references as you must.

### Step 1: Verify Disk Layout

**Using DiskPart to Identify Disk and Partition Information**
Before conversion, I confirmed my disk layout to ensure it met the requirements for MBR2GPT:

1. Open Command Prompt as an administrator.
2. Run the following commands to check the disk layout.

I confirmed the following:

- The target disk had no more than three primary partitions.
- There was enough space for the EFI System Partition (at least 100 MB).

### Step 2: Run MBR2GPT Without Data Loss

**Validating the Disk**
To ensure compatibility for conversion, I ran the following command:

```
mbr2gpt /validate /disk:[disk number]

```

(Note: Replace [disk number] with the number of the disk identified in DiskPart, e.g., 0.)

The tool checked the disk layout and confirmed it was ready for conversion.

**Converting the Disk**
Once validation was successful, I converted the disk to GPT:

```
mbr2gpt /convert /disk:[disk number]

```

The output confirmed success, indicating that:

- The EFI System Partition was created.
- The new boot files were installed.
- The existing boot entries were migrated.

### Result

After running MBR2GPT, my disk was converted to GPT successfully, without any data loss. The tool also created a new EFI System Partition, which is necessary for UEFI boot..

### Step 3: Configure UEFI in BIOS

After the disk conversion, I restarted my system and accessed the BIOS settings on my MSI Gaming M5 motherboard:

1. Press **Delete** during startup to enter the BIOS.
2. Navigate to **Advanced > Settings > Boot**.
3. Change the boot mode to **UEFI Only** (previously set to Legacy+UEFI).
    
    ![image](https://github.com/user-attachments/assets/e8d583a9-fc0b-41b3-9b99-f91f8b729b12)

    [Reference Image Source](https://appuals.com/msi-z370-gaming-pro-carbon-motherboard-review/) 
    
4. Ensure UEFI: Hard Disk is set as the first boot option.
5. Save the changes and reboot.

### Step 4: Enable Secure Boot and TPM 2.0

Once UEFI was active, I enabled Secure Boot and TPM 2.0 for Windows 11 compatibility:

1. In the BIOS Settings, navigate to **Advanced > Settings > Security > Trusted Computing**.
2. Select **Security Device Support** and set it to **Enable**.
    
    ![image 1](https://github.com/user-attachments/assets/2aab996c-3ef2-4b04-bcee-c9bf4f637ee6)

    [Reference Image Source](https://www.msi.com/blog/How-to-Enable-TPM-on-MSI-Motherboards-Featuring-TPM-2-0)
    
3. Set **TPM Device Selection** to **PTT (Platform Trust Technology)** for Intel-based systems 
4. Set **TPM Device Selection** to **AMD CPU fTPM** for AMD-based systems
5. Return to **Advanced > Settings > Boot** and enable **Secure Boot**, then save the changes.

**In Windows:**

1. Verify both features using System Information (msinfo32):
    - BIOS Mode: UEFI
    - Secure Boot State: On
2. Check TPM status using `tpm.msc` to confirm TPM 2.0 is enabled and ready.

### Step 5: Verify Recovery Environment (WinRE)

During the conversion, I encountered an issue where the Windows Recovery Environment (WinRE) appeared to need repairs. However, after restarting the system following the MBR2GPT conversion, WinRE began working correctly without additional steps. If issues persist, you can try the following commands to repair it manually:

1. Disable WinRE:
    
    ```
    reagentc /disable
    
    ```
    
2. Reassign the Recovery Environment path:
    
    ```
    reagentc /setreimage /path R:\\Windows
    
    ```
    
    (Replace R: with the drive letter of the Recovery Partition.)
    
3. Re-enable WinRE:
    
    ```
    reagentc /enable
    
    ```
    
4. Verify WinRE status:
    
    ```
    reagentc /info
    
    ```
    

### Final Results

After completing these steps, I verified the following:

- My PC was booting in UEFI mode.
- The disk was using the GUID Partition Table (GPT).
- Secure Boot and TPM 2.0 were enabled.

I also confirmed that my system passed the Windows 11 compatibility check.

![image 2](https://github.com/user-attachments/assets/65cd53d8-9d32-4313-bbce-7cfeace91e7a)

### Step 6: Install Windows 11 Using the Installation Assistant

To upgrade to Windows 11, I used the official Windows 11 Installation Assistant:

1. **Download the Installation Assistant** from the Windows 11 Download page.
2. **Run the Installation Assistant:** Launched the tool and followed the on-screen instructions to initiate the upgrade.

![image 3](https://github.com/user-attachments/assets/53ec63e3-c5ee-41bd-8e66-16803302c594)

**Installation Process:**
The tool downloaded the necessary files and upgraded my system while preserving my files and applications. It should take 30-60 minutes to complete.

**Post-Installation Verification:**
Check the Windows version by navigating to **Settings > System > About,** and ensure that Windows 11 has been installed successfully and is operating smoothly.

![image 4](https://github.com/user-attachments/assets/813cbd35-fe96-49cf-9819-859dc2af1895)

### Lessons Learned

- **MBR2GPT is a powerful tool:** It's easy to use and can convert disks without data loss if the requirements are met.
- **Backup is essential:** Even with non-destructive tools, it’s better to be safe than sorry.
- **WinRE issues can occur:** Repairing the Recovery Environment may be necessary after conversion.
- **UEFI setup requires BIOS adjustments:** Switching to UEFI mode and enabling Secure Boot are critical steps.
