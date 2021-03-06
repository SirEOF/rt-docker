FROM golang:1.12-alpine as builder

RUN apk add --no-cache git

RUN git clone --branch "v1.1" --single-branch --depth 1 \
    https://github.com/korylprince/fileenv.git /go/src/github.com/korylprince/fileenv

RUN git clone --branch "v1.0" --single-branch --depth 1 \
    https://github.com/korylprince/run-every.git /go/src/github.com/korylprince/run-every

RUN go install github.com/korylprince/fileenv
RUN go install github.com/korylprince/run-every

FROM alpine:3.10

COPY --from=builder /go/bin/fileenv /
COPY --from=builder /go/bin/run-every /

RUN apk add --no-cache bash mysql-client \
    rt4=4.4.4-r0 && \
    # patch to use utf8mb4 encoding
    sed -i "s/SET NAMES 'utf8'/SET NAMES 'utf8mb4'/g" /usr/lib/rt4/RT/Handle.pm

COPY ./indexer/LocalConfig.pm /etc/rt4/RT_SiteConfig.d/
COPY ./indexer/run.sh /

CMD ["/fileenv", "bash", "/run.sh"]
