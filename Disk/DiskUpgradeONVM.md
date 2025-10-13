# Ubuntu Disk Expansion Guide - LVM

## Overview
This guide documents the process of expanding the root disk on an Ubuntu server using **LVM (Logical Volume Manager)**.  
In this case, we expanded `/dev/sda3` from **13.2G → 23.2G**.

---

## 📋 Server Information

| Item | Value |
|------|--------|
| Operating System | Ubuntu Linux |
| Disk Device | `/dev/sda` |
| Partition | `/dev/sda3` (LVM) |
| Volume Group | `ubuntu-vg` |
| Logical Volume | `ubuntu-lv` |
| Mount Point | `/` |
| Initial Size | 13.2G |
| Final Size | 23.2G |

---

## ✅ Prerequisites
- Root or `sudo` access  
- Basic knowledge of Linux commands  
- **Backup** of important data (recommended)

---

## 🔧 Step-by-Step Instructions

### Step 1: Verify Current Disk Layout
```bash
lsblk
sudo fdisk -l /dev/sda
```
This shows your current **partition structure** and identifies available **unallocated space**.

---

### Step 2: Resize the Partition
Start the `parted` utility:
```bash
sudo parted /dev/sda
```

Inside the parted prompt, expand the partition to use all available space:
```
(parted) resizepart 3 100%
(parted) quit
```

---

### Step 3: Resize the Physical Volume
```bash
sudo pvresize /dev/sda3
```

**Expected output:**
```
Physical volume "/dev/sda3" changed
1 physical volume(s) resized or updated / 0 physical volume(s) not resized
```

---

### Step 4: Extend the Logical Volume
Extend the logical volume to use all free space:
```bash
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

**Alternative:** If you want to extend by a specific amount:
```bash
sudo lvextend -L +5G /dev/ubuntu-vg/ubuntu-lv
```

---

### Step 5: Resize the Filesystem
For **ext4** filesystem (online resizing while mounted):
```bash
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

For **XFS** filesystem:
```bash
sudo xfs_growfs /
```

---

### Step 6: Verify the Changes
Check the new disk size:
```bash
df -h
lsblk
```

---

## 📊 Results Comparison

### Before Expansion
| Component | Size |
|-----------|------|
| `sda3` partition | 13.2G |
| Logical volume | 13.25 GiB |
| Root filesystem (`/`) | ~13G |
| Available space | ~3-4G |

### After Expansion
| Component | Size |
|-----------|------|
| `sda3` partition | **23.2G** ✓ |
| Logical volume | **23.25 GiB** ✓ |
| Root filesystem (`/`) | **23G** ✓ |
| Available space | **~15G** ✓ |

---

## 📝 Sample Command Output

```bash
lff-study@worker:~$ df -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   23G  7.7G   15G  36% /
/dev/sda2                          1.7G  214M  1.4G  14% /boot

lff-study@worker:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0   25G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0  1.8G  0 part /boot
└─sda3                      8:3    0 23.2G  0 part 
  └─ubuntu--vg-ubuntu--lv 252:0    0 23.2G  0 lvm  /
```

---

## ⚠️ Important Notes

🔄 **Online Resizing:** The `resize2fs` command dapat expand ext4 filesystems while they are mounted, so **no reboot is required**.

💾 **Backup First:** Always **backup critical data** before performing disk operations.

🔍 **Check Filesystem Type:** Use `df -T` to verify if your filesystem is ext4, XFS, or other.

⚡ **LVM Advantage:** LVM makes disk management flexible; you can resize without rebooting.

📈 **Monitor Disk Space:** Regularly monitor available space with `df -h` to prevent the disk from becoming full.

---

## 🐛 Troubleshooting

### ❌ Error: "Device not found"
Ensure you're using the correct device name (e.g., `/dev/sda3` not `/dev/sdb3`)

### ❌ Error: "Filesystem is read-only"
Boot into recovery mode or from a live USB if the filesystem is corrupted.

### ❌ Changes not taking effect
Make sure you completed all steps in order:
1. `parted` resize
2. `pvresize`
3. `lvextend`
4. `resize2fs`

---

## 📚 References
- [Ubuntu LVM Guide](https://ubuntu.com/)
- [GNU Parted Manual](https://www.gnu.org/software/parted/)
- [LVM Documentation](https://tldp.org/HOWTO/LVM-HOWTO/)

---

## 🎯 Summary

✅ **Status:** Successfully Expanded  
📅 **Date Completed:** October 13, 2025  
👤 **Server:** lff-study@worker  
💾 **Disk Increase:** +10G (13.2G → 23.2G)