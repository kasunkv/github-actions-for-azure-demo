FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["GitHubActions.Azure.Web/GitHubActions.Azure.Web.csproj", "GitHubActions.Azure.Web/"]
RUN dotnet restore "GitHubActions.Azure.Web/GitHubActions.Azure.Web.csproj"
COPY . .
WORKDIR "/src/GitHubActions.Azure.Web"
RUN dotnet build "GitHubActions.Azure.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GitHubActions.Azure.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GitHubActions.Azure.Web.dll"]