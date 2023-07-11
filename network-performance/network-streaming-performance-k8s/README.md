# Network Streaming Performance Workflow for Kubernetes

***NOTE: This is a work-in-progress workflow.***

## Workflow Description

This example workflow runs a network streaming one-directional workload using the
[uperf](https://github.com/uperf/uperf) benchmark utility. The uperf plugin will
run on a Kubernetes cluster using a provided `kubeconfig` object and will use a kubernetes service for communication between the uperf server and client pods.

In addition to the uperf workload, the workflow generates a UUID, which can be used as a unique key for the generated data, collects system metrics with [Performance Co-pilot](https://pcp.io/), and collects system metadata using Ansible [gather facts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/gather_facts_module.html).

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to run
  and their data relationships, and the output to present to the user
- [`input.yaml`](input.yaml) -- The input parameters that the user provides for running
  the workflow
- [`config.yaml`](config.yaml) -- Global config parameters that are passed to the Arcaflow
  engine
                     
## Running the Workflow

You will need a Golang runtime and Docker to run the containers (Podman can
be used with the [system service](https://docs.podman.io/en/latest/markdown/podman-system-service.1.html)
enabled for socket connections, which are required by the Arcaflow engine to
communicate with the plugins).

Clone the engine:
```
$ git clone git@github.com:arcalot/arcaflow-engine.git
```

Clone this workflows repo, and set this directory to your workflow working directory (adjust as needed):
```
$ git clone https://github.com/arcalot/arcaflow-workflows.git
$ export WFPATH=$(pwd)/arcaflow-workflows/network-streaming-performance-k8s
```
 
Run the workflow:
```
$ cd arcaflow-engine
$ go run cmd/arcaflow/main.go -input ${WFPATH}/input-example.yaml \
-config ${WFPATH}/config.yaml -context ${WFPATH}
```

## Workflow Diagram
This diagram shows the complete end-to-end workflow logic.

```mermaid
flowchart LR
subgraph input
input.uperf_server_timeout_seconds
input.uperf_kbytes
input.uperf_runtime_seconds
input.uperf_nthreads
input.pmlogger_interval
input.run_id
input.kubeconfig
input.uperf_protocol
end
steps.uuidgen.outputs.success-->output
input.uperf_server_timeout_seconds-->steps.pcp
input.uperf_server_timeout_seconds-->steps.uperf_server
steps.uperf_client-->steps.uperf_client.outputs.error
steps.uperf_client-->steps.uperf_client.outputs.success
steps.pcp-->steps.pcp.outputs.error
steps.pcp-->steps.pcp.outputs.success
input.uperf_kbytes-->steps.uperf_client
steps.pcp.outputs.success-->output
input.uperf_runtime_seconds-->steps.uperf_client
steps.service.outputs.success-->steps.uperf_client
steps.kubeconfig-->steps.kubeconfig.outputs.success
steps.kubeconfig-->steps.kubeconfig.outputs.error
input.uperf_nthreads-->steps.uperf_client
steps.metadata-->steps.metadata.outputs.success
steps.metadata-->steps.metadata.outputs.error
steps.uuidgen-->steps.uuidgen.outputs.success
steps.uuidgen-->steps.uuidgen.outputs.error
input.pmlogger_interval-->steps.pcp
input.run_id-->output
steps.uperf_client.outputs.success-->output
steps.kubeconfig.outputs.success-->steps.uperf_client
steps.kubeconfig.outputs.success-->steps.metadata
steps.kubeconfig.outputs.success-->steps.pcp
steps.kubeconfig.outputs.success-->steps.uperf_server
steps.kubeconfig.outputs.success-->steps.service
steps.metadata.outputs.success-->output
steps.uperf_server-->steps.uperf_server.outputs.success
steps.uperf_server-->steps.uperf_server.outputs.error
input.kubeconfig-->steps.kubeconfig
input.uperf_protocol-->steps.uperf_client
steps.service-->steps.service.outputs.error
steps.service-->steps.service.outputs.success
```
