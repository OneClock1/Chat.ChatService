#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Chat/Chat.API/Chat.API.csproj", "Chat/Chat.API/"]
COPY ["Chat/Chat.Persistence/Chat.Persistence.csproj", "Chat/Chat.Persistence/"]
COPY ["Chat/Chat.Domain/Chat.Domain.csproj", "Chat/Chat.Domain/"]
COPY ["Chat/Chat.Infrastructure/Chat.Infrastructure.csproj", "Chat/Chat.Infrastructure/"]
RUN dotnet restore "Chat/Chat.API/Chat.API.csproj"
COPY . .
WORKDIR "/src/Chat/Chat.API"
RUN dotnet build "Chat.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Chat.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Chat.API.dll"]