# base image
FROM messence/rust-musl-cross:x86_64-musl as chef
ENV SQLX_OFFLINE=true
RUN cargo install cargo-chef
WORKDIR /rust-server-app

FROM chef AS planner
COPY . .
RUN cargo chef prepare --recipe-path recipe.json

FROM chef AS builder
COPY --from=planner /rust-server-app/recipe.json recipe.json
# build & cache dependencies
RUN cargo chef cook --release --target x86_64-unknown-linux-musl --recipe-path recipe.json
# copy the source code
COPY . .
# build the application
RUN cargo build --release --target x86_64-unknown-linux-musl

# create a new stage with a minimal image\
FROM scratch
# copy the binary build of the rust application from the previous stage
COPY --from=builder /rust-server-app/target/x86_64-unknown-linux-musl/release/rust-server-app /rust-server-app
ENTRYPOINT [ "/rust-server-app" ]
EXPOSE 3000