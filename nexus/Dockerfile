ARG TAG=ltsc2019
FROM mcr.microsoft.com/windows/servercore:$TAG
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ADD https://download.sonatype.com/nexus/3/latest-win64.zip C:/
RUN mkdir C:\Nexus; \
    Expand-Archive C:\latest-win64.zip -DestinationPath C:\Nexus

    