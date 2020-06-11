# introduce
risc-v 是一个免费和开源的一套精简指令集，类似x86, arm9, mips指令集。  
开始于 University of California, Berkeley. 2015年对外公布, 号称是为  
下一个50年计算和创新的铺垫。

# 编译工具
git clone --recursive https://github.com/riscv/riscv-gnu-toolchain  
problem:  
error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.  
fatal: The remote end hung up unexpectedly  
fatal: early EOF  
fatal: index-pack failed  

solve:  
git config --add core.compression -1
git config --global http.postBuffer 2000000000

problem:  
fatal: unable to access 'https://boringssl.googlesource.com/boringssl/': Failed to connect to boringssl.googlesource.com port 443: Connection timed out
fatal: clone of 'https://boringssl.googlesource.com/boringssl' into submodule path 'boringssl' failed
Failed to recurse into submodule path 'qemu/roms/edk2/CryptoPkg/Library/OpensslLib/openssl'
Failed to recurse into submodule path 'qemu/roms/edk2'
Failed to recurse into submodule path 'qemu'  

solve:    
gvim ./qemu/roms/edk2/CryptoPkg/Library/OpensslLib/openssl/.gitmodules
#url = https://boringssl.googlesource.com/boringssl  
url =  https://github.com/google/boringssl.git  

./configure --prefix=/opt/riscv
make

git submodule sync --recursive  
git submodule update --init --recursive  

error: RPC failed; curl 56 GnuTLS recv error (-9): A TLS packet with unexpected length was received.
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed

sovle: need to 翻墙

编译环境所需要的lib
sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev

编译生成工具链
./configure --prefix=/opt/riscv  
make  

# 构建仿真环境
git clone https://github.com/riscv/riscv-qemu.git  
cd riscv-qemu/  
git checkout riscv-qemu-3.0  
./configure  
make -j4  
make install  

problem: "ERROR: glib-2.12 required to compile QEMU"  
solve: sudo apt-get install libglib2.0-dev  

problem: "/usr/bin/ld: cannot find -lgcrypt" problem  
solve: sudo apt-get install libgcrypt11-dev  

problem:  
make[1]: flex: Command not found  
	 BISON dtc-parser.tab.c
make[1]: bison: Command not found  
	 LEX dtc-lexer.lex.c  
make[1]: flex: Command not found
solve: sudo apt-get install flex bison  

# hello world
riscv64-unknown-elf-gcc main.c  
qemu-riscv64 a.out    
remember add your riscv gcc&qemu to system PATH

### qemu-system-riscv32 -kernel build/bin/rv32imac/qemu-sifive_e/hello -S -s -machine sifive_e
VNC server running on 127.0.0.1:5900  
solved: sudo apt-get install libsdl2-dev
