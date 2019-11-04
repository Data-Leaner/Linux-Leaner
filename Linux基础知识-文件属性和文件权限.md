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
    > `fsck`检查的时候，空间才会被释放, **如有有正在读取的程序正在读取该文件，那么该文件也不会被删除**
    
    > 两个文件的inode信息是一致的
    ```bash
    # 创建文件
    touch ln_test_file
    
    # 查看链接数
    ls -li ln_test_file 
    
    # 为文件创建硬链接
    ln ln_test_file ln_hard_link_file
    
    # 查看链接数
    ls -li ln_test_file
    
    # 查看硬链接文件
    ls -li ln_hard_link_file
    
    # 删除源文件
    rm -f ln_test_file
    
    # 查看硬链接文件
    ls -li ln_hard_link_file
    
    # 查看硬链接文件内容
    cat ln_hard_link_file
    ```
    
    > 目录不允许创建硬链接
    ```bash
    # 创建目录
    mkdir ln_test_dir
    
    # 创建硬链接
    ln ln_test_dir ln_hard_link_dir
    ```
    
    > 但是目录一旦创建出来，硬链接数为2，是因为目录中的`.`为目录的硬链接
    ```bash
    # 查看创建目录的inode
    ls -ldi ln_test_dir
    
    # 进入目录
    cd ln_test_dir
    
    # 查看.目录的inode
    ls -ldi .
   
    # 查看 .. 目录的 inode
    ls -ldi ..
    ```
  
  - 软链接(symbolic link)，也叫符号链接
    > `ln -s`创建出来的叫软链接.软链接实际上就是一个文本文件，该文本中包含软链接指向另一文件位置信息内容。一般通过软链接访问到文件名，然后通过文件
    > 名访问inode，再进行数据访问，这样如果源文件被删除之后，那么软链接也将失效。
    
    > 两个文件的inode信息是不一致的
  
    ```bash
     # 创建文件
    touch ln_test_file
    
    # 查看链接数
    ls -li ln_test_file 
    
    # 为文件创建软链接
    ln -s ln_test_file ln_symbol_link_file
    
    # 查看链接数
    ls -li ln_test_file
    
    # 查看软链接文件
    ls -li ln_symbol_link_file
    
    # 删除源文件
    rm -f ln_test_file
    
    # 查看软链接文件
    ls -li ln_symbol_link_file
    
    # 查看软链接文件内容
    cat ln_symbol_link_file
    ```
  
    > 创建目录软链接
    ```bash
    # 创建目录
    mkdir ln_test_dir
    
    # 创建软链接
    ln ln_test_dir ln_symbolic_link_dir
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
