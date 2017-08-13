FROM openjdk:jdk-windowsservercore

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# ***************************************************************************************
# Setup Jenkins
# ***************************************************************************************

ENV HOME /jenkins
ENV JENKINS_VERSION 2.70
ENV JENKINS_HOME /jenkins
RUN mkdir \jenkins
RUN Invoke-WebRequest -Uri $('https://updates.jenkins-ci.org/download/war/{0}/jenkins.war' -f $env:JENKINS_VERSION) -UseBasicParsing -OutFile /jenkins.war

VOLUME C:\\jenkins

EXPOSE 8080

# ***************************************************************************************
# Setup chocolatey 
# ***************************************************************************************

RUN iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); \
	choco feature enable -n allowGlobalConfirmation; \
	choco -v;
	
# ***************************************************************************************
# Setup microsoft build tools
# ***************************************************************************************


RUN choco install microsoft-build-tools --allow-empty-checksums -version 15.0.26228.0; \
	choco install microsoft-build-tools --allow-empty-checksums -version 14.0.25420.1; \
	choco install visualstudio2017buildtools --allow-empty-checksums -version 15.2.26430.20170605; \
	choco install visualstudio2017testagent --allow-empty-checksums -version 15.2.26430.20170605; \
	[Environment]::SetEnvironmentVariable('Path', $('{0};C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\MSBuild\\15.0\\Bin' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine); \
	[Environment]::SetEnvironmentVariable('Path', $('{0};C:\Program Files (x86)\Microsoft Visual Studio\2017\TestAgent\Team Tools\Dynamic Code Coverage Tools' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine); \
	[Environment]::SetEnvironmentVariable('Path', $('{0};C:\Program Files (x86)\Microsoft Visual Studio\2017\TestAgent\Common7\IDE' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine);
	
# ***************************************************************************************
# Setup 7zip
# ***************************************************************************************

RUN choco install 7zip.portable; \ 
	7za;

# ***************************************************************************************
# Setup .net 4.7
# ***************************************************************************************

RUN choco install \
    dotnet4.7 \
    --confirm \
    --limit-output; \
	exit 0
	
    
# ***************************************************************************************
# Setup nuget
# ***************************************************************************************

RUN mkdir nuget; \
	\
	Invoke-WebRequest "https://dist.nuget.org/win-x86-commandline/latest/nuget.exe" -OutFile /nuget/nuget.exe; \
	\
	[Environment]::SetEnvironmentVariable('Path', $('{0};C:\\nuget' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	\
# we updated PATH, so we should refresh our local value
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine); 
    #\
	#\
    #nuget help;


# ***************************************************************************************
# Setup OpenCover
# ***************************************************************************************

RUN nuget install OpenCover -Version 4.6.519; \
	[Environment]::SetEnvironmentVariable('Path', $('{0};c:\\OpenCover.4.6.519\\tools' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine); \
	nuget install OpenCoverToCoberturaConverter -Version 0.2.6; \
	[Environment]::SetEnvironmentVariable('Path', $('{0};c:\\OpenCoverToCoberturaConverter.0.2.6.0\\tools' -f ($env:Path)), [EnvironmentVariableTarget]::Machine); \
	$env:PATH = [Environment]::GetEnvironmentVariable('PATH', [EnvironmentVariableTarget]::Machine);


CMD [ "java", "-jar", "c:\\jenkins.war" ]