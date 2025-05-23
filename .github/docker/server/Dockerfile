# Base
FROM golang:1.20.5-alpine AS builder
RUN apk add --no-cache git build-base gcc musl-dev
WORKDIR /app
COPY . /app
RUN go mod download
RUN go build ./cmd/interactsh-server


# Release
FROM alpine:3.18.2
RUN apk -U upgrade --no-cache \
    && apk add --no-cache bind-tools ca-certificates python3 libffi curl \
    && apk add --no-cache --virtual .build-deps python3-dev py3-pip py3-wheel libffi-dev build-base \
    && python3 -m pip install impacket \
    && python3 -m pip cache purge \
    && apk del .build-deps
RUN curl -o /usr/local/bin/smb_server.py https://raw.githubusercontent.com/projectdiscovery/interactsh/main/cmd/interactsh-server/smb_server.py
WORKDIR "/usr/local/bin"
COPY --from=builder /app/interactsh-server /usr/local/bin/

ENTRYPOINT ["interactsh-server"]