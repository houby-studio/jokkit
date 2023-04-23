### Build Stage

# Use the official Rust image as the base image
#FROM rust:1.69.0 as builder
FROM mcr.microsoft.com/devcontainers/rust:0-1-bullseye as develop

FROM rust:1.69.0 as build

# Set the working directory
WORKDIR /app

# Copy Cargo.toml and Cargo.lock to the working directory
COPY Cargo.toml Cargo.lock ./

# Build empty app with downloaded dependencies to produce a stable image layer for next build
# Create a dummy main.rs to build dependencies and cache them
RUN mkdir src && echo "fn main() {}" > src/main.rs
RUN cargo build --release
RUN rm -rf src

# Build web app with own code
# Copy the rest of the application code to the working directory
COPY . .

#RUN rm ./target/release/deps/axum_demo* TF?

# Build the application
RUN cargo build --release

### Run stage
FROM debian:buster-slim

# Install OpenSSL and other dependencies
#RUN apt-get update && apt-get install -y openssl libssl1.1 ca-certificates && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the binary from the build stage to the run stage
COPY --from=build /app/target/release/jokkit-backend /app/jokkit-backend

# Expose the port the app will run on
EXPOSE 3000

# Start the application
CMD ["./jokkit-backend"]
