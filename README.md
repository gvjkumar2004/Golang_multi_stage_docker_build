# Multi Stage Docker Build for golang application

This repository contains a simple calculator application written in Go. It showcases the advantages of using Go with Docker, particularly through multi-stage builds and distroless images.

## Why Go?

- **Statically-Typed Language**: Go compiles directly to machine code and does not require a runtime. This results in smaller and faster executables compared to dynamically-typed languages.
- **Efficient Execution**: No need for additional runtime dependencies, leading to streamlined containerization.

## Dockerizing with Multi-Stage Builds and Distroless Images

### Multi-Stage Builds

- **Reduced Image Size**: Separates the build environment from the runtime environment, ensuring only the final binary is included in the final image.
- **Cleaner Builds**: Minimizes the size of the Docker image by excluding build tools and dependencies.

### Distroless Images

- **Minimalist Approach**: Contains only the essential runtime libraries needed to run the application, excluding unnecessary components like package managers or shells.
- **Enhanced Security**: Reduces attack surfaces by removing unnecessary utilities from the container.

### Generic Dockerfile without multi stage build

```dockerfile
# Use an official Go image to build the application
FROM golang:1.20 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the Go application code into the container
COPY . .

# Build the Go application
RUN go build -o calculator calculator.go

# Command to run the app
CMD ["./calculator"]
```

### Multi stage Dockerfile

```dockerfile
###########################################
# BASE IMAGE
###########################################

FROM ubuntu AS build

RUN apt-get update && apt-get install -y golang-go

ENV GO111MODULE=off

COPY . .

RUN CGO_ENABLED=0 go build -o /app .

############################################
# HERE STARTS THE MAGIC OF MULTI STAGE BUILD
############################################

FROM scratch

# Copy the compiled binary from the build stage
COPY --from=build /app /app

# Set the entrypoint for the container to run the binary
ENTRYPOINT ["/app"]
```
## Image Size Comparison
### Below is a comparison of the Docker image sizes for the generic Dockerfile and the multi-stage Dockerfile:

![image](https://github.com/user-attachments/assets/5f394b5b-e7dc-4390-af17-3603226e2a61)

## Conclusion

### By leveraging Go's statically-typed nature and Docker's multi-stage builds, we achieve significant benefits:

- **Reduced Image Size**: Multi-stage builds and distroless images help in creating smaller, more efficient Docker images.
- **Enhanced Security**: Distroless images minimize attack surfaces by excluding unnecessary utilities.



