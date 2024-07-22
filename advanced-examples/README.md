# Advanced Workflow Examples

> [!NOTE]
> The Advanced Workflow Examples are a work-in-progress and may not be entirely complete
> or functional at this time.

## Network Performance

- [iPerf3 Kubernetes](network-performance/network-performance-iperf3-k8s/) - Runs an iPerf3 network performance test on a Kubernetes cluster using a service port
- [uperf Kubernetes](network-performance/network-performance-uperf-k8s/) - Runs a
network streaming one-directional workload using the uperf benchmark utility on a
Kubernetes cluster

## System Performance

- [stress-ng Kubernetes](system-performance/stressng-k8s/) - Runs a stress-ng workload
  plugin on a Kubernetes target system
- [stress-ng plus PCP](system-performance/stressng-pcp/) - A complex workflow exercising
  many Arcaflow features. Collects system metadata, and runs multiple loops of stress-ng
  loads while collecting system performance metrics with Performance Co-Pilot
- [sysbench](system-performance/sysbench/) - Collects system metadata, and runs a
  sysbench CPU workload plugin on the local system while collecting system performance
  metrics with Performance Co-Pilot, then indexes the data to Elasticsearch

> [!NOTE]
> The Arcaflow engine executes the steps of a workflow via *deployers* for one or more
> container platforms, such as
> [Podman](https://github.com/arcalot/arcaflow-engine-deployer-podman),
> [Docker](https://github.com/arcalot/arcaflow-engine-deployer-docker),
> or [Kubernetes](https://github.com/arcalot/arcaflow-engine-deployer-kubernetes).
> Arcaflow's default deployer is Podman. To override this default, specify a
> [configuration file](https://arcalot.io/arcaflow/running/setup/#configuration) via the
> `--config` option on the `arcaflow` command line. A basic
> [Docker config file](/docker-config.yaml) is included in this repo's root directory
> and can be used with any of these example workflows.
