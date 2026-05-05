FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/Services/SharedLibrary/SharedLibrary.csproj src/Services/SharedLibrary/
COPY src/Services/CollaboratorService.Domain/CollaboratorService.Domain.csproj src/Services/CollaboratorService.Domain/
COPY src/Services/CollaboratorService.Application/CollaboratorService.Application.csproj src/Services/CollaboratorService.Application/
COPY src/Services/CollaboratorService.Infrastructure/CollaboratorService.Infrastructure.csproj src/Services/CollaboratorService.Infrastructure/
COPY src/Services/CollaboratorService.API/CollaboratorService.API.csproj src/Services/CollaboratorService.API/

RUN dotnet restore src/Services/CollaboratorService.API/CollaboratorService.API.csproj

# Copy everything and build
COPY src/Services/SharedLibrary/ src/Services/SharedLibrary/
COPY src/Services/CollaboratorService.Domain/ src/Services/CollaboratorService.Domain/
COPY src/Services/CollaboratorService.Application/ src/Services/CollaboratorService.Application/
COPY src/Services/CollaboratorService.Infrastructure/ src/Services/CollaboratorService.Infrastructure/
COPY src/Services/CollaboratorService.API/ src/Services/CollaboratorService.API/

RUN dotnet publish src/Services/CollaboratorService.API/CollaboratorService.API.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "CollaboratorService.API.dll"]
