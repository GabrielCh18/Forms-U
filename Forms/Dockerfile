# Usamos las imágenes oficiales de Linux para .NET 10
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copiamos el archivo de proyecto y restauramos dependencias
COPY ["Forms/Forms.csproj", "Forms/"]
RUN dotnet restore "./Forms/Forms.csproj"

# Copiamos el resto del código y compilamos
COPY . .
WORKDIR "/src/Forms"
RUN dotnet build "./Forms.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publicamos la aplicación
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Forms.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Fase final: copiamos lo publicado y definimos el punto de entrada
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Forms.dll"]