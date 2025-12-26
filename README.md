# truenas-scale-custom-app-control

Convenience wrapper to control custom apps on TrueNAS Scale

## Overview

The `cstmapp` script simplifies the management of custom Docker Compose applications on TrueNAS Scale by providing a command-line interface that wraps TrueNAS Scale's app management API. It automatically detects Docker Compose files in your project directory and handles the deployment, updates, and lifecycle management of your custom applications.

## Setup

The script can be used by the `admin` or `root` user. 
The installation should be done as `root` user.

THe script must be located on a dataset, otherwise it can not be executed.

```bash
mkdir /mnt/<app-dataset>/bin
cd /mnt/<app-dataset>/bin
wget https://raw.githubusercontent.com/meyayl/truenas-scale-custom-app-control/refs/heads/main/cstmapp
chmod +x cstmapp
```
Note: make sure to replace `<app-dataset>` with an existing data set.

Add the `/mnt/<app-dataset>/bin` folder to your PATH variable.

As `admin`: `echo "export PATH+=:/mnt/<app-dataset>/bin" >> ~/.zshrc`
As `root`: `echo "export PATH+=:/mnt/<app-dataset>/bin" >> ~/.bashrc`


## Commands

### App Lifecycle Management
- `create <app_name>` - Create a new app from a Docker Compose file
- `update <app_name>` - Update an existing app with changes from the Compose file
- `up <app_name>` - Create a new app or update existing (smart deploy)
- `delete <app_name>` - Delete an app completely
- `down <app_name>` - Alias for delete

### App Control
- `start <app_name>` - Start a stopped app
- `stop <app_name>` - Stop a running app
- `restart <app_name>` - Restart an app
- `redeploy <app_name>` - Redeploy an app after re-pulling the image tag

### Information & Debugging
- `list` - List all deployed custom apps with their status
- `inspect <app_name>` - Expose detailed information about an app in JSON (use `--compact` for compact output)
- `compose --app_name <app_name> [args]` - Run docker compose commands directly on the app

### Options
- `--file/-f <compose_file>` - Specify a Docker Compose file (auto-detects if not provided)
- `<app_name>` - App name (defaults to current directory name if not specified)

The script automatically finds Docker Compose files (compose.yml, docker-compose.yml, etc.) and validates app names according to TrueNAS Scale requirements.
