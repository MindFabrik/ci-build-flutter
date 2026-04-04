FROM mcr.microsoft.com/powershell:7.4-windowsservercore-ltsc2022
SHELL ["pwsh", "-Command"]

# Install GIT
RUN New-Item -ItemType Directory -Path "C:/tempinst" -Force
RUN Invoke-WebRequest -Uri "https://github.com/git-for-windows/git/releases/download/v2.53.0.windows.2/Git-2.53.0.2-64-bit.exe" -OutFile "C:/tempinst/gitinst.exe"
RUN Start-Process "C:/tempinst/gitinst.exe" -ArgumentList '/VERYSILENT', '/NORESTART', '/NOCANCEL', '/SP-', '/CLOSEAPPLICATIONS', '/RESTARTAPPLICATIONS', '/NOICONS' -Wait
RUN Remove-Item "C:/tempinst/gitinst.exe"

# Explicitly restore pwsh to PATH after Git install may have clobbered it
RUN $machinePath = [System.Environment]::GetEnvironmentVariable('PATH', 'Machine'); \
    if ($machinePath -notlike '*PowerShell*') { \
      [System.Environment]::SetEnvironmentVariable('PATH', $machinePath + ';C:\Program Files\PowerShell\7', 'Machine'); \
    }

# Install Flutter
RUN Invoke-WebRequest -Uri "https://storage.googleapis.com/flutter_infra_release/releases/stable/windows/flutter_windows_3.41.6-stable.zip" -OutFile "C:/tempinst/flutter.zip"
RUN Expand-Archive -Path "C:/tempinst/flutter.zip" -DestinationPath "C:/"
RUN Remove-Item "C:/tempinst/flutter.zip"

# Configure Environment
RUN [System.Environment]::SetEnvironmentVariable('PATH', \
      'C:\flutter\bin;' + [System.Environment]::GetEnvironmentVariable('PATH', 'Machine'), \
      'Machine')
ENV POWERSHELL_TELEMETRY_OPTOUT=1
RUN git config --system core.longpaths true
RUN git config --system --add safe.directory C:/flutter
RUN git config --system --add safe.directory '*'

# Configure Flutter
RUN flutter --version

CMD ["pwsh", "-NoLogo"]