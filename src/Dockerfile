# syntax=docker/dockerfile:1
FROM --platform=$TARGETPLATFORM golang:1.21-alpine AS builder
ARG TARGETARCH
ARG TARGETOS
LABEL org.opencontainers.image.description="Simple echo app used for demos based on echo-http."
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH go build -a -o http-echo main.go

FROM gcr.io/distroless/static:nonroot-$TARGETARCH
WORKDIR /
COPY --from=builder /src/http-echo .
EXPOSE 8080
ENTRYPOINT ["/http-echo"]

