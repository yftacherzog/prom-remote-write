# Play locally with Prometheus remote-write configurations
The `config` directory contains configurations for 2 Prometheus instances. You
can change the configs on each directory and restart the containers with the
commands below to see how different configs affect the metrics/labels that are
remote-written from the writer to the reader instance.

Once the instances are up, browse to http://localhost:9090 and http://localhost:9070
to check the targets and graphs.
## Reader
The reader is configured to receive remote-write (without authentication) and
defines no scrapping targets of its own.

```
podman run --rm \
    --privileged \
    -p 9090:9090 \
    -v ./config/reader:/etc/prometheus \
    prom/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --web.enable-remote-write-receiver
```
## Writer
The writer is configured to remote-write to the reader and is scraping its own
target, so it will have something to write to the reader.
```
podman run --rm \
    --privileged \
    -p 9070:9090 \
    -v ./config/writer:/etc/prometheus \
    prom/prometheus \
    --config.file=/etc/prometheus/prometheus.yml
```
