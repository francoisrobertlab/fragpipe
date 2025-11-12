# FragPipe on Alliance Canada

This repository contains scripts to run FragPipe on Alliance Canada servers.

To install the scripts on Alliance Canada servers and create containers, see [INSTALL.md](INSTALL.md)

### Steps

1. [Create workflow and manifest files](#Create-workflow-and-manifest-files)
2. [Transfer data to `scratch`](#Transfer-data-to-scratch)
3. [Add FragPipe scripts folder to your PATH](#Add-FragPipe-scripts-folder-to-your-PATH)
4. [Download FragPipe container](#Download-FragPipe-container)
5. [See FragPipe help (optional)](#See-FragPipe-help)
6. [Checking the different steps FragPipe will use](#Checking-the-different-steps-FragPipe-will-use)
7. [Running FragPipe](#Running-FragPipe)

## Create workflow and manifest files

Start FragPipe on a Windows or Linux computer and configure the pipeline.

Save the workflow using *"Save to custom folder"* and the manifest using *"Save as manifest"*, both being in the *"Workflow"* tab.  

> [!TIP]
> You will need to change the location of `database.db-path` in the workflow file and the location of the RAW files in the manifest file.

Edit workflow file using you preferred text editor and replace the folder of the `database.db-path` element to `/data`.

Edit manifest file using you preferred text editor and replace the folder of the RAW files in the first column to `/data`.

## Transfer data to scratch

You will need to transfer the following files on the server in the `scratch` folder.

* MS/MS RAW files.
* FASTA file(s).
* Workflow file.
* Manifest file (containing sample names and location of RAW files).
* Any additional files that are needed by FragPipe, when applicable.

There are many ways to transfer data to the server. Here are some suggestions.

* Use an FTP software like [WinSCP](https://winscp.net) (Windows), [Cyberduck](https://cyberduck.io) (Mac), [FileZilla](https://filezilla-project.org).
* Use command line tools like `rsync` or `scp`.

## Add FragPipe scripts folder to your PATH

```shell
export PATH=~/projects/def-robertf/scripts/fragpipe:$PATH
```

For Rorqual server, use

```shell
export PATH=~/links/projects/def-robertf/scripts/fragpipe:$PATH
```

## Download FragPipe container

```shell
wget https://g-88ccb6.6d81c.5898.data.globus.org/fragpipe/fragpipe-23.1.sif
```

## See FragPipe help

> [!TIP]
> You can see FragPipe's help using this command

```shell
fragpipe.sh --help
```

## Checking the different steps FragPipe will use

> [!IMPORTANT]
> Replace `$workflow` and `$manifest` with the actual parameters to use.

```shell
fragpipe.sh --headless --workflow $workflow --manifest $manifest --workdir output --dry-run
```

## Running FragPipe

You should choose the right amount of CPUs and memory (RAM) to use.

A reasonable amount of CPUs is 24 for 3 RAW files or less and 48 for more than 3 RAW files. For memory, you can try with 64GB and adjust if the task fails due to an *out of memory* exception.

```shell
threads=48
```

> [!IMPORTANT]
> Replace `$workflow` and `$manifest` with the actual parameters to use.

```shell
sbatch --cpus-per-task=$threads --mem=64G fragpipe.sh --headless --workflow $workflow --manifest $manifest --workdir output --threads $threads
```
