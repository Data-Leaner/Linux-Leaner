> 快速创建一个文件系统
```bash
dd if=/dev/zero of=/dev/sdc bs=8k count=10000
mkfs -t ext3 /dev/sdc
```
