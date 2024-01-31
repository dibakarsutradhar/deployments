# base image
FROM messence/rust-musl-cross:x86_64-musl as builder
ENV SQLX_OFFLINE=true
WORKDIR /rust-server-app
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