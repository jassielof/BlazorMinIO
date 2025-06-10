FROM mcr.microsoft.com/dotnet/sdk:9.0-alpine AS build

WORKDIR /src

# Copy csproj and restore dependencies
COPY ["../BlazorMinIO.csproj", "."]
RUN dotnet restore

# Copy all files and build
COPY .. .
RUN dotnet build "BlazorMinIO.csproj" -c Release -o /app/build
RUN dotnet publish "BlazorMinIO.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:9.0-alpine AS final
WORKDIR /app
EXPOSE 8080
EXPOSE 443

# Copy published app
COPY --from=build /app/publish .

# Set environment variables
ENV ASPNETCORE_URLS=http://+:8080
ENV ASPNETCORE_ENVIRONMENT=Production

# Create non-root user for security
RUN adduser -u 5678 -D appuser && chown -R appuser /app
USER appuser

ENTRYPOINT ["dotnet", "BlazorMinIO.dll"]
