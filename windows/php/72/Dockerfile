##
# Download Files
##
FROM microsoft/iis AS dllcompile

# Promote powershell as default shell
#
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

ADD https://download.microsoft.com/download/6/A/A/6AA4EDFF-645B-48C5-81CC-ED5963AEAD48/vc_redist.x64.exe /vc_redist.exe

ADD https://windows.php.net/downloads/releases/php-7.2.12-nts-Win32-VC15-x64.zip /php.zip

RUN Expand-Archive -Path /php.zip -DestinationPath /php

RUN Copy-Item /php/php.ini-production /php/php.ini

##
# Build principal image
##
FROM microsoft/iis

COPY --from=dllcompile /php /php
COPY --from=dllcompile /vc_redist.exe /vc_redist.exe

# Enable required IIS Features
#
RUN dism.exe /Online /Enable-Feature /FeatureName:IIS-CGI /All

# Install VC Redist 14
#
RUN C:\vc_redist.exe /quiet /install && del C:\vc_redist.exe

# Configure IIS
# Configure system PATH
#
RUN %windir%\system32\inetsrv\appcmd.exe set config /section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'] \
    && %windir%\system32\inetsrv\appcmd.exe set config /section:system.webServer/handlers /+[name='PHP_via_FastCGI',path='*.php',verb='*',modules='FastCgiModule',scriptProcessor='c:\php\php-cgi.exe',resourceType='Either'] \
    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /[fullPath='c:\php\php-cgi.exe'].instanceMaxRequests:10000 \
    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000'] \
    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'].environmentVariables.[name='PHPRC',value='C:\php'] \
    && %windir%\system32\inetsrv\appcmd.exe set config /section:defaultDocument /enabled:true /+files.[value='index.php'] \
    && setx PATH /M %PATH%;C:\php \
    && setx PHP /M "C:\php" \
    && del C:\inetpub\wwwroot\* /Q
