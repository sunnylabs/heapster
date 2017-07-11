# Heapster

[![GoDoc](https://godoc.org/k8s.io/heapster?status.svg)](https://godoc.org/k8s.io/heapster) [![Build Status](https://travis-ci.org/kubernetes/heapster.svg?branch=master)](https://travis-ci.org/kubernetes/heapster)

Heapster enables Container Cluster Monitoring and Performance Analysis.

Heapster currently supports [Kubernetes](https://github.com/kubernetes/kubernetes) and CoreOS natively.
*Heapster is compatible with kubernetes versions starting from v1.0.6 only*

It can be extended to support other cluster management solutions easily.

Heapster collects and interprets various signals like compute resource usage, lifecycle events, etc, and exports cluster metrics via [REST endpoints](docs/model.md).
**Note: Some of the endpoints are only valid in Kubernetes clusters**

Heapster supports multiple sources of data.
More information [here](docs/source-configuration.md).

Heapster supports the pluggable storage backends described [here](docs/sink-owners.md).
We welcome patches that add additional storage backends.
Documentation on storage sinks [here](docs/sink-configuration.md).
The current version of Storage Schema is documented [here](docs/storage-schema.md).

### Running Heapster on Kubernetes

To run Heapster on a Kubernetes cluster with,
- InfluxDB use [this guide](docs/influxdb.md).
- Google Cloud Monitoring and Google Cloud Logging use [this guide](docs/google.md).

### Running Heapster on OpenShift

Using Heapster to monitor an OpenShift cluster requires some additional changes to the Kubernetes instructions to allow communication between the Heapster instance and OpenShift's secured endpoints. To run standalone Heapster or a combination of Heapster and Hawkular-Metrics in OpenShift, follow [this guide](https://github.com/openshift/origin-metrics).

#### Troubleshooting guide [here](docs/debugging.md)


## Community

Contributions, questions, and comments are all welcomed and encouraged! minkube developers hang out on [Slack](https://kubernetes.slack.com) in the #sig-instrumentation channel (get an invitation [here](http://slack.kubernetes.io/)). We also have the [kubernetes-dev Google Groups mailing list](https://groups.google.com/forum/#!forum/kubernetes-dev). If you are posting to the list please prefix your subject with "heapster: ".

## Building Heapster and Eventer binaries and containers
At the moment, you can build the container on mac, but you will get a 
runtime error if you run that container on linux.

### Instructions for building binaries and containers on linux
Heapster can be slow at landing 3rd party PR's
We currently have an open PR https://github.com/kubernetes/heapster/pull/1713
Until it lands, we need to build our own container, and encourage customers to deploy with our heapster container at https://hub.docker.com/r/wavefronthq/heapster
install go according to https://golang.org/doc/install

    ~> export GOPATH=~/go && cd $GOPATH
    go> mkdir -p src/k8s.io && cd !$
    k8s.io> git clone github.com/sunnylabs/heapster.git && cd heapster
    heapster> make test-unit
    heapster> make
    heapster> mv eventer heapster deploy/docker && cd !$
    docker> cp /etc/ssl/certs/ca-certificates.crt .
    docker> version=v1.4.0.1  sudo docker build -t ${version}-g$(git rev-parse --short HEAD) .
    docker> docker login
    docker> docker push wavefronthq/heapster:${version}-g$(git rev-parse --short HEAD)
    Then update heapster.yml in sunnylabs/k8s/clusters tree

