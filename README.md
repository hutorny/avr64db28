# Building an avr64db28 project with avr-gcc

1. Download and install latest `avr-gcc` https://github.com/ZakKemble/avr-gcc-build/releases
2. Download and unzip latest Atmel support package from http://packs.download.atmel.com/<br>
   e.g. http://packs.download.atmel.com/Atmel.AVR-Dx_DFP.2.2.253.atpack
3. Build c/c++ files with the following options:
   - C++:<br>`avr-g++ -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - C:<br>`avr-gcc -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - ASM:<br>`avr-g++ -c -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - Linker:<br>`avr-g++ -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28 -Wl,--start-group -Wl,-lm  -Wl,--end-group -Wl,--gc-sections -Wl,--defsym,__DATA_REGION_LENGTH__=0x6000`
  
**Note**<br>
`__DATA_REGION_LENGTH__=0x6000` is needed to workaround _address 0x806001 of avr64db section .bss is not within region data_ caused by mismatching avr-gcc linker script and the chip definition from the support package.

**Details**<br>
Address 0x806000 is set by `spec-avr64db28`
```
*link_data_start:
	%{!Tdata:-Tdata 0x806000}
```
While the linker script `avrxmega2.x` defines
```
data   (rw!x) : ORIGIN = 0x802000, LENGTH = __DATA_REGION_LENGTH__
```
Datasheet defines SRAM memory map

| Data space | Range |
|-----------|--------------|
|(Reserved) | 0x1600-0x3FFF |
| SRAM 16 KB |  0x4000-0x7FFF |

Perhaps `0x802000` is a base address for the entire xmega2 family, while `0x806000` could be a valid start for the data section in `avr64db28`. However, the memory range communicated to the linker:
```
data             0x0000000000802000 0x0000000000002000 rw !x
```
is not valid for `avr64db28`. The `__DATA_REGION_LENGTH__=0x6000` parameter makes it
```
data             0x0000000000802000 0x0000000000006000 rw !x
```
Now `data` includes `0x806000` and the linker succeeds. Although it is not clear what is the use of `0x4000-0x5FFF` memory range
