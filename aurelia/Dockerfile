# https://hub.docker.com/_/microsoft-dotnet-core-aspnet/
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1.2-alpine3.11 as base
WORKDIR /app
EXPOSE 80
EXPOSE 443

#--------------------------------------------------
FROM node:13.10.1-alpine3.11 as build-frontend
WORKDIR /usr/src/app
COPY ["ClientApp/package*.json", "./"]
RUN npm install

COPY ["ClientApp/", "./"]
#RUN npm run lint
#RUN npm run test:ci
RUN npm run build:prod

#--------------------------------------------------
FROM mcr.microsoft.com/dotnet/core/sdk:3.1.102-alpine3.11 AS build
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
ENTRYPOINT ["dotnet", "aurelia.dll"]
