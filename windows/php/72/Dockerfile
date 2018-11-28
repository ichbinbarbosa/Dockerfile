FROM microsoft/iis AS dllcompile

# Promote powershell as default shell
#
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

ADD https://download.microsoft.com/download/6/A/A/6AA4EDFF-645B-48C5-81CC-ED5963AEAD48/vc_redist.x64.exe /vc_redist.exe

ADD https://github.com/git-for-windows/git/releases/download/v2.19.2.windows.1/Git-2.19.2-64-bit.exe /git.exe

ADD https://windows.php.net/downloads/releases/php-7.2.12-nts-Win32-VC15-x64.zip /php.zip

ADD https://github.com/Microsoft/msphpsql/releases/download/v5.4.0-preview/Windows-7.2.zip /msphpsql.zip

RUN Expand-Archive -Path C:\php.zip -DestinationPath C:\php

RUN Expand-Archive -Path C:\msphpsql.zip -DestinationPath C:\msphpsql

RUN Copy-Item C:\msphpsql\Windows-7.2\x64\php_pdo_sqlsrv_72_nts.dll C:\php\ext

RUN Copy-Item C:\msphpsql\Windows-7.2\x64\php_sqlsrv_72_nts.dll C:\php\ext

RUN Copy-Item C:\PHP\php.ini-production C:\PHP\php.ini


FROM microsoft/iis

COPY --from=dllcompile /php /php
COPY --from=dllcompile /git.exe /git.exe
COPY --from=dllcompile /vc_redist.exe /vc_redist.exe


# Enable required IIS Features
#
RUN dism.exe /Online /Enable-Feature /FeatureName:IIS-CGI /All

# Install VC Redist 14
# Install GIT
#
RUN C:\vc_redist.exe /quiet /install && del C:\vc_redist.exe

RUN C:\git.exe /quiet /install && del C:\git.exe 

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


# Optional: Add a starter page
RUN powershell.exe -Command '<?php phpinfo(); ?>' | Out-File C:\inetpub\wwwroot\index.php -Encoding UTF8