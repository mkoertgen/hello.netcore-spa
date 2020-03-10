# hello.netcore-spa

Comparing dotnet core 3+ fullstack/spa options

## Angular

![Angular](img/angular.jpg)

Bootstrap using `dotnet new angular -n angular`

## React

![React](img/react.jpg)

Bootstrap using `dotnet new react -n react`

## Vue

![Vue](img/vue.jpg)

Bootstrap using

```console
dotnet new -i SoftwareAteliers.AspNetCoreVueStarter
mkdir vue
cd vue
dotnet new vue
```

## Aurelia

![Aurelia](img/aurelia.jpg)

Bootstrap using

```console
dotnet new -i Aurelia.DotNet.Templates
dotnet new aurelia -n aurelia
```

**NOTE:** At the time of writing the command fails and needs some manual rework & cleanup. Also, the template only supports `netcoreapp2.2` but can probably manually updated to `netcoreapp3.1`.

## References

- [Create an ASP.NET Core 3.0 Angular SPA project with Docker support](https://medium.com/swlh/create-an-asp-net-core-3-0-angular-spa-web-application-with-docker-support-86e8c15796aa)

### ServiceStack

- ServiceStack [NetCoreTemplates](https://github.com/NetCoreTemplates)

**NOTE:** Ruled out here because too opinionated (gulp, complete own stack, diverging from Microsoft standards).