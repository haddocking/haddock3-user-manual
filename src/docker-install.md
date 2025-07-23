# Using HADDOCK3 through its docker image

As part of the possible usage of HADDOCK3, we also provide a ready to use `docker` image of HADDOCK3, where tools and packages are already installed.

## DOCKER

To be able to use a provided image, you first need to have `docker` installed.
Please follow the instructions you can find there: [https://www.docker.com/](https://www.docker.com/).


## Installing HADDOCK3 from provided image

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
