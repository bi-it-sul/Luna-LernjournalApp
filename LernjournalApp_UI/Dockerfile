# Base image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
EXPOSE 5044
EXPOSE 8080
EXPOSE 9999

# Stage for building the application
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy all projects and restore dependencies
COPY ["LernjournalApp_UI/LernjournalApp_UI.csproj", "LernjournalApp_UI/"]

RUN dotnet restore "./LernjournalApp_UI/LernjournalApp_UI.csproj"

# Copy all source files and build, excluding conflicting appsettings
COPY . .
RUN rm -f LernjournalApp_UI/appsettings*.json
WORKDIR "/src/LernjournalApp_UI"
RUN dotnet build "./LernjournalApp_UI.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Stage for publishing the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./LernjournalApp_UI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final stage for running the application
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LernjournalApp_UI.dll"]