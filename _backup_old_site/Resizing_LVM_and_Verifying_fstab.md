
# Resizing an LVM Disk and Verifying `/etc/fstab`

## You're Absolutely Correct—Since This is an LVM Setup
Since this is an LVM setup, it should be possible to resize the logical volume and underlying physical volume without needing a live environment. My earlier suggestions involving `fdisk` were overly cautious for your situation. Let’s take a better, more LVM-centric approach that doesn’t require you to unmount the root filesystem unless absolutely necessary.

---

## What We Know:
1. **LVM is in use:** `/dev/sda3` is the physical volume, part of the volume group `ubuntu-vg`, and holds the logical volume `ubuntu--vg-ubuntu--lv` (mounted as `/`).
2. **Current Issue:** `/dev/sda3` does not span the full disk, and we need to expand it to use the additional space without data loss.

---

## Recommended Solution:
Since `/` is mounted and in use, unmounting it would require a live environment. However, LVM supports online resizing, so let’s use it properly.

---

### 1. Resize `/dev/sda3` to Fill the Disk
This step expands the partition itself to cover the new 250 GiB disk space. Even though it’s part of an LVM setup, the underlying partition must be resized.

1. Run `parted` for better handling of GPT disks:
   ```bash
   sudo parted /dev/sda
   ```
2. Within `parted`:
   - Select the disk:
     ```bash
     (parted) select /dev/sda
     ```
   - Print the current partition table:
     ```bash
     (parted) print
     ```
   - Resize the partition:
     ```bash
     (parted) resizepart 3 100%
     ```
   - Exit `parted`:
     ```bash
     (parted) quit
     ```

This adjusts `/dev/sda3` to use the remaining unallocated space.

---

### 2. Resize the Physical Volume
Inform LVM about the newly available space in the physical volume:
```bash
sudo pvresize /dev/sda3
```

This command updates LVM to recognize the extra space in `/dev/sda3`.

---

### 3. Extend the Logical Volume
Use the free space in the volume group (`ubuntu-vg`) to expand the logical volume (`ubuntu--vg-ubuntu--lv`):
```bash
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

This command allocates all available free space to the logical volume.

---

### 4. Resize the Filesystem
Finally, resize the filesystem to utilize the newly expanded logical volume:
- For ext4:
  ```bash
  sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
  ```
- For XFS:
  ```bash
  sudo xfs_growfs /
  ```

---

### 5. Verify the Expansion
Check the updated size of the root filesystem:
```bash
df -h /
```

---

## Why Unmounting is Not Needed
1. The LVM layer allows online resizing of the logical volume and the filesystem on top of it.
2. The only operation that involves the underlying partition (`/dev/sda3`) is handled by `parted`, which doesn’t require unmounting.

---

## `/etc/fstab` Summary

### 1. What `/etc/fstab` Does:
The `/etc/fstab` file contains information about disk partitions, their mount points, and options for mounting them. It ensures that partitions are automatically mounted at boot.

### 2. Verify `/etc/fstab` Configuration:
Run the following to inspect `/etc/fstab`:
```bash
cat /etc/fstab
```
Look for an entry corresponding to `/dev/sda3` or the LVM logical volume `/dev/ubuntu-vg/ubuntu-lv`.

### 3. Check the UUID:
Ensure the UUID listed in `/etc/fstab` matches the current UUID of `/dev/sda3`. To get the UUID, run:
```bash
blkid /dev/sda3
```

### 4. No Changes Needed:
If the `/etc/fstab` entries are correct (as they likely are for LVM), no updates are necessary. Confirm this by rebooting the system:
```bash
sudo reboot
```

### 5. Reboot Confirmation:
If the system boots successfully and the expanded root filesystem is accessible, your configuration is correct.

---

This concludes the full process for resizing the disk and verifying `/etc/fstab`.
