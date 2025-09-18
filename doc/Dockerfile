# STAGE 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia csproj e restaura dependências
COPY ["doc.csproj", "./"]
RUN dotnet restore "doc.csproj"

# Copia todo o código
COPY . ./

# Build
RUN dotnet build -c Release -o /app/build

# STAGE 2: Publish
FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

# STAGE 3: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV ASPNETCORE_HTTP_PORTS=5001
EXPOSE 5001

ENTRYPOINT ["dotnet", "doc.dll"]