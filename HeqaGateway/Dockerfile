FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5233

ENV ASPNETCORE_URLS=http://+:5233

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["HeqaGateway/HeqaGateway.csproj", "HeqaGateway/"]
RUN dotnet restore "HeqaGateway/HeqaGateway.csproj"
COPY . .
WORKDIR "/src/HeqaGateway"
RUN dotnet build "HeqaGateway.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release

RUN dotnet publish "HeqaGateway.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HeqaGateway.dll"]
