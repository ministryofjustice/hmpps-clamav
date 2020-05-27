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
docker build -t mojdigitalstudio/clamav:latest .
docker tag  mojdigitalstudio/clamav:latest  mojdigitalstudio/clamav:0.102.X_Y
docker push mojdigitalstudio/clamav:latest
docker push mojdigitalstudio/clamav:0.102.X_Y
```

## Circle CI

Whenever merges are made to the master branch a circle CI job is triggered to build a new image.

## Deployment instructions

The docker image can be deployed and run in the same way as any docker image, and for HMPPS this includes:

 - Locally, as part of a docker-compose'd set of inter-dependent services
 - As part of a Kubernetes deployment (deployment is managed by each service requiring AV)

This docker image is mostly borrowed from: <https://github.com/UKHomeOffice/docker-clamav> ...but with a few changes:

- Combined a RUN step to make the image smaller
- Removed the freshclam initial run from then build - not needed as entrypoint script runs it
- Specified a specific group ID for clamav, needed for the security context in the K8s volume mount
- Updated the clamav version to the current latest stable version

Upon merging to master, the docker image will be built and pushed to the quay.io image repository.


## To pull the latest image:

```bash
docker pull mojdigitalstudio/clamav:latest
```

## After a rebuild / push of the image (via CicleCI)

Each service which makes use of this will need to handle its own deployment.

## For a local development environment, run freshclam and clamd standalone:

```bash
docker run -d -p 3310:3310  mojdigitalstudio/clamav:latest
```

