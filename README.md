# Concourse Docker

This Docker image simply packages up the official `concourse` binary and
configures it as the `ENTRYPOINT`, with a bunch of sane defaults for Docker.

Configuration is done via `CONCOURSE_*` environment variables. To discover
them, run `--help`:

```sh
docker run concourse/concourse --help
docker run concourse/concourse web --help
docker run concourse/concourse worker --help
```
Usage
Quick-start
See the Quick-start folder.

Generate private keys
Run generate-keys.sh to create keys to be used by the web host and workers.

Environment variables
CONCOURSE_EXTERNAL_URL - The URL to access concourse web at. Usually http://192.168.99.100:8080 or http://localhost:8080
CONCOURSE_LOGIN - Username to use for concourse basic auth.
CONCOURSE_PASSWORD = Password to use for concourse basic auth.
Start Concourse
Run:

docker-compose up
Open the CONCOURSE_EXTERNAL_URL specified above (http://192.168.99.100:8080) and start using concourse.

See Using Concourse to get started.

See [the `concourse` binary docs](https://concourse-ci.org/install.html) for
more information - the instructions and requirements are the same. For network
and hardware resources reference, see [Deployment
Topology](https://concourse-ci.org/topology.html).


## Docker Compose

There are two Docker Compose `.yml` files in this repo. The first one,
`docker-compose.yml`, runs a more traditional multi-container cluster. You'll
need to run `./generate-keys.sh` before booting up, so that the containers know
how to authorize each other.

The `docker-compose-quickstart.yml` file can be used to quickly get up and
running with the `concourse quickstart` command. No keys need to be generated
in this case.


## Caveats

At the moment, workers running via Docker will not automatically leave the
cluster gracefully on shutdown. This means you'll have to run [`fly
prune-worker`](https://concourse-ci.org/administration.html#fly-prune-worker)
to reap them.

This will be resolved with [concourse/concourse
#1457](https://github.com/concourse/concourse/issues/1457).
