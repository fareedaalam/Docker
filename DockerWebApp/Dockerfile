#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Copy the published output of your application to the container
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["DockerWebApp.csproj", ""]
RUN dotnet restore "./DockerWebApp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "DockerWebApp.csproj" -c Release -o /app/build

# Publish the application to the container
FROM build AS publish
RUN dotnet publish "DockerWebApp.csproj" -c Release -o /app/publish

# Set up the entry point for the container
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerWebApp.dll"]