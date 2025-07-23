# Using HADDOCK3 through its docker image

As part of the possible usage of HADDOCK3, we also provide a ready to use `docker` image of HADDOCK3, where tools and packages are already installed.
Note that this image corresponds to the latest release of HADDOCK3, and not the latest version. To use the latest version in docker rather build it directly ([see below](#building-your-own-docker-container-from-the-latest-version-of-haddock3)).

## DOCKER

To be able to use a provided image, you first need to have `docker` installed.
Please follow the instructions you can find there: [https://www.docker.com/](https://www.docker.com/).


## Installing HADDOCK3 from provided images

### Installing the latest release

```bash
docker pull ghcr.io/haddocking/haddock3:latest
docker tag ghcr.io/haddocking/haddock3:latest haddock3
```

### Install a specific version

To install a specific version of haddock3, you must specify which release you are interested in (e.g.: `2025.05.0`)

```bash
docker run ghcr.io/haddocking/haddock3:2025.05.0
docker tag ghcr.io/haddocking/haddock3:2025.05.0 haddock3
```

Check [here](https://github.com/haddocking/haddock3/pkgs/container/haddock3/versions?filters%5Bversion_type%5D=tagged) to see the list of available releases.

## Building your own docker container from the latest version of HADDOCK3

With this approach, you will be building a new docker image from the latest version available in the main branch of the [haddock3 GitHub repository](https://github.com/haddocking/haddock3).

```bash
# Build a container from the latest haddock3 version
git clone https://github.com/haddocking/haddock3.git
cd haddock3
docker build . --label haddock3 --tag haddock3
```

## Run HADDOCK with a workflow file, e.g. myworkflow.cfg

```bash
docker run \
    -v $(pwd):/cwd  \
    --workdir /cwd \
    -u $(id -u)  \
    haddock3 \
    myworkflow.cfg
```


