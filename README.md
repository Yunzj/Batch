# Batch
@echo off&title 纭之家批处理教程---一键获取已安全软件列表
set "file=%~dp0软件信息列表.txt"
set "file2=%~dp0软件信息列表2.txt"
set a=0
echo  已安装软件列表>"%file%"
IF /I "%PROCESSOR_IDENTIFIER:~0,3%"=="x86" goto 32

echo 正在扫描64位软件。。。
echo  已安装64位软件列表 >>"%file%"
call :64 "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Uninstall"
call :64 "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall"
echo 正在扫描32位软件。。。
echo  已安装32位软件列表 >>"%file%"
call :64x32 "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall"
call :64x32 "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Uninstall"
:ok
echo 正在采用方案二 扫描...
call :322 "HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Installer\Products"
echo;
echo;      扫描结束。。。
echo;
echo;
if exist "%file%" START "" "%file%"
if exist "%file2%" START "" "%file2%"
exit

:cl
set /a a+=1
goto :eof



:64
for /f "tokens=1,2,* " %%i in ('REG QUERY "%~1" /reg:64') DO (
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayName" /reg:64 2^>nul ^| find /i "DisplayName" 2^>nul') do (echo 软件名：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayVersion" /reg:64 2^>nul ^| find /i "DisplayVersion" 2^>nul') do echo 版号：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "InstallDate" /reg:64 2^>nul ^| find /i "InstallDate" 2^>nul') do echo 安装日期：%%~c
 echo ----------------------------------------------------------------------------------------
 )
)>>"%file%"
goto :eof

:64x32
for /f "tokens=1,2,* " %%i in ('REG QUERY "%~1" /reg:32') DO (
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayName" /reg:32 2^>nul ^| find /i "DisplayName" 2^>nul') do (echo 软件名：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayVersion" /reg:32 2^>nul ^| find /i "DisplayVersion" 2^>nul') do echo 版号：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "InstallDate" /reg:32 2^>nul ^| find /i "InstallDate" 2^>nul') do echo 安装日期：%%~c
 echo ----------------------------------------------------------------------------------------
 )
)>>"%file%"
goto :eof

:32
echo 正在扫描32位软件。。。
echo  已安装32位软件列表 >>"%file%"
for /f "tokens=1,2,* " %%i in ('REG QUERY "%~1"') DO (
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayName" 2^>nul ^| find /i "DisplayName" 2^>nul') do (echo 软件名：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "DisplayVersion" 2^>nul ^| find /i "DisplayVersion" 2^>nul') do echo 版号：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "InstallDate" 2^>nul ^| find /i "InstallDate" 2^>nul') do echo 安装日期：%%~c
 echo ----------------------------------------------------------------------------------------
 )
)>>"%file%"
goto :ok

:322
echo  已安装软件列表 >>"%file2%"
for /f "tokens=1,2,* " %%i in ('REG QUERY "%~1"') DO (
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "ProductName" 2^>nul ^| find /i "ProductName" 2^>nul') do (echo 软件名：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "Version" 2^>nul ^| find /i "Version" 2^>nul') do echo 版号：%%~c
 for /f "tokens=1,2,* " %%a in ('REG QUERY "%%~i" /V "InstallDate" 2^>nul ^| find /i "InstallDate" 2^>nul') do echo 安装日期：%%~c
 echo ----------------------------------------------------------------------------------------
 )
)>>"%file2%"
goto :eof
