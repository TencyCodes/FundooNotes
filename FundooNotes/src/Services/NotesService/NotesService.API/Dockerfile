FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/Services/SharedLibrary/SharedLibrary.csproj src/Services/SharedLibrary/
COPY src/Services/NotesService/NotesService.Domain/NotesService.Domain.csproj src/Services/NotesService/NotesService.Domain/
COPY src/Services/NotesService/NotesService.Application/NotesService.Application.csproj src/Services/NotesService/NotesService.Application/
COPY src/Services/NotesService/NotesService.Infrastructure/NotesService.Infrastructure.csproj src/Services/NotesService/NotesService.Infrastructure/
COPY src/Services/NotesService/NotesService.API/NotesService.API.csproj src/Services/NotesService/NotesService.API/

RUN dotnet restore src/Services/NotesService/NotesService.API/NotesService.API.csproj

# Copy everything and build
COPY src/Services/SharedLibrary/ src/Services/SharedLibrary/
COPY src/Services/NotesService/ src/Services/NotesService/

RUN dotnet publish src/Services/NotesService/NotesService.API/NotesService.API.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "NotesService.API.dll"]
