# https://hub.docker.com/_/microsoft-dotnet-aspnet
FROM mcr.microsoft.com/dotnet/aspnet:5.0.2-alpine3.12-amd64 as base
WORKDIR /app
EXPOSE 80
EXPOSE 443

#--------------------------------------------------
FROM zenika/alpine-chrome:with-node AS build-angular-base

# see
# - https://github.com/Zenika/alpine-chrome/blob/master/with-chromedriver/Dockerfile
# - https://github.com/Zenika/alpine-chrome/blob/master/with-node/Dockerfile
USER root
RUN apk add --no-cache chromium-chromedriver
USER chrome

FROM build-angular-base as build-frontend
WORKDIR /usr/src/app
COPY ["ClientApp/package*.json", "./"]
RUN npm ci

COPY ["ClientApp/", "./"]
#RUN npm run lint
RUN npm run test:ci
RUN npm run e2e
RUN npm run build

#--------------------------------------------------
# https://hub.docker.com/_/microsoft-dotnet-sdk/
FROM mcr.microsoft.com/dotnet/sdk:5.0.102-alpine3.12-amd64 AS build
WORKDIR /usr/src/app
ENV Configuration Release
ENV DOCKER_BUILD True
# TODO: Docker cache w. dotnet restore?
COPY . .
RUN dotnet build
RUN dotnet test

#--------------------------------------------------
FROM build AS publish
RUN dotnet publish -o /app
RUN mkdir -p /app/ClientApp/dist/
COPY --from=build-frontend /usr/src/app/dist/ /app/ClientApp/dist/

#--------------------------------------------------
FROM base AS runtime
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "angular.dll"]
