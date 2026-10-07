FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BookApp/BookApp.csproj", "BookApp/"]
COPY ["BizLogic/BizLogic.csproj", "BizLogic/"]
COPY ["BizDbAccess/BizDbAccess.csproj", "BizDbAccess/"]
COPY ["DataLayer/DataLayer.csproj", "DataLayer/"]
COPY ["ServiceLayer/ServiceLayer.csproj", "ServiceLayer/"]
RUN dotnet restore "BookApp/BookApp.csproj"
COPY . .
WORKDIR "/src/BookApp"
RUN dotnet build "./BookApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BookApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BookApp.dll"]
