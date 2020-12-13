# >>
# >> Build Application
# >>

# >> .Net Framework 3.5 SDK on Windows Server Core 2019
#
# .NET Framework SDK
# https://hub.docker.com/_/microsoft-dotnet-framework-sdk/
#
# DotNetVersions project can not build by .NET Framework 4.8.
# Require .NET Framework 3.5.
FROM mcr.microsoft.com/dotnet/framework/sdk:3.5-20201110-windowsservercore-ltsc2019 AS Build

# >> Check PowerShell Version
#
# How to Check your PowerShell Version (All the Ways!)
# https://adamtheautomator.com/check-powershell-version/
#
# CMD [ "PowerShell", "(Get-Host).Version" ]
# Major  Minor  Build  Revision
# -----  -----  -----  --------
# 5      1      17763  1490

# >> Download Git for Windows
#
# Git - Downloading Package
# https://git-scm.com/download/win
#   Click here to download manually
#   https://github.com/git-for-windows/git/releases/download/v2.29.2.windows.3/Git-2.29.2.3-64-bit.exe
#
# PowerShell Invoke-WebRequest
# https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-5.1
RUN PowerShell Invoke-WebRequest -URI https://github.com/git-for-windows/git/releases/download/v2.29.2.windows.3/Git-2.29.2.3-64-bit.exe -OutFile git.exe

# >> Install Git for Windows
#
# Silent or Unattended Installation
# https://github.com/git-for-windows/git/wiki/Silent-or-Unattended-Installation
#
# CMD [ "C:\\Program Files\\Git\\bin\\git.exe", "--version" ]
# git version 2.29.2.windows.3
RUN ./git.exe /VERYSILENT /NORESTART /NOCANCEL /SP- /CLOSEAPPLICATIONS /RESTARTAPPLICATIONS /COMPONENTS="icons,ext\reg\shellhere,assoc,assoc_sh"

# >> Get Project from GitHub
#
# How to: Determine which .NET Framework versions are installed
# https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
#   DotNetVersions
#   https://github.com/jmalarcon/DotNetVersions
WORKDIR /DotNetVersions
RUN [ "C:\\Program Files\\Git\\bin\\git.exe", "init" ]
RUN [ "C:\\Program Files\\Git\\bin\\git.exe", "pull", "https://github.com/jmalarcon/DotNetVersions" ]

# >> Build Project
#
# Build and run the sample locally with MSBuild
# https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/dotnetapp/README.md#build-and-run-the-sample-locally-with-msbuild
RUN msbuild /t:restore
RUN msbuild /p:Configuration=Release

# >> Run Application
#
# DotNetVersions
# https://github.com/jmalarcon/DotNetVersions
#   This is useful for informational purposes,
#   but if for any reason you need to run it in a batch file
#   and get just the versions without stopping execution,
#   you can use the /b, -b or --b switch :
#
# CMD [ "C:\\DotNetVersions\\bin\\Release\\DotNetVersions.exe", "-b" ]
# 2.0.50727.4927 Service Pack 2
# 3.0.30729.4926 Service Pack 2
# 3.5.30729.4926 Service Pack 1
# 4.0.0.0
# 4.8.03761

# >>
# >> Build Release Image
# >>
#
# Use multi-stage builds
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-multi-stage-builds
#
# CMD [ "C:\\app\\DotNetVersions.exe", "-b" ]
# 4.0.0.0
# 4.7.03190
FROM mcr.microsoft.com/windows/servercore:ltsc2019
WORKDIR /app
COPY --from=Build /DotNetVersions/bin/Release .
CMD [ "C:\\app\\DotNetVersions.exe", "-b" ]