FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files for restore
COPY src/Services/SharedLibrary/SharedLibrary.csproj src/Services/SharedLibrary/
COPY src/Services/LabelsService.Domain/LabelsService.Domain.csproj src/Services/LabelsService.Domain/
COPY src/Services/LabelsService.Application/LabelsService.Application.csproj src/Services/LabelsService.Application/
COPY src/Services/LabelsService.Infrastructure/LabelsService.Infrastructure.csproj src/Services/LabelsService.Infrastructure/
COPY src/Services/LabelsService.API/LabelsService.API.csproj src/Services/LabelsService.API/
# LabelsService.API also references CollaboratorService projects
COPY src/Services/CollaboratorService.Domain/CollaboratorService.Domain.csproj src/Services/CollaboratorService.Domain/
COPY src/Services/CollaboratorService.Application/CollaboratorService.Application.csproj src/Services/CollaboratorService.Application/
COPY src/Services/CollaboratorService.Infrastructure/CollaboratorService.Infrastructure.csproj src/Services/CollaboratorService.Infrastructure/

RUN dotnet restore src/Services/LabelsService.API/LabelsService.API.csproj

# Copy everything and build
COPY src/Services/SharedLibrary/ src/Services/SharedLibrary/
COPY src/Services/LabelsService.Domain/ src/Services/LabelsService.Domain/
COPY src/Services/LabelsService.Application/ src/Services/LabelsService.Application/
COPY src/Services/LabelsService.Infrastructure/ src/Services/LabelsService.Infrastructure/
COPY src/Services/LabelsService.API/ src/Services/LabelsService.API/
COPY src/Services/CollaboratorService.Domain/ src/Services/CollaboratorService.Domain/
COPY src/Services/CollaboratorService.Application/ src/Services/CollaboratorService.Application/
COPY src/Services/CollaboratorService.Infrastructure/ src/Services/CollaboratorService.Infrastructure/

RUN dotnet publish src/Services/LabelsService.API/LabelsService.API.csproj -c Release -o /app/publish --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "LabelsService.API.dll"]
