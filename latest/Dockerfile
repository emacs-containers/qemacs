FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
      libncurses5-dev \
      libx11-dev \
      libxext-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://bellard.org/qemacs/qemacs-0.3.3.tar.gz qemacs.tar.gz
RUN sha256sum qemacs.tar.gz && sha256sum -c checksum
RUN mkdir qemacs && tar -C qemacs --strip-components 1 -xf qemacs.tar.gz
WORKDIR /build/qemacs
RUN ./configure --disable-xrender --disable-xv
RUN make
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libncurses6 \
      libx11-6 \
      libxext6 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
CMD ["qemacs"]
