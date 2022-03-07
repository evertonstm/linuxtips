FROM golang AS buildando

WORKDIR /app
ADD . /app
RUN go build -o meugo

FROM alpine
WORKDIR /app
COPY --from=buildando /app/meugo /app

ENTRYPOINT ./meugo