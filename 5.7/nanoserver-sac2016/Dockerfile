#Changed from https://github.com/nanoserver/IIS-PHP-MYSQL/blob/master/Dockerfile/Dockerfile

FROM microsoft/nanoserver:latest

ENV MYSQL_ROOT_PASSWORD="root" \
  MYSQL_ROOT_HOST="%" \
  MYSQL_DATABASE="" \
  MYSQL_USER="" \
  MYSQL_PASSWORD="" \
  MYSQL_ALLOW_EMPTY_PASSWORD="no" \
  MYSQL_RANDOM_ROOT_PASSWORD="no" \
  MYSQL_ONETIME_PASSWORD="no" \
  DOCKER_NEW_RUN="no"

RUN mkdir docker-entrypoint-initdb.d

# Download Links:
ARG mysql_download_url="https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.21-winx64.zip"

RUN powershell -Command \
  $ErrorActionPreference = 'Stop' ; \
  $ProgressPreference = 'SilentlyContinue' ; \
  Invoke-WebRequest -Uri $env:mysql_download_url -OutFile c:\mysql.zip ; \
  Expand-Archive -Path 'c:\mysql.zip' -DestinationPath 'c:\' ; \
  Rename-Item -Path 'c:\mysql-5.7.21-winx64' -NewName 'c:\mysql' ; \
  Remove-Item -Path 'c:\mysql.zip' -Force

RUN Setx PATH /M %PATH%;c:\\mysql\\bin & \
  c:\mysql\bin\mysqld.exe --install-manual MySQL

VOLUME c:\\mysql\\data

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

COPY InitializeDatabase.ps1 .
ENTRYPOINT .\\InitializeDatabase.ps1 -Verbose \
  -MYSQL_ROOT_PASSWORD $env:MYSQL_ROOT_PASSWORD \
  -MYSQL_ROOT_HOST $env:MYSQL_ROOT_HOST \
  -MYSQL_DATABASE $env:MYSQL_DATABASE \
  -MYSQL_USER $env:MYSQL_USER \
  -MYSQL_PASSWORD $env:MYSQL_PASSWORD \
  -MYSQL_ALLOW_EMPTY_PASSWORD $env:MYSQL_ALLOW_EMPTY_PASSWORD \
  -MYSQL_RANDOM_ROOT_PASSWORD $env:MYSQL_RANDOM_ROOT_PASSWORD \
  -MYSQL_ONETIME_PASSWORD $env:MYSQL_ONETIME_PASSWORD \
  -DOCKER_NEW_RUN $env:DOCKER_NEW_RUN;

EXPOSE 3306
CMD ["MySQL", "--user=root", "-p"]