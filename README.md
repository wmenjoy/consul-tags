# Consul Service Tags Modifier

[![GitHub version](https://badge.fury.io/gh/hedzr%2Fconsul-tags.svg)](https://badge.fury.io/gh/hedzr%2Fconsul-tags)
[![Build Status](https://travis-ci.org/hedzr/consul-tags.svg?branch=master)](https://travis-ci.org/hedzr/consul-tags)
[![license](https://img.shields.io/github/license/hedzr/consul-tags.svg)]()
[![Docker Automated buil](https://img.shields.io/docker/automated/hedzr/consul-tags.svg)]()
[![Docker Pulls](https://img.shields.io/docker/pulls/hedzr/consul-tags.svg)]()
<!-- [![GitHub tag](https://img.shields.io/github/tag/hedzr/consul-tags.svg)]() -->
<!-- [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/hedzr/consul-tags/latest.svg)]() -->

`consul-tags` can add, remove tag(s) of a consul service (one or all its instances).

Here is a first release for key functionality.


## Install

### Binary

Download binary from [Release](../../releases/latest) page.

### Docker Hub

```bash
docker pull hedzr/consul-tags
docker run -it --rm hedzr/consul-tags --addr 192.168.0.71:8500 ms --name test-redis tags ls
```

Replace `192.168.0.71` with your consul center ip or name.

DON'T use `127.0.0.1` with dockerize release.

> latest: master branch and based on golang:alpine


### Go Build

clone the repo and build:

```bash
go build -o consul-tags github.com/hedzr/consul-tags
```

mixin:

```bash
go get -u github.com/hedzr/consul-tags/objects/consul
```


## Usage


```bash

# run consul demo instance for testing
./build.sh consul run &

# use the local consul demo instance as default addrress, see also `--addr` in `consul-tags ms --help`
export CT_APP_MS_ADDR=localhost:8500

# list tags
consul-tags ms --name test-redis tags ls
consul-tags ms tags ls --name test-redis
# modify tags
consul-tags ms tags modify --name test-redis tags --add a,c,e
consul-tags ms tags mod --name test-redis --add a --add c --add e
consul-tags ms tags mod --name test-redis --rm a,c,e
consul-tags ms tags mod --name test-redis --rm a --rm c --rm e
# by id
consul-tags ms tags ls --id test-redis-1

# toggle master/slave
consul-tags ms tags toggle --name test-redis --addr 10.7.13.1:6379 --set role=master --reset role=slave
consul-tags ms tags tog --name test-mq --addr 10.7.16.3:5672 --set role=leader,type=ram --reset role=peer,type=disk

# get commands and sub-commands help
consul-tags ms -h
# get help: -h or --help
consul-tags -h
```

Default consul address is `consul.ops.local:8500`, can be overridden with environment variable `CT_APP_MS_ADDR` (host:port), too. Such as:

```
CONSUL_ADDR=127.0.0.1:8500 consul-tags ms --name=consul tags ls
# or
export CONSUL_HOST=ns2.company.domain:8500
consul-tags --help
# or
consul-tags --addr 127.0.0.1:8500 ms --name=consul tags ls 
```

### COMMANDS:

```
     kv              K/V pair Operations, ...
     ms, service, m  Microservice Operations, ...
     help, h         Shows a list of commands or help for one command
```

### GLOBAL OPTIONS:

```
   --help, -h                          show help (default: false)
   --init-completion value             generate completion code. Value must be 'bash' or 'zsh'
   --version, -v                       print the version (default: false)
```

## Shell completion



```bash
eval "`consul-tags --init-completion bash`"
consul-tags --init-completion bash >> ~/.bashrc
```

or put it to `/etc/bash_completion.d/`:

```bash
cp bin/consul-tags /usr/local/bin/
/usr/local/bin/consul-tags --init-completion bash | sudo tee /etc/bash_completion.d/consul-tags
source /etc/bash_completion.d/consul-tags
```

See also [Shell Completion](https://github.com/urfave/cli/tree/v2#shell-completion).

zsh: DOC TODO

Mac: DOC TODO


## Thanks

my plan is building a suite of devops. consul operations is part of it. exception consul tags modifying operation, these codes ported in:

- [colebrumley/consul-kv-backup](https://github.com/colebrumley/consul-kv-backup)

and some repositories are good:

- https://github.com/shreyu86/consul-backup
- https://github.com/kailunshi/consul-backup

### Third-party repos

Much more and mutable, not list here. But one of them:

- https://github.com/urfave/cli/tree/v2



## License

MIT.




