#####
# For development
#####
FROM rust:1.48-buster as development
RUN apt-get update && apt-get -y install \ 
  libssl-dev \
  pkg-config \
  procps \
  curl \
  postgresql-client-11 \
  inotify-tools \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*
WORKDIR /app
RUN cargo install cargo-watch cargo-tarpaulin && \
  rustup component add rustfmt

#####
# For build
#####
FROM rust:1.48-buster AS builder
WORKDIR /apps
RUN USER=root cargo new rust-bench
COPY Cargo.toml /apps/rust-bench/
WORKDIR /apps/rust-bench
RUN cargo build --release
COPY src/ /apps/rust-bench/src
RUN touch src/main.rs && cargo build --release

#####
# For production
#####
FROM debian:buster-slim
RUN apt-get update && apt-get -y install \ 
  libssl-dev \
  pkg-config \
  ca-certificates \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

RUN mkdir /app
COPY --from=builder /apps/rust-bench/target/release/rust-bench /app/rust-bench
EXPOSE 8080
CMD ["/app/rust-bench"]
