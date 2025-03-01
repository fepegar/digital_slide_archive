FROM python:3.9-slim

COPY provision.py .

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN curl -LJ https://github.com/krallin/tini/releases/download/v0.19.0/tini -o /usr/bin/tini && \
    chmod +x /usr/bin/tini

RUN pip install \
    histomicsui \
    large_image[sources] \
    girder-homepage \
    # girder-client is used to get sample data.  If you don't fetch sample \
    # data, it isn't necessary \
    girder-client \
    girder-oauth \
    --find-links https://girder.github.io/large_image_wheels

RUN girder build

COPY girder.cfg /etc/.

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD python provision.py --sample-data && girder serve --database mongodb://mongodb:27017/girder
