FROM golang:1.27.0-alpine3.24@sha256:4c9fe60190a2a3350ddc51de80d0224b8a6698d12bdfc999fee45ea9d6c46dbc AS builder
ENV GOTOOLCHAIN=local
WORKDIR /src
COPY otel/langfuse-fanout/go.mod otel/langfuse-fanout/go.sum* ./
RUN go mod download
COPY otel/langfuse-fanout/ ./
RUN CGO_ENABLED=0 GOOS=linux go build -trimpath -ldflags="-s -w" -o /out/langfuse-fanout ./cmd/langfuse-fanout

FROM alpine:3.22
RUN addgroup -S app && adduser -S app -G app
COPY --from=builder /out/langfuse-fanout /usr/local/bin/langfuse-fanout
USER app
EXPOSE 4318
ENTRYPOINT ["/usr/local/bin/langfuse-fanout"]
