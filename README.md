# Coder Images

This repo hold the `Dockerfile`s of our `Coder` images that will be used in an airtight environment.

## Archiving Issues

An issue occured when the first image arrived to the airtight environment.
The [golang](https://go.dev/) executables were broken, printing a memory map and showing
clear signs that the CLI is dead.

### Possible Causes

- In my opinion it happened as the VM we archived the `.tar` files is not
stable and was making archiving problems
- The VM has [win-rar](https://www.win-rar.com/) installed while the PC
in the airtight environment has [7zip](https://www.7-zip.org/) installed

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
