# Yocto BeagleBone AI-64 manifest repo

The .xml manifest file in this repo aggregates all the source repositories required for a fully functioning Yocto build of Embedded Linux.

## Creating Yocto Source directory with Repo Tool:

Each layer for Yocto builds resides in its own git repository. We install the repo tool on our development PCs to manage these repositories. The repo tool hides the complexity of git submodules. We install the tool on our development computer with the following commands in a directory included in $PATH (e.g., in $HOME/bin or /usr/local/bin)
```bash
$ cd ~/bin
$ wget https://storage.googleapis.com/git-repo-downloads/repo
$ chmod a+x ./repo
```

The repo tool needs to know from which URL to fetch the layer repositories and into which directory to put them. We define this information in a manifest file. For example, we initialize the umbrella project in the directory ```/yocto``` with the commands:

```bash
$ cd /yocto
$ repo init -u https://github.com/saizen408/bbai64-repo-manifest.git -b kirkstone -m bbai64-5.10.153.xml
...
repo has been initialized in /yocto
```

We clone the layer repositories into the directory ```/yocto/sources``` with the command

```bash
$ cd /yocto
$ repo sync
...
repo sync has finished successfully.
```

The ```/yocto/sources``` directory structure should look similar to the following:

```bash
#Install the handy tree command with 'apt-get install tree'
$ cd /yocto/sources
$ tree -L 1
.
├── meta-openembedded
├── meta-arago
├── meta-qt5
├── meta-virtualization
├── meta-ti
├── meta-arm
├── oe-core
├── meta-openembedded
└── poky
```
## Updating the Repo Manifest:
If any of the repos have new commits during the course of development, it is a good practice to update the manifest file with the following command.

```bash
$ cd path/to/yocto/directory
$ repo manifest -r -o bbai64-5.10.153.xml
```
Note that this command must be run in the directory that the ``.repo`` directory resides. In the case of the examples above, that directory would be ``/yocto``.
This method of updating the repo file allows for tying each repo to a specific commit as opposed to a branch. This ensures that future commits on non-internal repos (i.e. poky, meta-mender, etc.) don't break the yocto build.
