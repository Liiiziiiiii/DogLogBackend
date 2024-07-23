# Use the standard .NET SDK image
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# Set the working directory
WORKDIR /src

# Copy the csproj file and restore the dependencies
COPY ["ApiProject.csproj", "ApiProject/"]
RUN dotnet nuget locals all --clear
RUN dotnet restore "ApiProject/ApiProject.csproj"

# Copy the rest of the source code and build the project
COPY . .
WORKDIR "/src/ApiProject"
RUN dotnet build "ApiProject.csproj" -c Release -o /app/build

# Publish the application
FROM build AS publish
RUN dotnet publish "ApiProject.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Use the ASP.NET runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ApiProject.dll"]
