FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/Services/SharedLibrary/SharedLibrary.csproj src/Services/SharedLibrary/
COPY src/Services/UserService.Domain/UserService.Domain.csproj src/Services/UserService.Domain/
COPY src/Services/UserService.Application/UserService.Application.csproj src/Services/UserService.Application/
COPY src/Services/UserService.Infrastructure/UserService.Infrastructure.csproj src/Services/UserService.Infrastructure/
COPY src/Services/UserService.API/UserService.API.csproj src/Services/UserService.API/

RUN dotnet restore src/Services/UserService.API/UserService.API.csproj

# Copy everything and build
COPY src/Services/SharedLibrary/ src/Services/SharedLibrary/
COPY src/Services/UserService.Domain/ src/Services/UserService.Domain/
COPY src/Services/UserService.Application/ src/Services/UserService.Application/
COPY src/Services/UserService.Infrastructure/ src/Services/UserService.Infrastructure/
COPY src/Services/UserService.API/ src/Services/UserService.API/

RUN dotnet publish src/Services/UserService.API/UserService.API.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "UserService.API.dll"]
