# Knative RabbitMQ Lambda Source

**What:** Knative RabbitMQ Lambda Sources is a Knative event sources for a RabbitMQ cluster.

**Why:** You maybe be using some RabbitMQ setup but still interested to run workloads within Kubernetes and soon via [Knative](https://github.com/knative/docs) to benefit from features such as scale to zero and source-to-url FaaS functionality. To trigger those workloads when events happen in a your queue of RabbitMQ you need to have an event source that can consume RabbitMQ events and send them to your workload. This is a key principle in Knative eventing.

**How:** The source listed in this repo are fully open source and can be used in any Knative cluster. This is a Go event consumers for RabbitMQ. Most of them are packaged as `Container Sources` and make use of [CloudEvents](https://cloudevents.io/)

## Sources and Usage

Most sources have the following structure:

```shell
├── rabbitmq
│   ├── Dockerfile
│   ├── Gopkg.lock
│   ├── Gopkg.toml
│   ├── Makefile
│   ├── README.md
│   ├── rabbitmq-source.yaml
│   ├── main.go
│   └── main_test.go
```

The code is in `main.go`. The `Dockerfile` shows how the source is containerized and the `rabbitmq-source.yaml` is the `ContainerSource` manifest that you can deploy on your knative cluster.

For information on what is Knative please see the Knative [documentation](https://github.com/knative/docs/tree/master/eventing).

### Credentials

In the example manifests provided in this repo, the AWS credentials are loaded via a Kubernetes secret named `rabbitmq` which needs to contain the keys `aws_access_key_id` and `aws_secret_access_key`.

## External ressource
You can find other lambda source here : https://github.com/triggermesh/knative-lambda-sources. This repo was inspired by them.
