FROM mcr.microsoft.com/powershell:nanoserver-ltsc2022
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
