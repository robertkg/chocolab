ARG TAG=ltsc2019
FROM mcr.microsoft.com/windows/servercore:$TAG
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN net user /add ProGet Passw0rd! /fullname:'ProGet' ; \
    net localgroup Administrators ProGet /add

RUN $ProgressPreference = 'SilentlyContinue'; \
    Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?linkid=829176'  -OutFile sqlexpress.exe ; \
    Start-Process -Wait -FilePath .\sqlexpress.exe -ArgumentList /qs, /x:setup ; \
    .\setup\setup.exe /q /ACTION=Install /INSTANCENAME=Inedo /FEATURES=SQLEngine /UPDATEENABLED=0 /SQLSVCACCOUNT='NT AUTHORITY\System' /SQLSYSADMINACCOUNTS='BUILTIN\ADMINISTRATORS' /TCPENABLED=1 /NPENABLED=0 /IACCEPTSQLSERVERLICENSETERMS ; \
    Remove-Item -Recurse -Force sqlexpress.exe, setup

RUN SQLCMD.EXE -S localhost\sqlexpress -Q 'CREATE DATABASE [ProGet] COLLATE SQL_Latin1_General_CP1_CI_AS'
RUN SQLCMD.EXE -S localhost\sqlexpress -Q "sp_grantlogin 'NT AUTHORITY\Network Service'" -d master
