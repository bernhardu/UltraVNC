
Not intended for inclusion in upstream repository (in this state).


Some notes about parallel builds, and the development environment.



:: Start developer prompt
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





:: Debug build i686
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln

msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\vncviewer\vncviewer_vs2017.sln





:: Debug build amd64
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=x64 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln

msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=x64 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\vncviewer\vncviewer_vs2017.sln





mkdir c:\uvnc\32\xp
mkdir c:\uvnc\64\xp
mkdir C:\uvnc\Users\rudi\Desktop\git_ultravnc\Files
::set SIGNTOOL="C:\Program Files (x86)\Windows Kits\8.1\bin\x86\signtool.exe"
set SIGNTOOL=echo Skipping

:: Release build i686
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Release ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln

msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Release ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\vncviewer\vncviewer_vs2017.sln





:: Release build amd64
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=x64 ^
  /p:Configuration=Release ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln

msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=x64 ^
  /p:Configuration=Release ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  C:\temp\2022-09-19\UltraVNC\vncviewer\vncviewer_vs2017.sln





:: Some git notes
export EDITOR=nano

git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

git format-patch --keep-subject -o ../patches_$(date +%Y-%m-%d_%H-%M-%S) -5

git am --keep-cr ../patches/0001*

git rebase --interactive d548fac4102c8




:: Notepad++
::"Find in Files" Filter:     * !*.obj !*.pch !*.sbr !*.bsc !*.lib !*.pdb !*.map !*.tlog !\.git* !*.dll !*.exe !*.suo !*.dsm




:: Debug build i686 (with llvm/clang-cl)
msbuild ^
  /p:WindowsTargetPlatformVersion=10.0.22000.0 ^
  /p:PlatformToolset=v141 ^
  /p:Platform=Win32 ^
  /p:Configuration=Debug ^
  /p:BuildInParallel=true -maxcpucount:16 /p:CL_MPCount=16 /p:BrowseInformation=false /p:MinimalRebuild=false ^
  /p:CLToolExe=clang-cl.exe /p:CLToolPath="c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\Llvm\bin" ^
  C:\temp\2022-09-19\UltraVNC\winvnc\winvncVS2017.sln
