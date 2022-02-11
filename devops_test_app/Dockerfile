#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["devops_test_app/devops_test_app.csproj", "devops_test_app/"]
RUN dotnet restore "devops_test_app/devops_test_app.csproj"
COPY . .
WORKDIR "/src/devops_test_app"
RUN dotnet build "devops_test_app.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "devops_test_app.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "devops_test_app.dll"]