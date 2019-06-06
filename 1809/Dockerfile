# escape=`

ARG tag=1809

FROM mcr.microsoft.com/windows/servercore:$tag AS servercore

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV MHOLT_ARCHIVER_VERSION 2.0
ENV MHOLT_ARCHIVER_SHA256 c23eab86a8f77e446f2b5dbb03305ac9acfb5187108a0090a4bcb3d908fa9785

RUN Invoke-WebRequest $('https://github.com/mholt/archiver/releases/download/v{0}/archiver_windows_amd64.exe' -f $env:MHOLT_ARCHIVER_VERSION) -OutFile archiver.exe -UseBasicParsing ; `
    if ((Get-FileHash archiver.exe -Algorithm sha256).Hash -ne $env:MHOLT_ARCHIVER_SHA256) { Write-Error 'SHA256 mismatch' }

ENV PROMETHEUS_VERSION 2.9.2

RUN Invoke-WebRequest $('https://github.com/prometheus/prometheus/releases/download/v{0}/prometheus-{0}.windows-amd64.tar.gz' -f $env:PROMETHEUS_VERSION) -OutFile 'prometheus.tar.gz' -UseBasicParsing
RUN .\archiver.exe open prometheus.tar.gz
RUN Rename-Item -Path $('C:/prometheus-{0}.windows-amd64' -f $env:PROMETHEUS_VERSION) -NewName 'C:/prometheus'
RUN Invoke-WebRequest https://raw.githubusercontent.com/jvoeller/prometheus-windows/master/prometheus.yml -Outfile prometheus.yml -usebasicparsing
RUN Remove-Item C:/prometheus/prometheus.yml
RUN Move C:/prometheus.yml C:/prometheus/prometheus.yml

FROM mcr.microsoft.com/windows/nanoserver:$tag

COPY --from=servercore /windows/system32/netapi32.dll /windows/system32/netapi32.dll
COPY --from=servercore /prometheus/                   /prometheus/

EXPOSE 9090

WORKDIR /prometheus
RUN mkdir storage

CMD [ `
    "prometheus.exe", `
    "--config.file=/prometheus/prometheus.yml", `
    "--storage.tsdb.path=/prometheus/storage", `
    "--web.console.libraries=/prometheus/console_libraries", `
    "--web.console.templates=/prometheus/consoles" `
]
