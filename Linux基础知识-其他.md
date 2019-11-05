> 快速创建一个文件系统
```bash
# 创建一个文件
# if: input file 输入文件 
# of: output file 输出文件
# bs: block size 块大小
# count: 块个数
dd if=/dev/zero of=/dev/sdc bs=8k count=10000

# 格式化为一个文件系统
# -t：type 文件系统的类型
mkfs -t ext3 /dev/sdc

# 创建挂载的文件目录
mkdir -p /app/log

# 进行挂载
mount -o loop /dev/sdc /app/log

# 查看文件大小
df -h
```
