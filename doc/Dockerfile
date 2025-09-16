#STAGE 1: Build
FROM mcr:microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src #cria e abre diretorio (mkdir + cd)

#restore
COPY ["src/dep/doc.csproj", "dep/"]
RUN dotnet restore 'deploy_docker/deploy_docker.csproj'

#build
COPY ["src/dep", "dep/"]
WORKDIR /src/dep
RUN dotnet build 'doc.csproj' -c Release -o /app/build

#STAGE 2: Publish
FROM build as publish
RUN dotnet publish 'doc.csproj' -c Release -o /app/publish

#STAGE 3: Run
FROM mcr:microsoft.com/dotnet/aspnet:8.0
ENV ASPNETCORE_HTTP_PORTS=5001
EXPOSE 5001
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT [ "dotnet", "doc.dll"]