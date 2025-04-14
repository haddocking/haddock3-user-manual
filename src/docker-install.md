# Installing HADDOCK3 using docker

As part of the possible installation procedure, we also provide a `docker` version of HADDOCK3, where tools and packages are already installed.

## DOCKER

To be able to build or use a provided image, you first need to have `docker` installed.
Please follow the instructions you can find there: [https://www.docker.com/](https://www.docker.com/).


## Installing HADDOCK3 from provided image

```bash
# Run the latest haddock3 version
docker run ghcr.io/haddocking/haddock3:latest

## OR

# Run a haddock3 specific version (e.g: 2024.12.0b7)
docker run ghcr.io/haddocking/haddock3:2024.12.0b7
```
