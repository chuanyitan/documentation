# Linux System Study Note
* include linux system and kernel Study

# Linux System

## ELF file
* elf的开头一般是有一个magic number(0x 7F 45 4C 46 02 01  01 00)
* study link1 [知乎](https://zhuanlan.zhihu.com/p/544198038) 
* study link2 [博客](https://www.cnblogs.com/QiQi-Robotics/p/15573352.html)

```bash
# Elf Header

#define EI_NIDENT 16

typedef struct {
        unsigned char   e_ident[EI_NIDENT];
        Elf32_Half      e_type;
        Elf32_Half      e_machine;
        Elf32_Word      e_version;
        Elf32_Addr      e_entry;
        Elf32_Off       e_phoff;
        Elf32_Off       e_shoff;
        Elf32_Word      e_flags;
        Elf32_Half      e_ehsize;
        Elf32_Half      e_phentsize;
        Elf32_Half      e_phnum;
        Elf32_Half      e_shentsize;
        Elf32_Half      e_shnum;
        Elf32_Half      e_shstrndx;
} Elf32_Ehdr;

typedef struct {
        unsigned char   e_ident[EI_NIDENT];
        Elf64_Half      e_type;
        Elf64_Half      e_machine;
        Elf64_Word      e_version;
        Elf64_Addr      e_entry;
        Elf64_Off       e_phoff;
        Elf64_Off       e_shoff;     // 重要: 我们下一步的解析目标就从这里开始,这个段表的文件内偏移
        Elf64_Word      e_flags;
        Elf64_Half      e_ehsize;    // 当前elf header的size大小
        Elf64_Half      e_phentsize; // Size of program headers
        Elf64_Half      e_phnum;     // program header table 的数量
        Elf64_Half      e_shentsize; // Size of section headers， 一个段描述符的大小
        Elf64_Half      e_shnum;     // 重要: 这里给出了段表中的表项的个数，有多少个段描述符,同时也说明了一个ELF文件中有多少个段.
        Elf64_Half      e_shstrndx;  // 重要: 这里给出了段表中引用的用于表示段名的字符串表的段表项索引
} Elf64_Ehdr;

======================================

MAGIC NUM: 0x7F, 0x45, 0x4C, 0x46   (4)
  platform: 64位对象                 (1)
    endian: 小端                     (1)
  main ver: 1                        (1)
  EI_OSABI: 0                        (1)
EI_ABIVERSION: 0                     (1)   (7 padding to 8 align)       16
    e_type: 1  - ETYPE_REL           (2)
 e_machine: 62 - EM_X86_64           (2)
 e_version: 1                        (4)
   e_entry: 0  - 备注: 当没有入口,或者是目标文件时,为0值.即无效 (8)         16
   e_phoff: 0                        (8)
   e_shoff: 0x0000000000000190 , 段表偏移 (8)                            16
   e_flags: 0                        (4)
   e_ehsize: 64(0x40)                (2)
   e_phentsize: 0                    (2)
   e_phnum: 0                        (2)
   e_shentsize: 64                   (2)
   e_shnum: 13 , 段表数量             (2)
e_shstrndx: 10 , 段表引用注常量表编号  (2)             12+4=16      -->64 B
======================================
```

比如，ELF文件中会有很多的段，如 .text，.data以及.bss段等



 ```bash
 # Section Header Table
typedef struct {
	Elf32_Word	sh_name;
	Elf32_Word	sh_type;
	Elf32_Word	sh_flags;
	Elf32_Addr	sh_addr;
	Elf32_Off	sh_offset;
	Elf32_Word	sh_size;
	Elf32_Word	sh_link;
	Elf32_Word	sh_info;
	Elf32_Word	sh_addralign;
	Elf32_Word	sh_entsize;
} Elf32_Shdr;

typedef struct {
	Elf64_Word	sh_name; // 一个段的名称
	Elf64_Word	sh_type;
	Elf64_Xword	sh_flags;
	Elf64_Addr	sh_addr; // 加载到内存后的虚拟地址
	Elf64_Off	sh_offset; // 相对于文件的偏移
	Elf64_Xword	sh_size; // 段内容的大小
	Elf64_Word	sh_link;
	Elf64_Word	sh_info;
	Elf64_Xword	sh_addralign;
	Elf64_Xword	sh_entsize;
} Elf64_Shdr;
 ```
 