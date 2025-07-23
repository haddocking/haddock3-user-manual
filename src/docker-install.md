# Using HADDOCK3 through its docker image

As part of the possible usage of HADDOCK3, we also provide a ready to use `docker` image of HADDOCK3, where tools and packages are already installed. Note that this image corresponds to the latest release of HADDOCK3, and not the latest version. To use the latest version in docker rather build it directly (see below).

## DOCKER

To be able to use a provided image, you first need to have `docker` installed.
Please follow the instructions you can find there: [https://www.docker.com/](https://www.docker.com/).


## Installing the latest release of HADDOCK3 from provided image

```bash
# Install the latest haddock3 version
docker pull ghcr.io/haddocking/haddock3:latest
docker tag ghcr.io/haddocking/haddock3:latest haddock3

## OR

# Install a haddock3 specific version (e.g: 2024.12.0b7)
docker run ghcr.io/haddocking/haddock3:2024.12.0b7
docker tag ghcr.io/haddocking/haddock3:2024.12.0b7 haddock3

# Run HADDOCK with a workflow file, e.g. myworkflow.cfg
docker run \
    -v $(pwd):/cwd  \
    --workdir /cwd \
    -u $(id -u)  \
    haddock3 \
    myworkflow.cfg

```


## Building your own docker container from the latest version of HADDOCK3

```bash
# Build a container from the latest haddock3 version
git clone https://github.com/haddocking/haddock3.git
cd haddock3
docker build . --label haddock3 --tag haddock3
```
