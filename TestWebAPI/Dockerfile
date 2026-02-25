FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
ENV ASPNETCORE_URLS=http://0.0.0.0:8080

# non-root user
RUN adduser --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TestWebAPI/TestWebAPI.csproj", "TestWebAPI/"]
RUN dotnet restore "TestWebAPI/TestWebAPI.csproj"
COPY . .
WORKDIR "/src/TestWebAPI"
RUN dotnet publish "./TestWebAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "TestWebAPI.dll"]