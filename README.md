moifort/jhipster-gitlab-ci-image:2.26.1
==================

- [Introduction](#introduction)
	- [Source](#source)
	- [Current version](#current-version)
	- [Changelog](CHANGELOG.md)
- [Getting started](#getting-started)
  - [Gitlab runner](#gitlab-runner)
  - [JHipster project](#jhipster-project)

# Introduction

It's a docker image. The purpose of this image is to build a JHipster application. The runner of GitlabCI can build/test/generate application inside a docker image. 

## Source

- [Github - https://github.com/moifort/jhipster-gitlab-ci-image](https://github.com/moifort/jhipster-gitlab-ci-image)
- [Docker - https://hub.docker.com/r/moifort/jhipster-gitlab-ci-image](https://hub.docker.com/r/moifort/jhipster-gitlab-ci-image/)

## Current version

Docker Version: **2.26.1** (Based on Jhipster version)

- JHipster: 2.26.1
- Maven: 3.3.9
- NodeJS: 5.x
- Yo: 1.5.0
- Bower: 1.7.1
- Grunt: 0.1.13
- Gulp: 3.9.0

# Getting started

## Gitlab runner 

I use docker compose with sameersbn/docker-gitlab-ci-multi-runner: 

- [Github - https://github.com/sameersbn/docker-gitlab-ci-multi-runner](https://github.com/sameersbn/docker-gitlab-ci-multi-runner)
- [Docker - https://hub.docker.com/r/sameersbn/gitlab-ci-multi-runner/](https://hub.docker.com/r/sameersbn/gitlab-ci-multi-runner/)


### docker-compose.yml

```yml
gitlabmultirunner:
  image: moifort/docker-gitlab-ci-multi-runner:latest
  volumes:
    - ./data/data:/home/gitlab_ci_multi_runner/data
    - /var/run/docker.sock:/var/run/docker.sock
  environment:
    - CI_SERVER_URL=
    - RUNNER_TOKEN=
    - RUNNER_DESCRIPTION=jhipster
    - RUNNER_EXECUTOR=docker
  restart: always
```

## JHipster project

### .gitlab-ci.yml

At the root to your JHipster application `.gitlab-ci.yml` file. When you push your commit, Gitlab automaticaly run the test.

```yml
image: moifort/jhipster-gitlab-ci-image:latest

test:
  script:
    - "mvn test"
```

### .bowerrc

I don't solve the problem yet, if you want to run the generation/test/build of your JHipster project you will need to add `"allow_root": true` to your `.bowerrc`.


```json
{
    "directory": "src/main/webapp/bower_components",
    "allow_root": true
}
```
