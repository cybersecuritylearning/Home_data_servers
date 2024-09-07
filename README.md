# Thi is for my dell poweredge r610 config


# Creating a Bootable VMware ESXi USB Drive (UEFI) on Ubuntu

This guide helps you create a bootable USB drive with VMware ESXi 6.7 on an Ubuntu system, including formatting as FAT32 and copying ISO contents.

---

## 1. Fix GParted FAT32 Support
If GParted shows an error about missing FAT32 support, install the required packages:

```bash
sudo apt update
sudo apt install dosfstools mtools
```

---

## 2. Format the USB as FAT32
- Open **GParted** and select your USB drive.
- Delete any existing partitions.
- Create a new **FAT32** partition and apply the changes.

---

## 3. Mount the ESXi ISO and USB
- Mount the ESXi ISO:

```bash
sudo mount -o loop /path/to/esxi.iso /mnt/esxi_iso
```

- Mount the USB drive:

```bash
sudo mkdir /mnt/usb
sudo mount /dev/sdX1 /mnt/usb
```
Replace `/dev/sdX1` with the correct partition.

---

## 4. Copy ISO Contents to USB
Use `rsync` to copy the ESXi ISO’s contents to the USB:

```bash
sudo rsync -avh --progress /mnt/esxi_iso/ /mnt/usb/
```

---

## 5. Sync and Unmount
Ensure all data is written to the USB:

```bash
sync
```

Unmount the ISO and USB:

```bash
sudo umount /mnt/esxi_iso
sudo umount /mnt/usb
```

---

## 6. Boot from USB
Insert the USB into your server and boot into the ESXi installer via UEFI.

---

### Why This Works
- **FAT32 partition**: UEFI systems require a FAT32 partition to boot.
- **Copying ISO contents**: Using `rsync` ensures the proper boot files are copied for UEFI compatibility.

This method ensures the USB drive is UEFI-compatible, avoiding issues with the `dd` method.