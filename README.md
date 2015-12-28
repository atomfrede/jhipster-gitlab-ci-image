moifort/jhipster-gitlab-ci-image:latest
==================

- [Introduction](#introduction)
	- [Source](#source)
	- [Current version](#current-version)
	- [Changelog](CHANGELOG.md)
- [Getting started](#getting-started)
  - [Gitlab Runner](#gitlab-runner)
  - [JHipster project](#jhipster-project)

# Introduction

It's a docker image. The purpose of this image is to build a JHipster application. The runner of GitlabCI can build/test/generate application inside a docker image. 

## Source

- [Github - https://github.com/moifort/jhipster-gitlab-ci-image](https://github.com/moifort/jhipster-gitlab-ci-image)
- [Docker - https://hub.docker.com/r/moifort/jhipster-gitlab-ci-image](https://hub.docker.com/r/moifort/jhipster-gitlab-ci-image/)

## Current version

Docker Version: **2.26.1** (Based on Jhipster version)

- Maven: 3.3.9
- NodeJS: 5.x
- Yo: 1.5.0
- Bower: 1.7.1
- Grunt: 0.1.13
- Gulp: 3.9.0

## Caching

The tag **XX.X.X-cache** is a specified image with all **node_modules** and **maven** dependencies. If you don't want to use cache, take
the the tag **XX.X.X**. By default `latest` tag is configured with the cache.

### Why use cache?

- Avoid network problems: JHipster download a lot dependencies and some time some dependency didn't work (403 error). And
it's stop the build.
- Build/Test/Package a JHipster application faster.


### If they are other dependencies (other than JHipster)?

Don't worry if you configure your `.gitlab-ci.yml` file like below, the missing dependencies will download automatically.


## Gitlab Runner 

Use my image moifort/docker-gitlab-ci-multi-runner with docker-compose:

- [Github - https://github.com/moifort/docker-gitlab-ci-multi-runner](https://github.com/moifort/docker-gitlab-ci-multi-runner)
- [Docker Hub - https://hub.docker.com/r/moifort/docker-gitlab-ci-multi-runner](https://hub.docker.com/r/moifort/docker-gitlab-ci-multi-runner)


### docker-compose.yml

```yml
gitlabmultirunner:
  image: moifort/docker-gitlab-ci-multi-runner:latest
  volumes:
    - ./data:/home/gitlab_ci_multi_runner/data
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

before_script:
  - cp -r /root/node_modules .
  - npm install

stages:
  - build
  - test
  - deploy

test-grunt:
  stage: test
  script: "grunt test"

test-maven:
  stage: test
  script: "mvn test"
```

> Remove `cp -r /root/node_modules .` if you not use cache.

### .bowerrc

I don't solve the problem yet, if you want to run the generation/test/build of your JHipster project you will need to add `"allow_root": true` to your `.bowerrc`.


```json
{
    "directory": "src/main/webapp/bower_components",
    "allow_root": true
}
```
