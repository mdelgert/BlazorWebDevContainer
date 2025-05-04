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