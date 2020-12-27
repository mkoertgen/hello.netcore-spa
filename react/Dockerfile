# https://hub.docker.com/_/microsoft-dotnet-aspnet
FROM mcr.microsoft.com/dotnet/aspnet:5.0.1-alpine3.12-amd64 as base
WORKDIR /app
EXPOSE 80
EXPOSE 443

#--------------------------------------------------
FROM node:15.5.0-alpine3.12 as build-frontend
WORKDIR /usr/src/app
COPY ["ClientApp/package*.json", "./"]
RUN npm install

COPY ["ClientApp/", "./"]
#RUN npm run lint
#RUN npm run test:ci
RUN npm run build

#--------------------------------------------------
# https://hub.docker.com/_/microsoft-dotnet-sdk/
FROM mcr.microsoft.com/dotnet/sdk:5.0.101-alpine3.12-amd64 AS build
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
RUN mkdir -p /app/ClientApp/build/
COPY --from=build-frontend /usr/src/app/build/ /app/ClientApp/build/

#--------------------------------------------------
FROM base AS runtime
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "react.dll"]
