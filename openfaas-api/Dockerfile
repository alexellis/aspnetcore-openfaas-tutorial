FROM  ghcr.io/openfaas/of-watchdog:0.9.15 as watchdog

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY openfaas-api.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app .
COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

ENV ASPNETCORE_HTTP_PORTS=80
ENV fprocess="dotnet openfaas-api.dll"
ENV upstream_url="http://127.0.0.1:80"
ENV mode="http"

ENTRYPOINT ["fwatchdog"]