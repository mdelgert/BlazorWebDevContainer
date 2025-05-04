# Creating a .NET 8 Blazor Web Container Project

This document provides a detailed walkthrough of the process used to create this dev container-based Blazor solution. It explains each step taken, the rationale behind technical decisions, and how to extend the solution for your own projects.

## Table of Contents

1. [Setting Up the Dev Container](#setting-up-the-dev-container)
2. [Creating the Blazor Project](#creating-the-blazor-project)
3. [Solution Structure Overview](#solution-structure-overview)
4. [Development Workflow](#development-workflow)
5. [Customization Options](#customization-options)
6. [Troubleshooting](#troubleshooting)

## Setting Up the Dev Container

### 1. Creating the Dev Container Configuration

The dev container configuration consists of two primary files:

#### `.devcontainer/devcontainer.json`

This file defines the container configuration, features, and Visual Studio Code settings:

```json
{
  "name": "Blazor .NET 8",
  "build": {
    "dockerfile": "Dockerfile",
    "context": ".."
  },
  "features": {
    "ghcr.io/devcontainers/features/node:1": {
      "version": "lts"
    },
    "ghcr.io/devcontainers/features/git:1": {}
  },
  "forwardPorts": [5000, 5001, 5002],
  "postCreateCommand": "dotnet restore",
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-dotnettools.csharp",
        "ms-dotnettools.csdevkit",
        "ms-dotnettools.blazorwasm-companion",
        "ms-dotnettools.vscode-dotnet-runtime",
        "formulahendry.dotnet-test-explorer",
        "github.copilot",
        "github.copilot-chat",
        "esbenp.prettier-vscode"
      ]
    }
  }
}
```

Key components:
- **Base image**: Using the official .NET 8 SDK image
- **Features**: Adding Git and Node.js using the standardized devcontainer features
- **Port forwarding**: Exposing ports 5000-5002 for the Blazor app
- **Post-create command**: Running `dotnet restore` to fetch dependencies
- **VS Code extensions**: Installing extensions to support .NET/Blazor development, including C# Dev Kit for enhanced C# language support

#### `.devcontainer/Dockerfile`

The Dockerfile extends the base .NET 8 SDK image with additional configurations:

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0

# Set up the working directory
WORKDIR /workspaces/BlazorWebDevContainer

# Install necessary dependencies
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends curl gnupg build-essential

# Setup for HTTPS development
RUN dotnet dev-certs https --trust

# Expose ports
EXPOSE 5000 5001 5002

# Ensure Git and Node.js are properly configured
# Note: Git and Node.js are already installed via the features in devcontainer.json
# This is just additional configuration if needed

# Install eslint globally
RUN npm install -g eslint
```

Key components:
- **Working directory**: Setting the project directory
- **Dependencies**: Installing system-level dependencies
- **HTTPS certificates**: Configuring for secure development
- **ESLint**: Installing for JavaScript linting

### 2. Dev Container Features

The dev container uses the standardized Dev Containers features system:

- **Git**: An up-to-date version of Git, built from source as needed, is pre-installed and available on the `PATH`
  - Git user configuration is set up automatically via environment variables or defaults
  - You can customize by setting `GIT_USER_NAME` and `GIT_USER_EMAIL` environment variables
- **Node.js**: `node`, `npm`, and `eslint` are pre-installed and available on the `PATH` for JavaScript development

## Creating the Blazor Project

### 1. Creating the Directory Structure

We first created a basic directory structure:

```bash
mkdir -p src
```

### 2. Generating the Blazor Project

We used the .NET CLI to generate a standard Blazor project:

```bash
dotnet new blazor -o src/BlazorApp
```

This command creates a new Blazor Web App project with the following structure:
- Components folder for Razor components
- Pages folder for routable components
- Layout components for the application shell
- wwwroot folder for static assets

### 3. Creating the Solution File

We created a solution file to organize our project:

```bash
dotnet new sln --name BlazorWebDevContainer
dotnet sln add src/BlazorApp/BlazorApp.csproj
```

This organizes the project for easier management, especially when adding more projects later.

## Solution Structure Overview

The final solution structure follows this organization:

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
├── BlazorWebDevContainer.sln    # Solution file
├── README.md                    # Project documentation
└── DOCUMENTATION.md             # This development process document
```

### Key Files and Their Purpose

#### `Program.cs`

This is the entry point for the Blazor application, configuring services and middleware:

```csharp
// Main application configuration
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

var app = builder.Build();
// Configure HTTP request pipeline
// ...
app.Run();
```

#### `Components/App.razor`

The root component that establishes the application's layout and routing:

```razor
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <base href="/" />
    <link href="bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="app.css" rel="stylesheet" />
    <HeadOutlet />
</head>
<body>
    <Routes />
    <script src="_framework/blazor.web.js"></script>
</body>
</html>
```

## Development Workflow

### Running the Application

Once inside the dev container, you can run the application with:

```bash
cd src/BlazorApp
dotnet watch run
```

This starts the application with hot reload enabled, allowing for changes to be immediately reflected in the browser.

### Debugging

1. Go to the "Run and Debug" panel in VS Code
2. Select ".NET Core Launch (web)" from the dropdown
3. Press F5 to start debugging

Breakpoints can be set in any .cs or .razor file.

### Adding New Components

To add a new page component:

1. Create a new .razor file in the Components/Pages directory
2. Add the @page directive with a route
3. Implement your component

Example:
```razor
@page "/counter"

<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

## Customization Options

### Adding NuGet Packages

To add additional NuGet packages:

```bash
cd src/BlazorApp
dotnet add package PackageName
```

### Adding JavaScript Libraries

There are multiple ways to add JavaScript libraries:

1. **Using npm**:
   ```bash
   cd src/BlazorApp/wwwroot
   npm init -y
   npm install --save libraryname
   ```

2. **Direct reference** in wwwroot:
   - Download the library files
   - Place them in wwwroot
   - Reference them in App.razor or specific components

### CSS Styling

The project uses Bootstrap by default. To customize:

1. Edit the app.css file in wwwroot
2. Add component-specific styles using .razor.css files
3. Add global styles by modifying the bootstrap configuration

## Troubleshooting

### Common Issues

#### Port Conflicts

If you encounter port conflicts:

1. Change the ports in Properties/launchSettings.json
2. Update the forwarded ports in .devcontainer/devcontainer.json

#### HTTPS Certificate Issues

If HTTPS certificates aren't working:

```bash
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

#### Container Build Failures

If the container fails to build:

1. Check Docker logs for errors
2. Ensure Docker has enough resources allocated
3. Try rebuilding with `--no-cache` option

### Getting Help

- [.NET GitHub Issues](https://github.com/dotnet/core/issues)
- [Dev Containers GitHub Issues](https://github.com/microsoft/vscode-dev-containers/issues)
- [Blazor Documentation](https://docs.microsoft.com/aspnet/core/blazor/)