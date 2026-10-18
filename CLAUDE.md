# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project status

`ApiEcommerce` is an ASP.NET Core Web API (.NET 9) learning project. It is still at the `dotnet new webapi --use-controllers` template stage: the only endpoint is the sample `WeatherForecastController`. There is no test project, no EF Core, and no database connection string configured yet.

## Commands

Run from the repo root (`ApiEcommerce.sln` is the default solution, per `.vscode/settings.json`):

```bash
dotnet build                                      # build the solution
dotnet run --project ApiEcommerce                 # http profile: http://localhost:5190
dotnet run --project ApiEcommerce --launch-profile https   # https://localhost:7114 + http://localhost:5190
dotnet watch --project ApiEcommerce               # hot reload
```

- OpenAPI document (Development only): `GET /openapi/v1.json`. This uses `Microsoft.AspNetCore.OpenApi` (`AddOpenApi`/`MapOpenApi`). There is no Swagger UI package.
- `ApiEcommerce/ApiEcommerce.http` has sample requests for the VS Code / Rider HTTP client. Add new endpoints there when you create them.

## Architecture

- Single project, `ApiEcommerce/`, using minimal hosting in `Program.cs`. Services are registered on `builder.Services` and the pipeline is configured after `builder.Build()`.
- Controllers live in `ApiEcommerce/Controllers/`, use `[ApiController]` with `[Route("[controller]")]`, and are discovered through `AddControllers()` / `MapControllers()`.
- Nullable reference types and implicit usings are enabled. Namespaces are file-scoped (`namespace ApiEcommerce.Controllers;`).

## Dev environment

`.devcontainer/` uses docker-compose to start two services:
- `app`: the .NET 9 devcontainer image. The workspace is mounted at `/workspaces/api`.
- `sql`: SQL Server 2022 Developer (container `sqlserver2022`) on port 1433, with user `sa` and the password set in `.devcontainer/docker-compose.yml`. From inside the devcontainer, the host name is `sql`. From the host machine, use `localhost,1433`.

The database is meant for the upcoming e-commerce data layer. When EF Core is added, put the connection string in `appsettings.Development.json` or user-secrets.
