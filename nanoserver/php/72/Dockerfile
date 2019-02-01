##
# Download Files
##
FROM nanoserver/apache24 AS dllcompile

# Promote powershell as default shell
#
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

RUN Invoke-WebRequest -Uri https://jbit.es/files/vcruntime140.dll -OutFile /vcruntime140.dll 

RUN Invoke-WebRequest -Uri https://windows.php.net/downloads/releases/php-7.2.14-nts-Win32-VC15-x64.zip -OutFile /php.zip ; \
    Expand-Archive -Path /php.zip -DestinationPath /php ; \
    Copy-Item /php/php.ini-production /php/php.ini   


##
# Image ichbinbarbosa/php-ns:72 
##
FROM nanoserver/apache24

COPY --from=dllcompile /php /php
COPY --from=dllcompile /vcruntime140.dll /Windows/System32

RUN mkdir sites-enabled ; \
    Add-Content /Apache24/conf/httpd.conf "IncludeOptional sites-enabled/*.conf"

##
# Set PHP environment variable. Enable php command line.
#
RUN setx PATH /M %PATH%;C:\php ; \
    && setx PHP /M "C:\php"

# Enable required IIS Features
#
#RUN dism.exe /Online /Enable-Feature /FeatureName:IIS-CGI /All

# Install VC Redist 14
#
#RUN C:\vc_redist.exe /quiet /install && del C:\vc_redist.exe

# Configure IIS
# Configure system PATH
#
#RUN %windir%\system32\inetsrv\appcmd.exe set config /section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'] \
#    && %windir%\system32\inetsrv\appcmd.exe set config /section:system.webServer/handlers /+[name='PHP_via_FastCGI',path='*.php',verb='*',modules='FastCgiModule',scriptProcessor='c:\php\php-cgi.exe',resourceType='Either'] \
#    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /[fullPath='c:\php\php-cgi.exe'].instanceMaxRequests:10000 \
#    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000'] \
#    && %windir%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+[fullPath='c:\php\php-cgi.exe'].environmentVariables.[name='PHPRC',value='C:\php'] \
#    && %windir%\system32\inetsrv\appcmd.exe set config /section:defaultDocument /enabled:true /+files.[value='index.php'] \
#    && setx PATH /M %PATH%;C:\php \
#    && setx PHP /M "C:\php" \
#    && del C:\inetpub\wwwroot\* /Q
