FROM golang:1.27.3-alpine3.24@sha256:fa3d596898d9f126af7c01d14ee7597bab3f39cccf392ef36b69fe77cd681f18 AS builder
ENV GOTOOLCHAIN=local
WORKDIR /src
COPY otel/langfuse-fanout/go.mod otel/langfuse-fanout/go.sum* ./
RUN go mod download
COPY otel/langfuse-fanout/ ./
RUN CGO_ENABLED=0 GOOS=linux go build -trimpath -ldflags="-s -w" -o /out/langfuse-fanout ./cmd/langfuse-fanout

FROM alpine:3.24
RUN addgroup -S app && adduser -S app -G app
COPY --from=builder /out/langfuse-fanout /usr/local/bin/langfuse-fanout
USER app
EXPOSE 4318
ENTRYPOINT ["/usr/local/bin/langfuse-fanout"]
