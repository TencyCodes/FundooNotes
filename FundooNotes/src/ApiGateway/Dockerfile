FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/ApiGateway/ApiGateway.csproj src/ApiGateway/

RUN dotnet restore src/ApiGateway/ApiGateway.csproj

# Copy everything and build
COPY src/ApiGateway/ src/ApiGateway/

RUN dotnet publish src/ApiGateway/ApiGateway.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "ApiGateway.dll"]
