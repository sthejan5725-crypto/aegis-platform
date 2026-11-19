# Multi-stage Dockerfile for AEGIS ASP.NET Core Web API (.NET 10)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

COPY ["Aegis.slnx", "./"]
COPY ["src/Aegis.Domain/Aegis.Domain.csproj", "src/Aegis.Domain/"]
COPY ["src/Aegis.Application/Aegis.Application.csproj", "src/Aegis.Application/"]
COPY ["src/Aegis.Infrastructure/Aegis.Infrastructure.csproj", "src/Aegis.Infrastructure/"]
COPY ["src/Aegis.Api/Aegis.Api.csproj", "src/Aegis.Api/"]
RUN dotnet restore "src/Aegis.Api/Aegis.Api.csproj"

COPY . .
WORKDIR "/src/src/Aegis.Api"
RUN dotnet publish "Aegis.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
RUN apt-get update && apt-get install -y --no-install-recommends libgomp1 && rm -rf /var/lib/apt/lists/*
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "Aegis.Api.dll"]
