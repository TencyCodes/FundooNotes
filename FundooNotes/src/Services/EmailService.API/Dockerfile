FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/Services/EmailService.API/EmailService.API.csproj src/Services/EmailService.API/

RUN dotnet restore src/Services/EmailService.API/EmailService.API.csproj

# Copy everything and build
COPY src/Services/EmailService.API/ src/Services/EmailService.API/

RUN dotnet publish src/Services/EmailService.API/EmailService.API.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "EmailService.API.dll"]
