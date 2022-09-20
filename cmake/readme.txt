
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



















========
 
git clone https://github.com/bernhardu/UltraVNC.git

cd UltraVNC
git remote add upstream https://github.com/ultravnc/UltraVNC.git

git fetch upstream
git merge upstream/main main
git push

git checkout --track origin/development
git merge upstream/development development
git push


git checkout mingw


export BUILD_ARCH=x86_64
#export BUILD_ARCH=x86_64; export VARIANT=_clang_asan; export CMAKEARG="-Dasan=true"
#export BUILD_ARCH=i686
#export BUILD_ARCH=i686; export VARIANT=_clang_asan; export CMAKEARG="-Dasan=true"
cd /home/bernhard/data/entwicklung/2023/ultravnc/2023-04-30
export CCACHE_DIR=$PWD/ccache
mkdir obj_${BUILD_ARCH}${VARIANT} -p
cd    obj_${BUILD_ARCH}${VARIANT}
cmake --toolchain ../UltraVNC/cmake/toolchain-mingw32-w64_$BUILD_ARCH.cmake -DCMAKE_C_COMPILER_LAUNCHER=ccache -DCMAKE_CXX_COMPILER_LAUNCHER=ccache ${CMAKEARG} ../UltraVNC/cmake
make -j`nproc`

cp -a /usr/lib/gcc/x86_64-w64-mingw32/12-win32/libstdc++-6.dll    .
cp -a /usr/lib/gcc/x86_64-w64-mingw32/12-win32/libgcc_s_seh-1.dll .
cp -a /usr/lib/gcc/i686-w64-mingw32/12-win32/libstdc++-6.dll      .
cp -a /usr/lib/gcc/i686-w64-mingw32/12-win32/libgcc_s_dw2-1.dll   .


LLVM:
export PATH=/home/bernhard/data/entwicklung/2023/llvm-mingw/2023-05-02/llvm-mingw_build/bin:$PATH

cp -a /home/bernhard/data/entwicklung/2023/llvm-mingw/2023-05-02/llvm-mingw_inst/x86_64-w64-mingw32/bin/libclang_rt.asan_dynamic-x86_64.dll .
cp -a /home/bernhard/data/entwicklung/2023/llvm-mingw/2023-05-02/llvm-mingw_inst/x86_64-w64-mingw32/bin/libc++.dll .
cp -a /home/bernhard/data/entwicklung/2023/llvm-mingw/2023-05-02/llvm-mingw_inst/x86_64-w64-mingw32/bin/libunwind.dll .








===========
Windows:
- Install e.g. Visual Studio Express 2022
- Install MFC components (otherwise an error about a missing afxres.h gets written.)


"C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Visual Studio 2022\Visual Studio Tools\VC\x64 Native Tools Command Prompt for VS 2022.lnk"




:: Get always english messages (seems to need to install english language pack in VS setup?)
set DOTNET_CLI_UI_LANGUAGE=en
set VSLANG=1033
chcp 850



:: Enable parallel build (todo: maybe not all are needed/helpful, find out which)
set UseMultiToolTask=true
set EnforceProcessCountAcrossBuilds=true
set MultiProcMaxCount=16
set CL=/MP



::set SIGNTOOL="C:\Program Files (x86)\Windows Kits\8.1\bin\x86\signtool.exe"
set SIGNTOOL=echo Skipping


mkdir c:\uvnc\32\xp
mkdir c:\uvnc\64\xp


::set _C=Debug
set _C=Release
::set _A=Win32
set _A=x64

set _P=^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=%_A% ^
  /p:Configuration=%_C% ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 ^
  /p:BrowseInformation=false ^
  /p:MinimalRebuild=false ^
  /t:Clean;Build
set CL=/MP

cd /d C:\temp\UltraVNC

msbuild %_P% C:\temp\UltraVNC\winvnc\winvncVS2017.sln
msbuild %_P% C:\temp\UltraVNC\vncviewer\vncviewer_vs2017.sln







Windows, build with cmake:
==========================
# Start developer prompt
mkdir c:\temp\obj_x86_64
cd /d c:\temp\obj_x86_64
cmake -S C:\temp\UltraVNC\cmake -B C:\temp\obj_x86_64

::set _C=Debug
set _C=Release
::set _A=Win32
set _A=x64

set _P=^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=%_A% ^
  /p:Configuration=%_C% ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 ^
  /p:BrowseInformation=false ^
  /p:MinimalRebuild=false ^
  /t:Clean;Build
set CL=/MP

msbuild %_P% c:\temp\obj_x86_64\UltraVNC.sln






mkdir c:\temp\obj_x86_64_asan
cd /d c:\temp\obj_x86_64_asan
cmake -Dasan=true -S C:\temp\UltraVNC\cmake -B C:\temp\obj_x86_64_asan

set _C=Debug
::set _C=Release
::set _A=Win32
set _A=x64

set _P=^
  /p:Platform=%_A% ^
  /p:Configuration=%_C% ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 ^
  /p:BrowseInformation=false ^
  /p:MinimalRebuild=false ^
  /t:Clean;Build
set CL=/MP

msbuild %_P% c:\temp\obj_x86_64_asan\UltraVNC.sln

start
copy "c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.35.32215\bin\Hostx64\x64\clang_rt.asan_dbg_dynamic-x86_64.dll" winvnc\Debug\
copy "c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.35.32215\bin\Hostx64\x64\clang_rt.asan_dbg_dynamic-x86_64.dll" vncviewer\Debug\
winvnc\Debug\winvnc.exe
vncviewer\Debug\vncviewer.exe











:: Some git notes
export EDITOR=nano

git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

git format-patch --keep-subject -o ../patches_$(date +%Y-%m-%d_%H-%M-%S) -5

git am --keep-cr ../patches/0001*

git rebase --interactive d548fac4102c8




:: Notepad++
::"Find in Files" Filter:     * !*.obj !*.pch !*.sbr !*.bsc !*.lib !*.pdb !*.map !*.bmp !*.tlog !\.git* !*.dll !*.exe !*.suo !*.dsm












:: Debug build i686 (with llvm/clang-cl)
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  /p:CLToolExe=clang-cl.exe /p:CLToolPath="c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\Llvm\bin" ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln
