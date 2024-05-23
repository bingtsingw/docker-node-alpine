FROM alpine:3.20

RUN addgroup -g 1000 node \
    && adduser -u 1000 -G node -s /bin/sh -D node \
    && apk add --no-cache libstdc++

ENV NODE_VERSION 16.13.0
ENV NODE_TARFILE node-v$NODE_VERSION-linux-x64-musl.tar.xz

RUN wget "https://unofficial-builds.nodejs.org/download/release/v$NODE_VERSION/$NODE_TARFILE" \
    && wget "https://unofficial-builds.nodejs.org/download/release/v$NODE_VERSION/SHASUMS256.txt" \
    && grep $NODE_TARFILE SHASUMS256.txt | sha256sum -c - \
    && tar -xJf "$NODE_TARFILE" -C /usr/local --strip-components=1 --no-same-owner \
    && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    && npm uninstall npm -g \
    && rm -f "$NODE_TARFILE" \
    && rm -f SHASUMS256.txt \
    && node --version

ENV PNPM_VERSION 6.16
ENV PNPM_TARFILE v$PNPM_VERSION.js

RUN wget "https://get.pnpm.io/$PNPM_TARFILE" \
    && wget "https://get.pnpm.io/SHASUMS256.txt" \
    && grep $PNPM_TARFILE SHASUMS256.txt | sha256sum -c - \
    && cat $PNPM_TARFILE | node - add --global pnpm \
    && rm -f "$PNPM_TARFILE" \
    && rm -f SHASUMS256.txt \
    && pnpm --version

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

CMD [ "node" ]
