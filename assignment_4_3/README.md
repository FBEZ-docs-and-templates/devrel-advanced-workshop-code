# Assignment 4.3 -- Solution

This folder contains the solution for the assignment 4.3 of the ESP-IDF: Advanced workshop. 

## 1. Objective

This project demonstrates how to enable **flash encryption** on an ESP32 device using ESP-IDF. It walks through checking the encryption status, enabling development-mode encryption, updating the partition table, and verifying the resulting eFuse changes.

---

## 2. Key Concepts

* **eFuses**: One-time programmable hardware configuration bits used to manage device security features.
* **Flash Encryption**: Automatically encrypts and decrypts data stored in flash memory during reads/writes.
* **Development Mode Encryption**: A non-production-safe mode allowing re-flashing with plaintext.
* **Partition Table Offsets**: Adjustments needed to accommodate a larger bootloader after encryption is enabled.

---

## 3. Features

* Query and inspect device eFuses to determine current encryption status.
* Enable flash encryption in development mode via `menuconfig`.
* Configure a compatible partition table to support encrypted bootloader.
* Reset, flash, and verify the updated encryption status post-deployment.
* Verbose bootloader logging for clearer understanding of encryption flow.

---

## 4. Implementation Overview

The project contains ESP-IDF-compatible source files and configuration settings aligned with the following steps:

1. **Check Initial eFuse Status**
   Use `idf.py efuse-summary` to confirm default zero values in key security-related eFuses.

2. **Enable Flash Encryption**

   * Navigate to: `Security Features` → `Enable flash encryption on boot`
   * Set `Usage Mode` to `Development (Not secure)`

3. **Adjust Bootloader Verbosity**

   * Set `Bootloader log verbosity` to `Verbose` in `menuconfig` to aid debugging.

4. **Configure Partition Table**
   The bootloader’s encrypted version requires more space. Update the partition table offset to ensure compatibility.

5. **Flash and Reset**

   * Flash the project once to burn required eFuses (`BURN DEV` prompt).
   * Reset the device, then flash again to apply encryption.

6. **Re-check Encryption Status**
   Run `idf.py efuse-summary` to verify changes:

   * `SPI_BOOT_CRYPT_CNT` should be enabled.
   * One `BLOCK_KEY` should store the encryption key with appropriate `KEY_PURPOSE`.

---

## 5. Notes

* **Warning**: Flash encryption is an irreversible operation. Even in development mode, certain eFuses will be permanently burned.
* **Development Mode Limitation**: This mode is not secure for production. It allows unencrypted reflashing and disables certain protections.
* **Reset Required**: After the initial burn, a manual reset is required before re-flashing to finalize encryption setup.
* **Bootloader Growth**: The encryption mechanism increases bootloader size. Ensure the partition table offset accommodates this change.
* **Security Awareness**: Even in a lab setting, it's good practice to review what each eFuse controls before proceeding.

