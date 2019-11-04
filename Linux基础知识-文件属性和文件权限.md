# Linux 目录
Linux中每一个文件都包含有访问全新的信息，这些信息控制了哪些组和用户可以访问该文件
## Linux文件
### 文件属性
文件或目录文件的属性包含：
- 索引节点(inode)
  > index node的简写，每个被格式化的存储设备包含两部分：inode和block。block用来存储实际的数据，inode包含的属性信息，主要是`ls -l`的结果，但是
  > 唯独不包含的是**文件名**，inode除了记录文件的属性外，还会为每个文件进行信息索引，所以inode就有了数值，可以通过inode的数值快速找到对应的文件。

  > centOS 5 inode大小为128字节，centOS 6 inode大小默认为256字节

  > 存储设备一旦被格式化，那么inode大小就确定了，无法再进行更改，但是在`格式化存储设备的时候可以指定inode的大小`, block的大小跟文件系统有关

  > 每个文件，哪怕是空文件都要占用**至少一个**`block`和一个`inode`

- 文件类型
  - **p:** 管道文件
  - **s:** socket文件
  - **\-:** 普通文件
  - **d:** 目录文件
  - **c:** 字符文件
  - **b:** 块文件
  - **l:** 链接文件
- 权限属性
  
  
- 链接数
  - 硬链接(hard link)
    > `ln` 直接创建出来的叫硬链接。通过索引节点进行链接，Linux允许多个文件名指向同一个inode节点，这样设计的目的是允许一个文件有多个路径，这样可以避免
    > 删除源数据。这样文件在被删除的时候，还是删除了文件名与inode节点的链接，并不删除inode和其他的链接。只有在所有的硬链接被删除之后，在新的数据存储或
    > `fsck`检查的时候，空间才会被释放
    
    ```bash
    # 创建文件
    touch ln_test_file
    
    # 查看链接数
    ls -li ln_test_file 
    
    # 为文件创建硬链接
    ln ln_test_file ln_hard_link_file
    
    # 查看链接数
    ls -li ln_test_file
    ```
  
  - 软链接(symbol link)，也叫符号链接
    > `ln -s`创建出来的叫软链接
  
    ```bash
     # 创建文件
    touch ln_test_file
    
    # 查看链接数
    ls -li ln_test_file 
    
    # 为文件创建软链接
    ln -s ln_test_file ln_symbol_link_file
    
    # 查看链接数
    ls -li ln_test_file
    ```
  
- 所属用户
- 所属组
- 最近修改时间

```bash
ls -lhi
# -i 显示inode的信息

# 查看inode个数
df -i

# 查看inode大小
dumpe2fs /dev/sda3 | grep -i "Inode size"

# 查看block大小
dumpe2fs /dev/sda3 | grep -i "block size"

```
