# Blazor Web Dev Container

This repository demonstrates how to use development containers with a .NET 8 Blazor web application. It provides a consistent, reproducible development environment that works across different machines.

## What are Dev Containers?

Development containers (or dev containers) are Docker containers specifically configured for development purposes. They provide:

- **Consistent development environments** across different machines and team members
- **Isolated dependencies** that don't interfere with your local system
- **Pre-configured tools** ready to use without manual installation
- **Environment parity** between development and production

## What's Included

This dev container includes:

- .NET 8 SDK for Blazor development
- Git (latest version) for source control
- Node.js and npm for front-end development
- ESLint for JavaScript linting
- HTTPS certificate configuration
- VS Code extensions optimized for Blazor development

## Project Structure

```
BlazorWebDevContainer/
├── .devcontainer/               # Dev container configuration
│   ├── devcontainer.json        # Container settings and features
│   └── Dockerfile               # Custom container image definition
├── src/                         # Source code directory
│   └── BlazorApp/               # Blazor web application
│       ├── Components/          # Razor components
│       │   ├── Layout/          # Layout components
│       │   └── Pages/           # Page components
│       ├── wwwroot/             # Static web assets
│       └── Program.cs           # Application entry point
└── BlazorWebDevContainer.sln    # Solution file
```

## Getting Started

### Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Opening the Project in a Dev Container

1. Clone this repository
2. Open the folder in Visual Studio Code
3. When prompted, click "Reopen in Container" or:
   - Press F1, select "Dev Containers: Reopen in Container"

VS Code will build the container and connect to it. This may take a few minutes the first time.

### Running the Blazor Application

Once inside the dev container:

```bash
cd src/BlazorApp
dotnet watch run
```

This will start the Blazor application with hot reload enabled. Visit:
- https://localhost:5001 for the HTTPS endpoint
- http://localhost:5000 for the HTTP endpoint

## Development Features

### HTTPS Development

The dev container is configured with HTTPS certificates. The application can be accessed securely through https://localhost:5001.

### VS Code Extensions

The dev container comes with pre-configured VS Code extensions:

- C# for language support
- Blazor WASM Companion for Blazor development
- .NET Core Test Explorer for running tests
- GitHub Copilot for AI-assisted development
- Prettier for code formatting

### GitHub Codespaces Compatibility

This dev container configuration is compatible with GitHub Codespaces, allowing you to develop in the cloud without any local setup.

## Learning Resources

- [Dev Containers documentation](https://code.visualstudio.com/docs/devcontainers/containers)
- [Blazor documentation](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
- [.NET 8 documentation](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-8)

## License

See the [LICENSE](./LICENSE) file for license rights and limitations.