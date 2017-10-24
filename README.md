# Docker Image for Seafile 5 & 6 Pro Edition

[![Docker Automated buil](https://img.shields.io/docker/automated/xama/docker-seafile-pro.svg)]()
[![Docker Stars](https://img.shields.io/docker/stars/xama/docker-seafile-pro.svg)]()
[![Docker Pulls](https://img.shields.io/docker/pulls/xama/docker-seafile-pro.svg)]()
[![Docker Layers](https://images.microbadger.com/badges/image/xama/docker-seafile-pro.svg)]()

## Introduction

You can use this image to set up and run seafile pro edition as a docker container. The pro edition has more features than the free one, but is limited to 3 users if you don't have a license.

## Setup

Due to the nature of seafile, the setup is interactive only. During the setup procedure you're going to be asked about a few things, default is most of the time OK. 

You can tweak the configuration later by editing the config files generated during setup. I already included libreoffice integration (document preview), resumable file uploads and a sqlite3 database.

Start the setup procedure by running the following command.
Change `/var/seafile` if you want to have the installation somewhere else.

```
docker run -it --rm \
	--name=seafile-setup \
	-v /var/seafile:/seafile \
	xama/docker-seafile-pro setup
```

PLEASE NOTE THE "setup" FOLLOWING THE IMAGE NAME!

## Run

This will start seafile in the background.

```
docker run -d \
	--name=seafile \
	-p 8082:8082 \
	-p 8000:8000 \
	-v /var/seafile:/seafile \
	xama/docker-seafile-pro
```

You should be able to access `http://localhost:8000` in your browser now.

## Upgrade (EXPERIMENTAL)

CAUTION: THIS FEATURE IS IN ITS CURRENT STATE EXPERIMENTAL. USE AT YOUR OWN RISK!

This image supports upgrading seafile too!

First, remove the seafile image. (You'll have to stop your container first)

```
docker stop seafile
docker rm seafile
docker rmi xama/docker-seafile-pro
```

Proceed to run the container the same way you ran the setup. (except you'll need to use "upgrade" instead of "setup")

```
docker run -it --rm \
	--name=seafile-upgrade \
	-v /var/seafile:/seafile \
	xama/docker-seafile-pro upgrade
```

This procedure requires a few minutes to complete. Please be patient and grab a coffee.

If for some reason seafile fails to start or you weren't patient, you can restore a backup of the state before seafile has been upgraded.
Backups are located at (in our example) `/var/seafile/backup` in .tar.gz format.
Start the container as usual after restoring the backup (see "Run").

## Use a custom version

I recommend using rather the image tags provided by docker hub since changes to the container environment may always occur, which could break previous versions. 
When using container tags you ensure the docker environment really works with your version of preference.
For instance, you'll want to use xama/docker-seafile-pro:5.1.11 instead of xama/docker-seafile-pro:latest if you want to stay on 5.1.11

Refer to [Releases](https://github.com/xama5/docker-seafile-pro/releases) for available versions.

## Install license file

You may install the pro edition for up to 3 users for free.
If you have a license, you can add the file to the root directory of seafile.
`/var/seafile` in my case.

## Important notice

Since Seafile is included in the image as of 6.0.6, all prior versions cannot be installed anymore because seafile removed the binaries from their servers.
If you already have Seafile with a prior version installed you're fine. This only affects new users who wish to install an older version.

## Contribute

This solution is quick'n'dirty, but works.
If you have any suggestions in order to improve this image, please create a pull request/issue. Thank you!
