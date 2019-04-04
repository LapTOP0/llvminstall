# llvminstall
安装LLVM4.0.0防止内存溢出的方法

下载LLVM：http://llvm.org/releases/4.0.0/llvm-4.0.0.src.tar.xz
下载Clang： http://llvm.org/releases/4.0.0/cfe-4.0.0.src.tar.xz
下载Compiler RT：http://llvm.org/releases/4.0.0/compiler-rt-4.0.0.src.tar.xz

解压

tar -xf ../cfe-4.0.0.src.tar.xz -C tools &&
tar -xf ../compiler-rt-4.0.0.src.tar.xz -C projects &&

mv tools/cfe-4.0.0.src tools/clang &&
mv projects/compiler-rt-4.0.0.src projects/compiler-rt

安装
mkdir -v build &&
cd       build &&

第一次安装时内存溢出，分配了20GB交换空间仍然没什么用
参考https://stackoverflow.com/questions/44807589/fatal-error-building-the-llvm-source-code-in-ubuntu

CC=gcc CXX=g++                              \
cmake -DCMAKE_INSTALL_PREFIX=/usr           \
-DLLVM_PARALLEL_LINK_JOBS=1	\
-DLLVM_LINK_LLVM_DYLIB=true   	\
      -DLLVM_ENABLE_FFI=ON                  \
      -DCMAKE_BUILD_TYPE=Release            \
      -DLLVM_BUILD_LLVM_DYLIB=ON            \
      -DLLVM_TARGETS_TO_BUILD="host;AMDGPU" \
      -Wno-dev ..                           &&
make

参考：http://www.linuxfromscratch.org/blfs/view/8.1/general/llvm.html
