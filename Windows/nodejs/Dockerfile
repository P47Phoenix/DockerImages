FROM microsoft/windowsservercore:latest

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest -Uri ('https://nodejs.org/dist/v6.11.1/node-v6.11.1-win-x64.zip') -UseBasicParsing -OutFile /nodejs.zip

RUN expand-archive -path c:\\nodejs.zip -destinationpath c:\\nodejs

RUN [Environment]::SetEnvironmentVariable('Path', $('{0};c:\\nodejs\node-v6.11.1-win-x64' -f ($env:Path)),'Machine')

RUN del c:\\nodejs.zip

RUN node -v

RUN npm -v
