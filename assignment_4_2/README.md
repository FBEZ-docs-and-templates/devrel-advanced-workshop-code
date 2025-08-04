# Assignment 4.1 -- Solution

This folder contains the solution of the ESP-IDF advanced workshop. 

## 1. Objective

The objective of this assignment is to modify the partition table to enable Over-the-Air (OTA) firmware updates. This involves replacing the default single-app layout with one that supports a factory app and two OTA partitions.

## 2. Key Concepts

* **Partition Table**: Defines how flash memory is allocated in ESP32 devices.
* **OTA (Over-the-Air) Updates**: Mechanism to update firmware remotely without physical access to the device.
* **`esptool.py`**: Utility to read and write ESP32 flash memory.
* **`gen_esp32part.py`**: Script to convert binary partition tables to human-readable format.
* **ESP-IDF `menuconfig`**: Configuration interface to customize build options including partition layout and flash size.

## 3. Features

* Reads and decodes the existing partition table from flash memory.
* Switches to the built-in *Factory app, two OTA definitions* partition scheme.
* Expands flash size to accommodate multiple app partitions.
* Verifies changes by re-reading and decoding the updated partition table.

## 4. Implementation Overview

### Step 1: Check Current Partition Table

1. **Read flash memory:**

   ```bash
   esptool.py -p <YOUR-PORT> read_flash 0x8000 0x1000 partition_table.bin
   ```

2. **Convert to readable format:**

   ```bash
   python $IDF_PATH/components/partition_table/gen_esp32part.py partition_table.bin
   ```

Example output for default table:

```
# ESP-IDF Partition Table
# Name, Type, SubType, Offset, Size, Flags
nvs,data,nvs,0x9000,24K,
phy_init,data,phy,0xf000,4K,
factory,app,factory,0x10000,1M,
coredump,data,coredump,0x110000,64K,
```

### Step 2: Change Partition Scheme

* Open `menuconfig`:

  ```
  idf.py menuconfig
  ```

* Navigate to:

  * `Partition Table` → `Factory app, two OTA definitions`
  * `Serial Flasher Config` → `Flash Size` → `4MB` (required for OTA support)

### Step 3: Verify Updated Table

Repeat the same steps as in Step 1. You should now see:

```
# ESP-IDF Partition Table
# Name, Type, SubType, Offset, Size, Flags
nvs,data,nvs,0x9000,16K,
otadata,data,ota,0xd000,8K,
phy_init,data,phy,0xf000,4K,
factory,app,factory,0x10000,1M,
ota_0,app,ota_0,0x110000,1M,
ota_1,app,ota_1,0x210000,1M,
```

## 5. Notes

* This setup is essential for enabling OTA updates in future assignments.
* The partition layout used here is one of ESP-IDF’s built-in defaults—no custom CSV files are required at this stage.
* If your board has limited flash, ensure it supports at least 4MB before applying this configuration.
* The partition tool (`gen_esp32part.py`) is part of the ESP-IDF SDK, so make sure your environment variables are correctly set.
