# https://hub.docker.com/_/microsoft-dotnet-core-aspnet/
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1.2-alpine3.11 as base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1.102-alpine3.11 AS build
RUN apk add --no-cache nodejs nodejs-npm

# TODO: Docker cache w. dotnet restore?
COPY . .
RUN dotnet build
RUN dotnet test

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS runtime
COPY --from=build /app .
ENTRYPOINT ["dotnet", "vue.dll"]
