# Arcaflow Example Workflows

This repo contains workflows created and maintained by the Arcaflow team. The
[basic examples](basic-examples) show how to use particular features of workflows. The
[advanced examples](advanced-examples) provide more complex real-world use cases.

> [!NOTE]
> The Arcaflow engine executes the steps of a workflow via *deployers* for one or more
> container platorms, such as
> [Podman](https://github.com/arcalot/arcaflow-engine-deployer-podman),
> [Docker](https://github.com/arcalot/arcaflow-engine-deployer-docker),
> or [Kubernetes](https://github.com/arcalot/arcaflow-engine-deployer-kubernetes).
> Arcaflow's default deployer is Podman. To override this default, a
> [configuration file](https://arcalot.io/arcaflow/running/setup/#configuration) must be
> used with the `arcaflow` command. A basic [Docker config file](/docker-config.yaml) is
> included in this repo's root directory and can be used with any of these example
> workflows via the `--config` parameter.

## Contributing

You can use a cluster local to your machine to functionally test your workflows. See the
[local development documentation](docs/local-development.md).