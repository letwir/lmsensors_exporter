lmsensors_exporter [![Build Status](https://travis-ci.org/mdlayher/lmsensors_exporter.svg?branch=master)](https://travis-ci.org/mdlayher/lmsensors_exporter) [![GoDoc](http://godoc.org/github.com/mdlayher/lmsensors_exporter?status.svg)](http://godoc.org/github.com/mdlayher/lmsensors_exporter)
==================

Command `lmsensors_exporter` provides a Prometheus exporter for
[lm_sensors](https://en.wikipedia.org/wiki/Lm_sensors) sensor metrics.
MIT Licensed.

Usage
-----

Available flags for `lmsensors_exporter` include:

```
$ ./lmsensors_exporter -h
Usage of ./lmsensors_exporter:
  -telemetry.addr string
        address for lmsensors exporter (default ":9165")
  -telemetry.path string
        URL path for surfacing collected metrics (default "/metrics")
```


ADD: install Service HOWTO
-----
# systemd service install
## lm-sensors install
```
sudo apt-get update
sudo apt-get install lm-sensors
yes | sudo sensors-detect
```
## exporter download
```
ARCH=arm64
# or amd64
sudo wget https://github.com/letwir/lmsensors_exporter/releases/download/0.1.1/lmsensors_exporter-$ARCH -O /usr/local/bin/lmsensors_exporter
sudo chmod +x /usr/local/bin/lmsensors_exporter
```
## enable service
```
sudo tee /etc/systemd/system/lmsensors_exporter.service << EOF
[Unit]
Description=lmsensors exporter service
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/lmsensors_exporter
User=root
Group=root
SyslogIdentifier=lmsensors_exporter
Restart=on-failure
RemainAfterExit=no
RestartSec=100ms
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable lmsensors_exporter.service
sudo systemctl start lmsensors_exporter.service
```
