#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["PracticumServer.Api/PracticumServer.Api.csproj", "PracticumServer.Api/"]
COPY ["PracticumServer.Service/PracticumServer.Service.csproj", "PracticumServer.Service/"]
COPY ["PracticumServer.Core/PracticumServer.Core.csproj", "PracticumServer.Core/"]
COPY ["PracticumServer.Data/PracticumServer.Data.csproj", "PracticumServer.Data/"]
RUN dotnet restore "./PracticumServer.Api/./PracticumServer.Api.csproj"
COPY . .
WORKDIR "/src/PracticumServer.Api"
RUN dotnet build "./PracticumServer.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./PracticumServer.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PracticumServer.Api.dll"]