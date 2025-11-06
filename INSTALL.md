# Installing FragPipe scripts on Alliance Canada

### Steps

1. [Updating scripts](#Updating-scripts)
2. [Installing of the scripts](#Installing-of-the-scripts)
   1. [Change directory to `projects` folder](#Change-directory-to-projects-folder)
   2. [Clone repository](#Clone-repository)
3. [Creating container for FragPipe](#Creating-container-for-FragPipe)

## Updating scripts

Go to the fragpipe scripts folder and run `git pull`.

```shell
cd ~/projects/def-robertf/scripts/fragpipe
git pull
```

For Rorqual server, use

```shell
cd ~/links/projects/def-robertf/scripts/fragpipe
git pull
```

## Installing of the scripts

### Change directory to projects folder

```shell
cd ~/projects/def-robertf/scripts
```

For Rorqual server, use

```shell
cd ~/links/projects/def-robertf/scripts
```

### Clone repository

```shell
git clone https://github.com/francoisrobertlab/fragpipe.git
```

## Creating container for FragPipe

### Download FragPipe and tools

> [!TIP]
> Make sure Java is installed, use `java -version` to check.

Go to [FragPipe download Website](https://github.com/Nesvilab/FragPipe/releases) and download the FragPipe ZIP file for Linux. Unzip FragPipe and start the GUI.

```shell
version=23.1
```

```shell
wget https://github.com/Nesvilab/FragPipe/releases/download/$version/FragPipe-$version-linux.zip
unzip FragPipe-$version-linux.zip
./fragpipe-$version/bin/fragpipe
```

In FragPipe's GUI, download additional tools. In the *"Config"* tab, use the *"Download / Update"* button in the *"MSFragger, IonQuant, diaTracer"* section.

Once the tools are downloaded, you are ready to create the container.

### Create container

To create an [Apptainer](https://apptainer.org) container for FragPipe, you must use a Linux computer. Ideally, you should have root access on the computer. 

```shell
version=23.1
```

Save location of tools. This is the default location, if you downloaded FragPipe only once.

```shell
tools=fragpipe-$version/tools
```

```shell
sudo apptainer build --build-arg version=$version --build-arg tools=$tools fragpipe-$version.sif fragpipe.def
```

On Alliance Canada server, you need to use `fakeroot`. Note that containers created using `fakeroot` may fail.

```shell
module load apptainer
apptainer build --fakeroot --build-arg version=$version --build-arg tools=$tools fragpipe-$version.sif fragpipe.def
```

### Copy container on Globus

```shell
scp fragpipe-$version.sif 'narval.computecanada.ca:~/projects/def-robertf/Sharing/globus-shared-apps/fragpipe'
```
