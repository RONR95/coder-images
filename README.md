# Coder Images

This repo hold the `Dockerfile`s of our [Coder](https://coder.com/) images that will be used in an airtight environment.

## Images

### Golang

- [1.19.13](/golang/1.19.13/Dockerfile)
- [1.20.13](/golang/1.20.13/Dockerfile)
- [1.21.6](/golang/1.21.6/Dockerfile)

### Node

- [18.19.0](/node/18.19.0/Dockerfile)
- [20.11.0](/node/20.11.0/Dockerfile)
- [21.6.1](/node/21.6.1/Dockerfile)

### Python

- [3.8.18](/python/3.8.18/Dockerfile)
- [3.9.18](/python/3.9.18/Dockerfile)
- [3.11.7](/python/3.11.7/Dockerfile)
- [3.12.1](/python/3.12.1/Dockerfile)

## Archiving Issues

An issue ocurred when the first image arrived to the airtight environment.
The [golang](https://go.dev/) executables were broken, printing a memory map and showing
clear signs that the CLI is dead.

### Possible Causes

- VM we archived the `.tar` file is not stable
- The VM has an old [win-rar](https://www.win-rar.com/) version installed while the PC
in the airtight environment has the latest version of [7zip](https://www.7-zip.org/)

### Solution

The following steps were chosen to provide a working solution:
- Building the images on a reliable PC
- Archiving using `7zip` as its proved more reliable than `win-rar`
- Layers to be uploaded to [github](https://github.com/firefly-out/coder-images)

## Image Life Cycle

Each `Dockerfile` was:
1. Built using `docker build -t <image-name> -f <path-to-Dockerfile> .` with connection to the `www`
2. Saved locally using `docker save -o <image-name>.tar <image-name>:latest`
3. Split into `100MB` chunks using `7zip`
4. Extracted using `7zip`
5. Loaded into a sterile `Docker Desktop` instance using `docker load -i <image-name>.tar`
6. Executed using `docker run <image-name>:latest` for checking in the binary executables of the framework (`golang` as an example) is not corrupted
