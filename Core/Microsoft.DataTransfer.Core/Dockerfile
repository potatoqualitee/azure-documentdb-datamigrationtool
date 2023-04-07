#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Core/Microsoft.DataTransfer.Core/Microsoft.DataTransfer.Core.csproj", "Core/Microsoft.DataTransfer.Core/"]
COPY ["Interfaces/Microsoft.DataTransfer.Interfaces/Microsoft.DataTransfer.Interfaces.csproj", "Interfaces/Microsoft.DataTransfer.Interfaces/"]
RUN dotnet restore "Core/Microsoft.DataTransfer.Core/Microsoft.DataTransfer.Core.csproj"
COPY . .
WORKDIR "/src/Core/Microsoft.DataTransfer.Core"
RUN dotnet build "Microsoft.DataTransfer.Core.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Microsoft.DataTransfer.Core.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Microsoft.DataTransfer.Core.dll"]