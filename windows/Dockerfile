# Use Windows Server Core or Nano Server as base
FROM mcr.microsoft.com/windows/servercore:ltsc2025

# Install PowerShell 7
SHELL ["powershell", "-Command"]
RUN Invoke-WebRequest -Uri https://github.com/PowerShell/PowerShell/releases/download/v7.4.0/PowerShell-7.4.0-win-x64.msi \
    -OutFile ps7.msi; \
    Start-Process msiexec.exe -ArgumentList '/i', 'ps7.msi', '/quiet' -Wait; \
    Remove-Item ps7.msi

SHELL ["pwsh", "-Command"]


# Prepare image
RUN New-Item -ItemType Directory -Path "C:/tempinst" -Force
RUN Invoke-WebRequest -Uri "https://github.com/git-for-windows/git/releases/download/v2.53.0.windows.2/Git-2.53.0.2-64-bit.exe" -OutFile "C:/tempinst/gitinst.exe"
RUN Start-Process "C:/tempinst/gitinst.exe" -ArgumentList '/SILENT' -Wait
RUN Remove-Item "C:/tempinst/gitinst.exe"

RUN Invoke-WebRequest -Uri "https://storage.googleapis.com/flutter_infra_release/releases/stable/windows/flutter_windows_3.41.6-stable.zip" -OutFile "C:/tempinst/flutter.zip"
RUN Expand-Archive -Path "C:/tempinst/flutter.zip" -DestinationPath "C:/"
RUN Remove-Item "C:/tempinst/flutter.zip"
ENV PATH="C:\flutter\bin;${PATH}"



# Verify installations
RUN flutter --version
