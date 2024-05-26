# drone-s3-cache

[![Build Status](https://img.shields.io/github/actions/workflow/status/ecmchow/drone-s3-cache/build.yml?branch=master&style=flat-square&logo=GitHub)](https://github.com/ecmchow/drone-s3-cache/actions/)
[![Gitter chat](https://badges.gitter.im/drone/drone.png)](https://gitter.im/drone/drone)
[![Join the discussion at https://discourse.drone.io](https://img.shields.io/badge/discourse-forum-orange.svg)](https://discourse.drone.io)
[![Drone questions at https://stackoverflow.com](https://img.shields.io/badge/drone-stackoverflow-orange.svg)](https://stackoverflow.com/questions/tagged/drone.io)
[![](https://images.microbadger.com/badges/image/plugins/s3-cache.svg)](https://microbadger.com/images/plugins/s3-cache "Get your own image badge on microbadger.com")
[![Go Doc](https://godoc.org/github.com/ecmchow/drone-s3-cache?status.svg)](http://godoc.org/github.com/ecmchow/drone-s3-cache)
[![Go Report](https://goreportcard.com/badge/github.com/ecmchow/drone-s3-cache)](https://goreportcard.com/report/github.com/ecmchow/drone-s3-cache)

> ### This is a customized version of the [Official S3 Cache plugin](https://github.com/drone-plugins/drone-s3-cache) for Drone CI. See [Docker Hub](https://hub.docker.com/repository/docker/ecmchow/drone-s3-cache/general) for more details.

Drone plugin that allows you to cache directories within the build workspace, this plugin is backed by S3 compatible storages. For the usage information and a listing of the available options please take a look at [the docs](http://plugins.drone.io/drone-plugins/drone-s3-cache/).

## Build

Build the binary with the following command:

```console
export GOOS=linux
export GOARCH=amd64
export CGO_ENABLED=0
export GO111MODULE=on

go build -v -a -tags netgo -o release/linux/amd64/drone-s3-cache
```

## Docker

Build the Docker image with the following command:

```console
docker build \
  --label org.label-schema.build-date=$(date -u +"%Y-%m-%dT%H:%M:%SZ") \
  --label org.label-schema.vcs-ref=$(git rev-parse --short HEAD) \
  --file docker/Dockerfile.linux.amd64 --tag plugins/s3-cache .
```

## Usage

```console
docker run --rm \
  -e PLUGIN_FLUSH=true \
  -e PLUGIN_ENDPOINT="http://minio.company.com" \
  -e PLUGIN_ACCESS_KEY="myaccesskey" \
  -e PLUGIN_SECRET_KEY="mysecretKey" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  plugins/s3-cache

docker run --rm \
  -e PLUGIN_RESTORE=true \
  -e PLUGIN_ENDPOINT="http://minio.company.com" \
  -e PLUGIN_ACCESS_KEY="myaccesskey" \
  -e PLUGIN_SECRET_KEY="mysecretKey" \
  -e DRONE_REPO_OWNER="foo" \
  -e DRONE_REPO_NAME="bar" \
  -e DRONE_COMMIT_BRANCH="test" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  plugins/s3-cache

docker run -it --rm \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  alpine:latest sh -c "mkdir -p cache && echo 'testing cache' >> cache/test && cat cache/test"

docker run --rm \
  -e PLUGIN_REBUILD=true \
  -e PLUGIN_MOUNT=".bundler" \
  -e PLUGIN_ENDPOINT="http://minio.company.com" \
  -e PLUGIN_ACCESS_KEY="myaccesskey" \
  -e PLUGIN_SECRET_KEY="mysecretKey" \
  -e DRONE_REPO_OWNER="foo" \
  -e DRONE_REPO_NAME="bar" \
  -e DRONE_COMMIT_BRANCH="test" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  plugins/s3-cache
```
