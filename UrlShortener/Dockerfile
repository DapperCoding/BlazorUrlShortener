#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
ARG cstring
ENV DefaultConnectionString $cstring
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["UrlShortener.csproj", ""]
RUN dotnet restore "./UrlShortener.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "UrlShortener.csproj" -c Release -o /app/build

FROM build AS publish
ENV DefaultConnectionString $cstring
RUN dotnet publish "UrlShortener.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UrlShortener.dll"]