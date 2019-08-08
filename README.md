# PodMan-Compose

[PodMan-Compose](https://github.com/muayyad-alsadi/podman-compose) is a script to run `docker-compose.yml` using [podman](https://podman.io/),
doing necessary mapping to make it work rootless.

This fork demonstrates how to enforce container authentication and block the start of unwanted container. 
More information on our [blog post](https://medium.com/@opvizor/run-only-compliant-container-with-podman-and-podman-compose-5cd493b530b0).

## Usage
```
usage: podman-compose.py [-h] [-f FILE] [-p PROJECT_NAME]
                         [--podman-path PODMAN_PATH] [--no-ansi]
                         [--no-cleanup] [--dry-run]
                         [-t {1pod,1podfw,hostnet,cntnet,publishall,identity}]
                         [--enforce-content-trust]
                         command ...

positional arguments:
  command               command to run
  args

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  Specify an alternate compose file (default: docker-
                        compose.yml)
  -p PROJECT_NAME, --project-name PROJECT_NAME
                        Specify an alternate project name (default: directory
                        name)
  --podman-path PODMAN_PATH
                        Specify an alternate path to podman (default: use
                        location in $PATH variable)
  --no-ansi             Do not print ANSI control characters
  --no-cleanup          Do not stop and remove existing pod & containers
  --dry-run             No action; perform a simulation of commands
  -t {1pod,1podfw,hostnet,cntnet,publishall,identity}, --transform_policy {1pod,1podfw,hostnet,cntnet,publishall,identity}
                        how to translate docker compose to podman
                        [1pod|hostnet|accurate]
  --enforce-content-trust
                        Enable content trust enforcement; stop the execution
                        if a not verified image is encountered

## NOTE

it's still underdevelopment and does not work yet.

## Mappings

* `1podfw` - create all containers in one pod (inter-container communication is done via `localhost`), doing port mapping in that pod
* `1pod` - create all containers in one pod, doing port mapping in each container
* `identity` - no mapping
* `hostnet` - use host network, and inter-container communication is done via host gateway and published ports
* `cntnet` - create a container and use it via `--network container:name` (inter-container communication via `localhost`)
* `publishall` - publish all ports to host (using `-P`) and communicate via gateway
```

## Examples

```
./podman-compose.py --enforce-content-trust up
```
