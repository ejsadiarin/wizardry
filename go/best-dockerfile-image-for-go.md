---
date: 2024-09-15T20:18
tags:
  - Go
  - How-To
---

<!-- 2024-09-15-2018 (September 15, 2024 08:18:39 PM) -->

# Best Dockerfile Image for Go

1. copy go.mod and go.sum before the source code, then go mod download so the deps are in their own layer
2. instead of copying the sources use a `—mount=type=bind` mount
3. use a cache mount for `GOCACHE`
4. make a multi platform image for arm64 support (Don't specify GOOS/GOARCH in the Dockerfile, so that it can be re-used for multiple architectures)
   - use distroless as final base image
5. specify the base image as `docker.io/golang`, not just golang (better compatibility with podman, pedantically more correct)
6. if you don’t need debug symbols, `go build -ldflags=“-s -w”`
7. pretty sure you can still do USER `1001:1001` in the scratch image to not run as root
8. Add a `.dockerignore` with `.git`, `.env`, etc. (less important with the bind mount)
9. You're going to want zoneinfo and TLS CAs in your scratch container:
   ```dockerfile
   COPY --from=build /usr/share/zoneinfo /usr/share/zoneinfo
   COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
   ```
10. put the steps in the final stage before the copy `—from` so they don’t get invalidated

## General Idea

> I maintain that with Go it is always faster and most efficient from a CI perspective to compile outside and then copy into a scratch container.
> No dancing around the docker cache, just rely on the local system cache.
> Don't complicate something that statically compiles with the intricacies of layering.
> Multiplatform builds will go faster and you can leverage built in docker args to create multi platform manifests.

The only exception is CGO when you are working with shared libraries.

## Decision Explanations

1. uid `1000` is avoided because it is the default for the first non root user on Unix systems :)
   - You don’t want the docker user to accidentally inherit privileges of the non-root user on the host.
     - Especially since user 1000 has passwordless sudo on a lot of vm images.
   - the concern here is if there is a container escape or if you are using bind mounts without remapping uids.

# Distroless Image

- why distroless? read this: [Kubernetes rebase images to distroless](https://github.com/kubernetes/enhancements/blob/master/keps/sig-release/1729-rebase-images-to-distroless/README.md)
- copied from [official distroless github repository](https://github.com/GoogleContainerTools/distroless?tab=readme-ov-file)

Docker multi-stage builds make using distroless images easy.

Follow these steps to get started:

1. Pick the right base image for your application stack.
2. Write a multi-stage docker file. Note: This requires Docker 17.05 or higher.
3. The basic idea is that you'll have one stage to build your application artifacts, and insert them into your runtime distroless image.
4. If you'd like to learn more, please see the documentation on multi-stage builds.

```Dockerfile
# Start by building the application.
FROM golang:1.18 as build

WORKDIR /go/src/app
COPY . .

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

# Now copy it into our base image.
FROM gcr.io/distroless/static-debian12
COPY --from=build /go/bin/app /
CMD ["/app"]
```

You can find other examples here:

- [Java](https://github.com/GoogleContainerTools/distroless/blob/main/examples/java/Dockerfile)
- [Python 3](https://github.com/GoogleContainerTools/distroless/blob/main/examples/python3/Dockerfile)
- [Go](https://github.com/GoogleContainerTools/distroless/blob/main/examples/go/Dockerfile)
- [Node.js](https://github.com/GoogleContainerTools/distroless/blob/main/examples/nodejs/Dockerfile)
- [Rust](https://github.com/GoogleContainerTools/distroless/blob/main/examples/rust/Dockerfile)

To run any example, go to the directory for the language and run

```bash
docker build -t myapp .
docker run -t myapp
```

To run the Node.js Express app node-express and expose the container's ports:

```bash
npm install # Install express and its transitive dependencies
docker build -t myexpressapp . # Normal build command
docker run -p 3000:3000 -t myexpressapp
```

This should expose the Express application to your localhost:3000
