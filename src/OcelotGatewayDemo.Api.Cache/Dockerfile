FROM mcr.microsoft.com/dotnet/aspnet:7.0-focal AS base
WORKDIR /app
EXPOSE 6000

ENV ASPNETCORE_URLS=http://+:6000

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:7.0-focal AS build
WORKDIR /src
COPY ["src/OcelotGatewayDemo.Api.Cache/OcelotGatewayDemo.Api.Cache.csproj", "src/OcelotGatewayDemo.Api.Cache/"]
RUN dotnet restore "src/OcelotGatewayDemo.Api.Cache/OcelotGatewayDemo.Api.Cache.csproj"
COPY . .
WORKDIR "/src/src/OcelotGatewayDemo.Api.Cache"
RUN dotnet build "OcelotGatewayDemo.Api.Cache.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "OcelotGatewayDemo.Api.Cache.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OcelotGatewayDemo.Api.Cache.dll"]
