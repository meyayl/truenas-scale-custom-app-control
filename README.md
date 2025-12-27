# truenas-scale-custom-app-control

Convenience wrapper to control custom apps on TrueNAS Scale

## Overview

The `cstmapp` script simplifies the management of custom Docker Compose applications on TrueNAS Scale by providing a command-line interface that wraps TrueNAS Scale's app management API. It automatically detects Docker Compose files in your project directory and handles the deployment, updates, and lifecycle management of your custom applications.

## Setup

The script can be used by the `admin` or `root` user. 

The installation should be done as `root` user.

The script must be located on a dataset, otherwise it cannot be executed succesfully.

```bash
mkdir /mnt/<app-dataset>/bin
cd /mnt/<app-dataset>/bin
wget https://raw.githubusercontent.com/meyayl/truenas-scale-custom-app-control/refs/heads/main/cstmapp
chmod +x cstmapp
```
Note: make sure to replace `<app-dataset>` with an existing data set.

Add the `/mnt/<app-dataset>/bin` folder to your PATH variable.

Run command as `admin` and/or `root`:
```bash
echo 'export PATH=$PATH:/mnt/<app-dataset>/bin' >> ~/.zshrc
```
Changes to the `.zshrc` only apply if the terminal is logged out and in again, or if the file is sourced with `source ~/.zshrc`.

## Commands

### App Lifecycle Management
- `create [app_name]` - Create a new app from a Docker Compose file
- `update [app_name]` - Update an existing app with changes from the Compose file
- `up [app_name]` - Create a new app or update existing (smart deploy)
- `delete [app_name]` - Delete an app completely
- `down [app_name]` - Alias for delete

### App Control
- `start [app_name]` - Start a stopped app
- `stop [app_name]` - Stop a running app
- `restart [app_name]` - Restart an app (stops then starts)
- `redeploy [app_name]` - Redeploy an app after re-pulling the image tag

### Information & Debugging
- `list` - List all deployed custom apps with their status
  - `--all/-a` - Include all apps (not just custom apps)
- `inspect [app_name]` - Show detailed app information in JSON format
  - `--compact/-c` - Compact JSON output
  - `--yaml` - YAML output format
- `config [app_name]` - Get app configuration
  - `--compact/-c` - Compact JSON output
  - `--yaml` - YAML output format
- `compose [args]` - Run docker compose commands directly on the app
  - `--app_name <name>` - Specify app name
  - `--file/-f <compose_file>` - Specify compose file

### Global Options
- `--file/-f <compose_file>` - Specify a Docker Compose file (auto-detects if not provided)
- `[app_name]` - App name (defaults to current directory name if not specified)

### Features
- Automatic Docker Compose file detection (compose.yml, compose.yaml, docker-compose.yml, docker-compose.yaml)
- App name validation according to TrueNAS Scale requirements (1-40 chars, lowercase, alphanumeric + hyphens)
- Real-time progress reporting for long-running operations
- Smart deployment with `up` command (creates or updates as needed)
