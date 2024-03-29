# Dockerfile for Puppet Bolt managed Core 2019 server
ARG TAG=ltsc2019
FROM mcr.microsoft.com/windows/servercore:$TAG
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Set up WinRM for HTTPS
RUN $cert = New-SelfSignedCertificate -DnsName "winrm" -CertStoreLocation Cert:\LocalMachine\My; \
    winrm create winrm/config/Listener?Address=*+Transport=HTTPS ('@{Hostname=\"winrm\"; CertificateThumbprint=\"' + $cert.Thumbprint + '\"}'); \
    winrm set winrm/config/service/Auth '@{Basic=\"true\"}'

# Create Bolt account
RUN net user /add Bolt Passw0rd! /fullname:'Puppet Bolt' ; \
    net localgroup Administrators Bolt /add

# https://chocolatey.org/install
# Chocolatey prompts on the first plan, doing first time install in dockerfile instead
# Network mode inherit host DNS server. Set DNS servers explicit during install
RUN Set-DnsClientServerAddress -InterfaceIndex 4 -ServerAddresses 8.8.8.8, 8.8.4.4; \
    Set-ExecutionPolicy Bypass -Scope Process -Force; \
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; \
    iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'));

# Import chocolab self-signed certificate
ADD cert c:/temp
RUN Import-Certificate -FilePath C:\temp\chocolab.local.cer -CertStoreLocation Cert:\LocalMachine\Root\ 1>$null; \
    Remove-Item C:\temp\chocolab.local.cer;
