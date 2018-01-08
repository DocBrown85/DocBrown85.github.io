---
layout: post
title: Mapping IO Registers With Union And Structs
---

Memory Mapped I/O is a technique that allows to use the same address space to address both memory and I/O devices.
The memory and registers of the I/O devices are mapped to address values, so when an address is accessed by the CPU,
it may refer to a portion of physical RAM, but it can also refer to memory of the I/O device. Thus, the CPU instructions used to access
the memory can also be used for accessing devices.

For example, suppose we have an I/O device which has only a 32-bit register storing an RGB value to use when lighting a LED. Such
register can be viewed as follows:

```
+---+---+---+---+
| 0 | R | G | B |
+---+---+---+---+

```

We can model the register using `uninion` and `struct` constructs in a way we can access both single fields and the whole RGB value
according to our need:

```
typedef union mmap_io_rgb_reg_u {
  
  volatile unsigned long REG;
  struct {
  
    volatile unsigned long :8;  // 8-bit unused space
    volatile unsigned long r:8;
    volatile unsigned long g:8
    volatile unsigned long b:8
    
  } FIELDS;
  
} RGB_REG;

```

The above code:

1. Uses a `union` to declare two different ways to view a memory location (the whole register and the single fields)
2. Uses a `struct` to define memory pieces that can be referenced individually, namely the parts of the whole register.
3. Uses the `:n` notation (which is used to declare the number of bits per data member in structures) to define the number
of bits assigned to each part of the register.

Supposing the start of the memory mapped IO devices whithin the address space is at address `0x40000000` and the `mmap_io_rgb_reg` 
starts at offset `0x40` as delcared in a proper header file:

```
// the starting address of memory mapped devices
#define MMAP_IO_BASE       0x40000000

// definition of the offset and address of the RGB register
#define RGB_IO_REG_OFFSET  0x40
#define RGB_IO_REG         (MMAP_IO_BASE + RGB_REG_OFFSET)

// other offsets and registers defined here...
```

the register can then be used as follows:

```
void main(void) {

  volatile RGB_REG* rgb_reg_ptr;
  
  rgb_reg_ptr = (RGB_REG*) RGB_IO_REG;
  
  // we can set single fields
  rgb_reg_ptr->FIELDS.r = 255;
  rgb_reg_ptr->FIELDS.g = 24;
  rgb_reg_ptr->FIELDS.b = 128;
  
  // or set the whole register
  rgb_reg_ptr->REG = 0;
  rgb_reg_ptr->REG = (255 << 16) | (24 << 8) | 128;
  
}
```
