all: entry64 cpu_features print nolib setup shutdown cpu_native\
	 alloc console serial_console vga_console link_script final_link

entry64:
	gcc -I./include -DCC_VERSION=7.5 -D__X86_64__ -m64 -U __linux__ -U __FreeBSD__ \
	-U __sun__ -D__ASSEMBLY__ -D __Unikraft__ -DUK_CODENAME="Rhea" -DUK_VERSION=0.4 \
	-DUK_FULLVERSION=0.4.0~00bbf2c -mtune=generic -no-pie -g3 -DKVMPLAT -D__LIBNAME__=libkvmplat \
	-D__BASENAME__=entry64.S -c entry64.S -o entry64.o -Wp,-MD,.entry64.o.d

cpu_features:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector -fno-omit-frame-pointer \
	-I./include -I./nolibc/include -I./plat/kvm/include \
	-fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" -DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~0453b2e \
	-O0 -fno-optimize-sibling-calls -fno-tree-vectorize -no-pie -D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks \
	-fno-asynchronous-unwind-tables  -mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf \
	-fno-builtin-sprintf -fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf -fno-builtin-vscanf \
	-fno-builtin-vfscanf -fno-builtin-vsscanf -DKVMPLAT       -g3 -D__LIBNAME__=libkvmplat -D__BASENAME__=cpu_features.c \
	-D__VARIANT__=common -c /home/daniel/Faculty/BachelorThesis/unikraft/plat/common/x86/cpu_features.c \
	-o cpu_features.common.o -Wp,-MD,.cpu_features.common.o.d

link_script:
	gcc  -E -P -x assembler-with-cpp  -I./include  -DCC_VERSION=7.5 -D__X86_64__ -m64 -U __linux__ \
	-U __FreeBSD__ -U __sun__ -D__ASSEMBLY__ -D __Unikraft__ -DUK_CODENAME="Rhea" -DUK_VERSION=0.4 \
	-DUK_FULLVERSION=0.4.0~71946ac  -mtune=generic -no-pie -DKVMPLAT link64.lds.S -o link64.lds \
	-Wp,-MD,.link64.lds.d

final_link:
	gcc -r -nostdinc -nostdlib -Wl,--omagic -Wl,-r -Wl,-d -Wl,--build-id=none -no-pie  -Wl,-m,elf_x86_64 \
	entry64.o setup.o cpu_features.common.o print.o console.o serial_console.o vga_console.o shutdown.o \
	cpu_native.o alloc.o build/libnolibc.o -Wl,--start-group     -Wl,--end-group -o static_kernel.ld.o
	objcopy -w -G kvmos_* -G _libkvmplat_entry static_kernel.ld.o static_kernel.o
	gcc  -nostdinc -nostdlib -Wl,--omagic -Wl,--build-id=none  -no-pie  -Wl,-m,elf_x86_64 -Wl,-dT,link64.lds \
	-Wl,-T,ukdebug/extra.ld -Wl,-T,vfscore/extra.ld  static_kernel.o -o static_kernel.dbg

setup:
	gcc -I./include -I./nolibc/include -I./plat/kvm/include -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector -fno-omit-frame-pointer \
	-fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" -DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7 \
	-O0 -fno-optimize-sibling-calls -fno-tree-vectorize -no-pie -D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks \
	-fno-asynchronous-unwind-tables  -mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf \
	-fno-builtin-sprintf -fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf -fno-builtin-vscanf \
	-fno-builtin-vfscanf -fno-builtin-vsscanf -DKVMPLAT       -g3 -D__LIBNAME__=libkvmplat -D__BASENAME__=setup.c \
	-c setup.c -o setup.o -Wp,-MD,.setup.o.d

print:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=print.c  -c print.c -o print.o -Wp,-MD,.print.o.d

console:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=console.c  -c console.c -o console.o -Wp,-MD,.console.o.d

serial_console:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=serial_console.c  -c serial_console.c -o serial_console.o -Wp,-MD,.serial_console.o.d

vga_console:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=vga_console.c  -c vga_console.c -o vga_console.o -Wp,-MD,.vga_console.o.d

shutdown:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=shutdown.c  -c shutdown.c -o shutdown.o -Wp,-MD,.shutdown.o.d

cpu_native:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=cpu_native.c  -c cpu_native.c -o cpu_native.o -Wp,-MD,.cpu_native.o.d

alloc:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=alloc.c  -c alloc.c -o alloc.o -Wp,-MD,.alloc.o.d

time:
	gcc   -nostdinc -nostdlib -U __linux__ -U __FreeBSD__ -U __sun__ -fno-stack-protector \
	-fno-omit-frame-pointer -fno-tree-sra -Wall -Wextra -D __Unikraft__ -DUK_CODENAME="Rhea" \
	-DUK_VERSION=0.4 -DUK_FULLVERSION=0.4.0~62c61a7  -O0 -fno-optimize-sibling-calls \
	-fno-tree-vectorize -no-pie -I./include -I./nolibc/include -I./plat/kvm/include \
	-D__X86_64__ -m64 -mno-red-zone -fno-reorder-blocks -fno-asynchronous-unwind-tables \
	-mtune=generic  -DCC_VERSION=7.5  -fno-builtin-printf -fno-builtin-fprintf -fno-builtin-sprintf \
	-fno-builtin-snprintf -fno-builtin-vprintf -fno-builtin-vfprintf -fno-builtin-vsprintf \
	-fno-builtin-vsnprintf -fno-builtin-scanf -fno-builtin-fscanf -fno-builtin-sscanf \
	-fno-builtin-vscanf -fno-builtin-vfscanf -fno-builtin-vsscanf  -D__IN_LIBUKDEBUG__      -g3 \
	-D__LIBNAME__=libukdebug -D__BASENAME__=time.c  -c time.c -o time.o -Wp,-MD,.time.o.d

nolib:
	make -f Makefile_nolibc

run:
	sudo qemu-system-x86_64 -enable-kvm -nographic -device isa-debug-exit -kernel $(exec)

run_gdb:
	sudo qemu-system-x86_64 -s -S -cpu host -enable-kvm -m 128 -nodefaults -no-acpi -display none -serial stdio -device isa-debug-exit -kernel $(exec) -append verbose

clean:
	rm -rf *.o .*.o.d link64.lds .link64.lds.d static_kernel.*
	make clean -f Makefile_nolibc
