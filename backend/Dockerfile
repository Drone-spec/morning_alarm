ARG APP_NAME=Drones-Rust-Project

FROM rust:latest AS build

WORKDIR /app

RUN --mount=type=bind,source=src,target=src \
    --mount=type=bind,source=Cargo.toml,target=Cargo.toml,target=Cargo.toml \
    --mount=type=bind,source=Cargo.lock,target=Cargo.lock \
    --mount=type=cache,target=/app/target/ \
    --mount=type=cache,target=/usr/local/cargo/registry/ \
    --mount=type=bind,source=migrations,target=migrations \
    <<EOF

set -e
cargo build --locked --release
cp ./target/release/$APP_NAME /bin/server
EOF

FROM debian:bullseye-slim AS final

ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \ 
    appuser

USER appuser

COPY --from=build /bin/server /bin/

EXPOSE 8000

CMD ["/bin/server"]
