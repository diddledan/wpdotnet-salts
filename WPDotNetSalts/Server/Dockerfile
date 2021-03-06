#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:5.0-buster AS build
WORKDIR /src
COPY ["WPDotNetSalts/Server/Server.csproj", "WPDotNetSalts/Server/"]
COPY ["WPDotNetSalts/WPDotNetSalts/WPDotNetSalts.msbuildproj", "WPDotNetSalts/WPDotNetSalts/"]
RUN dotnet restore "WPDotNetSalts/Server/Server.csproj"
COPY . .
WORKDIR "/src/WPDotNetSalts/Server"
RUN dotnet build "Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Server.dll"]