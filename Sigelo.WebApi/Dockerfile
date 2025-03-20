FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5063

ENV ASPNETCORE_URLS=http://+:5063

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Sigelo.WebApi/Sigelo.WebApi.csproj", "Sigelo.WebApi/"]
RUN dotnet restore "Sigelo.WebApi/Sigelo.WebApi.csproj"
COPY . .
WORKDIR "/src/Sigelo.WebApi"
RUN dotnet build "Sigelo.WebApi.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Sigelo.WebApi.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Sigelo.WebApi.dll"]
