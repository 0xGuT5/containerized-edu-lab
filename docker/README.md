# Packet Tracer container image

Ubuntu 20.04 + Packet Tracer 8.1.1, exposed over VNC by `x11vnc -create`,
which spawns an X server and runs `/usr/local/bin/packettracer` via `~/.xinitrc`.

Packet Tracer is not redistributable, so `packettracer_8.1.1_amd64.deb` is not
in this repo. Download it from Cisco Netacad, drop it here, then:

```sh
docker build -t ayoubmokhtari/pt8 .
```

See the [root README](../README.md) for the Kubernetes deployment.
