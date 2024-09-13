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
