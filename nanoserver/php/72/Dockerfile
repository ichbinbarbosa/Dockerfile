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

##
# Override httpd.conf
#
ADD extras/httpd.conf /Apache24/conf/httpd.conf

RUN mkdir /Apache24/conf/sites-enabled

##
# Set PHP environment variable. Enable php command line.
#
# @todo FIX THIS RUN. Dont work =(
#
RUN setx PATH /M %PATH%;C:\php ; \
    setx PHP /M "C:\php"
