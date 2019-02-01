##
# Download Files
##
FROM ichbinbarbosa/php-ns:72 AS dllcompile

# Promote powershell as default shell
#
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

RUN Invoke-WebRequest -Uri https://github.com/Microsoft/msphpsql/releases/download/v5.5.0-preview/Windows-7.2.zip -OutFile /msphpsql.zip ; \
    Expand-Archive -Path /msphpsql.zip -DestinationPath /msphpsql ; \
    Add-Content /php/php.ini "extension=php_pdo_sqlsrv_72_nts" ; \
    Add-Content /php/php.ini "extension=php_sqlsrv_72_nts"

##
# Image ichbinbarbosa/php-ns:72-mssql
##
FROM ichbinbarbosa/php-ns:72

COPY --from=dllcompile /php/php.ini /php/php.ini
COPY --from=dllcompile /msphpsql/Windows-7.2/x64/php_pdo_sqlsrv_72_nts.dll /php/ext
COPY --from=dllcompile /msphpsql/Windows-7.2/x64/php_sqlsrv_72_nts.dll /php/ext
