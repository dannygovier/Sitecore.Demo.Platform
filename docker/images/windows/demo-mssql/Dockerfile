# escape=`

ARG BASE_IMAGE
ARG SOLUTION_IMAGE

FROM ${SOLUTION_IMAGE} as solution
FROM $BASE_IMAGE as data

COPY --from=solution /solution/db /output/

COPY docker/images/windows/demo-mssql/data/ /output

RUN C:\DeployDatabases.ps1 -ResourcesDirectory "/output/security"
RUN C:\DeployDatabases.ps1 -ResourcesDirectory "/output/data"
RUN C:\DeployDatabases.ps1 -ResourcesDirectory "/output/descendants"

RUN Remove-Item "/output" -Recurse

FROM data as production

ENV USER_PASSWORD="b"
ENV ADMIN_USER_NAME="sitecore\superuser"
ENV SITECORE_ADMIN_PASSWORD="b"
ENV DISABLE_DEFAULT_ADMIN=FALSE
ENV EXM_BASE_URL=http://cd
ENV SQL_SERVER="mssql"
ENV START_OVERRIDE_SCRIPTS="start-override-platform-windows.ps1"

COPY /docker/images/windows/demo-mssql/sql /sql

COPY /docker/images/windows/demo-mssql/HashPassword.ps1 /docker/images/windows/demo-mssql/start-override.ps1 /docker/images/windows/demo-mssql/start-override-platform-windows.ps1 /

CMD .\start-override -sa_password $env:sa_password -ACCEPT_EULA $env:ACCEPT_EULA -attach_dbs \"$env:attach_dbs\" -Verbose