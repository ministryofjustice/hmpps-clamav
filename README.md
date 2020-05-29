# hmpps-clamav

[![CircleCI](https://circleci.com/gh/ministryofjustice/hmpps-clamav/tree/master.svg?style=svg)](https://circleci.com/gh/ministryofjustice/hmpps-clamav)
[![Known Vulnerabilities](https://snyk.io/test/github/ministryofjustice/hmpps-clamav/badge.svg)](https://snyk.io/test/github/ministryofjustice/hmpps-clamav)

Repository which is used to build a tailored version of a ClamAV docker image.

## Build instructions

You will need the following tools installed:

| Tool           |  Version  |  Reason                                                            |
|----------------|-----------|--------------------------------------------------------------------|
| docker         |  18.x     | Bulding the docker image                                           |

## To test with a local build:

```bash
docker build -t hmpps/hmpps-clamav:latest .
docker tag  hmpps/hmpps-clamav:latest  hmpps/hmpps-clamav:0.102.X_Y
```

## To manually push the image built to quay.io (though should not be needed, as circleci will do this) :

```bash
docker push quay.io/hmpps/hmpps-clamav:latest
docker push quay.io/hmpps/hmpps-clamav:0.102.X_Y
```

## Circle CI

Whenever merges are made to the master branch a circle CI job is triggered to build & push a new image to quay.io.

## Deployment instructions

The docker image can be deployed and run in the same way as any docker image, and for HMPPS this includes:

 - Locally, as part of a docker-compose set of inter-dependent services
 - As part of a Kubernetes deployment (deployment is managed by each service requiring AV)

This docker image is mostly borrowed from: <https://github.com/UKHomeOffice/docker-clamav> ...but with a few changes:

- Combined a RUN step to make the image smaller
- Removed the freshclam initial run from then build - not needed as entrypoint script runs it
- Specified a specific group ID for clamav, needed for the security context in the K8s volume mount
- Updated the clamav version to the current latest stable version

## To pull the latest image:

```bash
docker pull quay.io/hmpps/hmpps-clamav:latest
```

## After a rebuild / push of the image (via CicleCI)

Each service which makes use of this will need to handle its own deployment - see `pathfinder` for an example in k8s.

## For a local development environment, run freshclam and clamd standalone:

```bash
docker run -d -p 3310:3310  hmpps/hmpps-clamav:latest
```

