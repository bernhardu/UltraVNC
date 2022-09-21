
# cmake, mingw-w64 and nasm should be installed

git clone https://github.com/bernhardu/UltraVNC.git



mkdir obj_x86_64
cd    obj_x86_64
cmake --toolchain ../UltraVNC/cmake/toolchain-mingw32-w64_x86_64.cmake ../UltraVNC/cmake
make -j`nproc`
cp -a /usr/lib/gcc/x86_64-w64-mingw32/10-win32/libstdc++-6.dll    .
cp -a /usr/lib/gcc/x86_64-w64-mingw32/10-win32/libgcc_s_seh-1.dll .


mkdir obj_i686
cd    obj_i686
cmake --toolchain ../UltraVNC/cmake/toolchain-mingw32-w64_i686.cmake ../UltraVNC/cmake
make -j`nproc`
cp -a /usr/lib/gcc/i686-w64-mingw32/10-win32/libstdc++-6.dll      .
cp -a /usr/lib/gcc/i686-w64-mingw32/10-win32/libgcc_s_dw2-1.dll   .



# To build with LLVM-mingw one might just put it into the path before
export PATH=/path/to/llvm-mingw_build/bin:$PATH





# Windows, Visual Studio Command prompt

mkdir obj
cd    obj
cmake -G Ninja ../UltraVNC/cmake
ninja
