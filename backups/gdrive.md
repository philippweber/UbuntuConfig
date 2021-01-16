# Usin gdrive on Ubuntu

```bash
# How to use gdrive with bash

# Drive may be installed using snap:
#snap info drive

# Compile & install manually using go

# Find official golang image

podman search docker.io/golang
podman pull docker.io/library/golang
podman images

# Create gdrive directory
mkdir gdrive
cd gdrive/

# Create src & bin ("bin" would be sufficient, only "bin" contains the compiled program)
mkdir src
mkdir bin

# Compile (use "go/bin" if you do not need source files)
podman run --rm -v "$PWD":/go -w /go golang:latest go get -v -u github.com/odeke-em/drive/cmd/drive

# Init drive, follow instructions
./drive init

# This will store credentials in the local ".gd" subdirectory
jq . .gd/credentials.json

# Try it
./drive list

# Copy install the program
sudo cp drive /usr/local/bin/

# You can now use drive in any directory with the ".gd" directory init
drive ls
drive new -folder git-repo
drive ls git-repo
drive ls -recursive git-repo
drive ls -no-prompt -recursive git-repo
drive du -no-prompt -recursive git-repo
```
