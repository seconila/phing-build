# phing-build

This is a Phing build.xml script used to upload websites to remote servers.

https://www.phing.info

The build.xml uses Phing's FileSyncTask to handle the file uploads so you'll need web hosting that supports SSH login.
SSH will prompt you for the password if you're not using SSH keys to connect.

You can see precisely what will be uploaded beforehand using the “sync:dryrun” option.

## Environments

The build.xml supports 3 environments as standard:

* prod (production)
* pre (pre-live, clients can upload content without the worry of it being destroyed by developers)
* staging (used for initial client preview and feedback)

## Setup

### Excluding files from upload

Copy the `sync.exclude.dist` file to `sync.exclude` and customise as required.

### Configuring upload environments

Copy the `env.properties.dist` file to either `prod.properties`, `pre.properties` or `staging.properties` and update the configuration.

```
// Note forward slashes after folders
sync.source.dir = /full/path/to/dir/
sync.destination.dir = /full/path/to/dir/
sync.destination.backup.dir = /full/path/to/dir/backup/
sync.exclude.file = sync.exclude
sync.remote.host = ssh-host
sync.remote.port = 22
sync.remote.user = ssh-user
```

| Config option | Description |
| ------------- | -----|
| sync.source.dir | Full path where files are stored locally on your system |
| sync.destination.dir | Full path where files are stored on remote server |
| sync.destination.backup.dir | Full path where files that were replaced during the transfer will be backed up on remote server |
| sync.exclude.file | Path of sync.exclude file |
| sync.remote.host | Hostname of remote server |
| sync.remote.port | Port of remote server |
| sync.remote.user | SSH username |


## Usage

**phing help**

Displays the available options.

**phing sync:dryrun**

Only list files that will be uploaded to an environment - nothing will actually get uploaded to the remote server. An itemised list will be displayed.

**phing sync**

Upload files to an environment. An itemised list will be displayed.

**phing ssh**

Opens an SSH session to an environment.
