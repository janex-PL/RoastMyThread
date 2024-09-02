FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /
COPY ["src/RoastMyThread.Web/RoastMyThread.Web.csproj", "src/RoastMyThread.Web/"]
RUN dotnet restore "src/RoastMyThread.Web/RoastMyThread.Web.csproj"
COPY . .
WORKDIR "/src/RoastMyThread.Web"
RUN dotnet build "RoastMyThread.Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "RoastMyThread.Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RoastMyThread.Web.dll"]
