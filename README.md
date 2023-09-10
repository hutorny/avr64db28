# Building an avr64db28 project with avr-gcc

1. Download and install latest `avr-gcc` https://github.com/ZakKemble/avr-gcc-build/releases
2. Download and unzip latest Atmel support package from http://packs.download.atmel.com/<br>
   e.g. http://packs.download.atmel.com/Atmel.AVR-Dx_DFP.2.2.253.atpack
3. Build c/c++ files with the following options:
   - C++:<br>`avr-g++ -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - C:<br>`avr-gcc -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - ASM:<br>`avr-g++ -c -D__AVR_DEV_LIB_NAME__=avr64db28 -I<Atmel.AVR-Dx_DFP>/include -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28`
   - Linker:<br>`avr-g++ -mmcu=avr64db28 -B <Atmel.AVR-Dx_DFP>/gcc/dev/avr64db28 -Wl,--start-group -Wl,-lm  -Wl,--end-group -Wl,--gc-sections`
