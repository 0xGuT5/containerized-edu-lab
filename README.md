# Containerized Education Lab

A web-accessible student lab platform: Cisco Packet Tracer 8.1.1 running as a
container and served over VNC, deployed on Kubernetes alongside a web interface
and a PostgreSQL/pgAdmin backend. Students reach the tools from a browser with
zero local installation.

Built in June 2022 as the lab infrastructure for a networking course at ESI
(2CS), so a whole class could use Packet Tracer without installing it on every
machine.

## Layout

| Path                   | What it is                                    |
| ---------------------- | --------------------------------------------- |
| `docker/`              | Packet Tracer image — Ubuntu 20.04 + x11vnc    |
| `k8s/packettracer.yaml`| Packet Tracer Deployment + NodePort service    |
| `k8s/webapp.yaml`      | Web interface Deployment + NodePort service    |
| `k8s/postgres.yaml`    | PostgreSQL 11 + PV/PVC, ConfigMap, Secret      |
| `k8s/pgadmin.yaml`     | pgAdmin 4 Deployment + NodePort service        |

## Services

| Service      | NodePort | Target      |
| ------------ | -------- | ----------- |
| Packet Tracer| 30170    | 5900 (VNC)  |
| Web interface| 30106    | 80          |
| pgAdmin      | 30165    | 80          |
| PostgreSQL   | —        | 5432        |

## Building the image

Packet Tracer is not redistributable, so the `.deb` is deliberately not in this
repo. Download `packettracer_8.1.1_amd64.deb` from Cisco Netacad and place it
next to the Dockerfile:

```sh
cd docker
cp ~/Downloads/packettracer_8.1.1_amd64.deb .
docker build -t ayoubmokhtari/pt8 .
```

The image starts `x11vnc -create -forever`, which spawns an X server and runs
`/usr/local/bin/packettracer` from `~/.xinitrc`. Connect any VNC client to
port 5900.

## Deploying

```sh
kubectl apply -f k8s/
```

`k8s/postgres.yaml` expects a hostPath at `/mnt/nfs/postgres-data` on the node,
backing a 5Gi `ReadWriteMany` PersistentVolume.

## Note on credentials

The values in `k8s/postgres.yaml` and `k8s/pgadmin.yaml` are lab placeholders
for a throwaway classroom cluster, they were never real.
