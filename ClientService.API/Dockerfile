#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 30021
EXPOSE 30001

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["ClientService.API/ClientService.API.csproj", "ClientService.API/"]
COPY ["ClientService.Data/ClientService.Data.csproj", "ClientService.Data/"]
RUN dotnet restore "ClientService.API/ClientService.API.csproj"
COPY . .
WORKDIR "/src/ClientService.API"
RUN dotnet build "ClientService.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ClientService.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ClientService.API.dll"]
