# phing-build

This is a Phing build.xml script for uploading a website to a remote server.

https://www.phing.info

The build.xml uses Phing's FileSyncTask to handle the file uploads so you'll need web hosting that supports SSH login.
SSH will prompt you for the password if you're not using SSH keys to connect.

You can see precisely what will be uploaded beforehand using the `sync:dryrun` option.

## Environments

The build.xml supports 3 remote environments as standard:

* prod (production)
* pre (pre-live, the customer can upload content without fear of it being destroyed or overwritten by developers)
* staging (used for initial customer preview and feedback)

## Setup

### Excluding files from upload

Copy the `sync.exclude.dist` file to `sync.exclude` and customise as required.

### Configuring upload environments

Copy the `env.properties.dist` file to either `prod.properties`, `pre.properties` or `staging.properties` and update the configuration.

```
// Note forward slashes after folders
sync.source.dir = /full/path/to/dir/
sync.destination.dir = /full/path/to/dir/
sync.destination.backup.dir = /full/path/to/backup/dir/
sync.exclude.file = sync.exclude
sync.remote.host = ssh-host
sync.remote.port = 22
sync.remote.user = ssh-user
```

| Config option | Description |
| ------------- | -----|
| sync.source.dir | Full path to where files are stored locally on your system |
| sync.destination.dir | Full path to where files are stored on the remote server |
| sync.destination.backup.dir | Full path to where files that were replaced during the transfer will be backed up on the remote server |
| sync.exclude.file | Path of sync.exclude file |
| sync.remote.host | Hostname of the remote server |
| sync.remote.port | Port of the remote server |
| sync.remote.user | SSH username |

## Usage

**phing help**

Displays the available options.

**phing sync:dryrun**

Only list files that will be uploaded - nothing will actually be uploaded to the remote server. An itemised list will be displayed.

**phing sync**

Upload files to a remote server immediately. An itemised list will be displayed.

**phing ssh**

Opens an SSH session to a remote server.

## Setting up passwordless SSH login

Create the RSA key pair using:

    ssh-keygen

By default, this will create the private and public key files in your ~/.ssh folder:

    ~/.ssh/id_rsa
    ~/.ssh/id_rsa.pub

Copy the public key to the remote server using `ssh-copy-id`. This is a one-time operation and allows you to execute sync and ssh tasks without password prompts. E.g.:

    ssh-copy-id remote-user@remote-host.com

You can execute `phing ssh` to test passwordless SSH login.

## Passing parameters directly to FileSyncTask from the command-line for batch scripting

`phing sync-execute-task -Ddryrun=true -propertyfile prod.properties`

The above example shows a dryrun to the prod environment using Phing's -D<property>=<value> and -propertyfile arguments.
