ARG BUILD_FROM=hassioaddons/base:5.0.1

FROM $BUILD_FROM AS build
RUN apk add git gcc musl-dev libffi-dev openssl-dev python3-dev=3.7.5-r1 cmake docker python3=3.7.5-r1 zlib openssl=1.1.1d-r0 wget vim zip sudo nano rsync
RUN git clone https://github.com/lbschenkel/broadlink-bridge.git -n /tmp/build \
    && cd /tmp/build \
    && git checkout "v0.1.2" \
    && pip wheel --wheel-dir=/tmp/wheels .

FROM $BUILD_FROM AS runtime
ENV LANG C.UTF-8
COPY --from=build /tmp/wheels /tmp/wheels
COPY run.sh /
RUN pip install --no-index --find-links=/tmp/wheels/ broadlink-bridge \
    && rm -Rfv /tmp/* \
    && chmod a+x /run.sh

WORKDIR /data
USER nobody

CMD [ "/run.sh" ]
