FROM golang:1.27.4-alpine3.24@sha256:70bd7ddea8b6cbe5d6943cde279d6b67feb10cc924fb9100d1d8757c1575b280 AS builder
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
