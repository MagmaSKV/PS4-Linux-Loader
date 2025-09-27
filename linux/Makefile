
all: payload.bin

clean:
	rm -f payload.elf payload.bin; 
	cd ps4-kexec; make clean
	cd ../lib/; make clean
    
../lib/lib.a:
	cd ../lib; make


ps4-kexec/kexec.bin:
	cd ps4-kexec; make

payload.elf: ../lib/lib.a main.c ps4-kexec/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__11_00__ -DPS4_11_00 main.c -Wl,-gc-sections -o payload-1100.elf -fPIE -ffreestanding

%.bin: %.elf
	objcopy $< --only-section .text --only-section .data --only-section .bss --only-section .rodata -O binary $@
	file $@ | fgrep -q '$@: DOS executable (COM)'

ps4-kexec-%/kexec.bin:
	cd ps4-kexec-$*; make
