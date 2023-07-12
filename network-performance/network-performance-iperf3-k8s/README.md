# Network Performance Tests with iperf3

## Workflow Description

This workflow runs an iperf3 network performance test on a Kubernetes cluster using a service port. The workflow collects metadata from a cluster node, as well as node-level performance metrics using Performance Co-Pilot (PCP). Additionally it generates a unique UUID to associate with the data in order to aid as an index reference when storing data.

## TODO

This workflow needs a few updates as features become available in the Arcaflow engine:

- Split iperf3 plugin into a sub-workflow
  - This needs a way to pass the kubernetes connection object to the sub-workflow
- PCP data collection should be run using smart parallelization with the iperf3 client
- PCP and metadata plugins should be scheduled using pod affinity with the iperf3 client
- Need to support pod and host network and not just service network
- Kubeconfig should be passed as a file instead of inline in the `input.yaml` file
- The iperf3 client should wait for the iperf3 server to be ready before starting

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to run
  and their data relationships, and the output to present to the user
- [`input.yaml`](input.yaml) -- The input parameters that the user provides for running
  the workflow
- [`config.yaml`](config.yaml) -- Global config parameters that are passed to the Arcaflow
  engine
                     
## Running the Workflow

### Workflow Execution

Download a Go binary of the latest version of the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```
$ export WFPATH=<path to this workflow directory>
$ arcaflow -input ${WFPATH}/input.yaml -config ${WFPATH}/config.yaml -context ${WFPATH}
```

## Workflow Diagram
```mermaid
flowchart LR
steps.iperf3_server.cancelled-->steps.iperf3_server.outputs
steps.iperf3_server.cancelled-->steps.iperf3_server.crashed
steps.iperf3_server.cancelled-->steps.iperf3_server.deploy_failed
steps.pcp.crashed-->steps.pcp.crashed.error
steps.metadata.deploy-->steps.metadata.starting
steps.metadata.deploy-->steps.metadata.deploy_failed
steps.metadata.starting-->steps.metadata.running
steps.metadata.starting-->steps.metadata.crashed
steps.service.outputs-->steps.service.outputs.success
steps.service.outputs-->steps.service.outputs.error
steps.uuidgen.starting-->steps.uuidgen.running
steps.uuidgen.starting-->steps.uuidgen.crashed
steps.iperf3_server.starting-->steps.iperf3_server.running
steps.iperf3_server.starting-->steps.iperf3_server.crashed
steps.iperf3_server.deploy_failed-->steps.iperf3_server.deploy_failed.error
steps.iperf3_server.crashed-->steps.iperf3_server.crashed.error
steps.iperf3_client.deploy_failed-->steps.iperf3_client.deploy_failed.error
steps.pcp.deploy-->steps.pcp.starting
steps.pcp.deploy-->steps.pcp.deploy_failed
steps.pcp.outputs.success-->outputs.success
steps.iperf3_client.outputs.success-->outputs.success
steps.iperf3_client.crashed-->steps.iperf3_client.crashed.error
steps.kubeconfig.deploy_failed-->steps.kubeconfig.deploy_failed.error
steps.service.deploy_failed-->steps.service.deploy_failed.error
steps.iperf3_client.running-->steps.iperf3_client.outputs
steps.iperf3_client.running-->steps.iperf3_client.crashed
steps.kubeconfig.outputs.success-->steps.iperf3_server.deploy
steps.kubeconfig.outputs.success-->steps.service.starting
steps.kubeconfig.outputs.success-->steps.iperf3_client.deploy
steps.kubeconfig.outputs.success-->steps.metadata.deploy
steps.kubeconfig.outputs.success-->steps.pcp.deploy
steps.service.deploy-->steps.service.starting
steps.service.deploy-->steps.service.deploy_failed
steps.metadata.deploy_failed-->steps.metadata.deploy_failed.error
steps.kubeconfig.running-->steps.kubeconfig.outputs
steps.kubeconfig.running-->steps.kubeconfig.crashed
steps.uuidgen.outputs.success-->outputs.success
steps.pcp.starting-->steps.pcp.running
steps.pcp.starting-->steps.pcp.crashed
steps.iperf3_server.running-->steps.iperf3_server.outputs
steps.iperf3_server.running-->steps.iperf3_server.crashed
steps.uuidgen.deploy_failed-->steps.uuidgen.deploy_failed.error
steps.service.outputs.success-->steps.iperf3_client.starting
steps.iperf3_client.cancelled-->steps.iperf3_client.crashed
steps.iperf3_client.cancelled-->steps.iperf3_client.deploy_failed
steps.iperf3_client.cancelled-->steps.iperf3_client.outputs
steps.uuidgen.crashed-->steps.uuidgen.crashed.error
steps.metadata.running-->steps.metadata.outputs
steps.metadata.running-->steps.metadata.crashed
steps.uuidgen.deploy-->steps.uuidgen.deploy_failed
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.pcp.deploy_failed-->steps.pcp.deploy_failed.error
steps.service.starting-->steps.service.running
steps.service.starting-->steps.service.crashed
steps.uuidgen.running-->steps.uuidgen.outputs
steps.uuidgen.running-->steps.uuidgen.crashed
steps.service.crashed-->steps.service.crashed.error
steps.metadata.outputs-->steps.metadata.outputs.error
steps.metadata.outputs-->steps.metadata.outputs.success
input-->steps.metadata.deploy
input-->steps.pcp.starting
input-->steps.service.starting
input-->steps.iperf3_client.deploy
input-->steps.kubeconfig.starting
input-->steps.iperf3_server.deploy
input-->outputs.success
input-->steps.pcp.deploy
input-->steps.iperf3_server.starting
input-->steps.iperf3_client.starting
steps.iperf3_client.outputs-->steps.iperf3_client.outputs.success
steps.iperf3_client.outputs-->steps.iperf3_client.outputs.error
steps.iperf3_client.deploy-->steps.iperf3_client.starting
steps.iperf3_client.deploy-->steps.iperf3_client.deploy_failed
steps.pcp.outputs-->steps.pcp.outputs.success
steps.pcp.outputs-->steps.pcp.outputs.error
steps.iperf3_server.outputs-->steps.iperf3_server.outputs.success
steps.iperf3_server.outputs-->steps.iperf3_server.outputs.error
steps.pcp.cancelled-->steps.pcp.deploy_failed
steps.pcp.cancelled-->steps.pcp.outputs
steps.pcp.cancelled-->steps.pcp.crashed
steps.uuidgen.outputs-->steps.uuidgen.outputs.error
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.pcp.running-->steps.pcp.crashed
steps.pcp.running-->steps.pcp.outputs
steps.iperf3_client.starting-->steps.iperf3_client.crashed
steps.iperf3_client.starting-->steps.iperf3_client.running
steps.metadata.cancelled-->steps.metadata.outputs
steps.metadata.cancelled-->steps.metadata.crashed
steps.metadata.cancelled-->steps.metadata.deploy_failed
steps.service.cancelled-->steps.service.outputs
steps.service.cancelled-->steps.service.crashed
steps.service.cancelled-->steps.service.deploy_failed
steps.kubeconfig.cancelled-->steps.kubeconfig.outputs
steps.kubeconfig.cancelled-->steps.kubeconfig.crashed
steps.kubeconfig.cancelled-->steps.kubeconfig.deploy_failed
steps.metadata.crashed-->steps.metadata.crashed.error
steps.service.running-->steps.service.outputs
steps.service.running-->steps.service.crashed
steps.iperf3_server.deploy-->steps.iperf3_server.starting
steps.iperf3_server.deploy-->steps.iperf3_server.deploy_failed
steps.kubeconfig.deploy-->steps.kubeconfig.starting
steps.kubeconfig.deploy-->steps.kubeconfig.deploy_failed
steps.kubeconfig.outputs-->steps.kubeconfig.outputs.success
steps.kubeconfig.outputs-->steps.kubeconfig.outputs.error
steps.kubeconfig.crashed-->steps.kubeconfig.crashed.error
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.uuidgen.cancelled-->steps.uuidgen.crashed
steps.uuidgen.cancelled-->steps.uuidgen.deploy_failed
steps.kubeconfig.starting-->steps.kubeconfig.running
steps.kubeconfig.starting-->steps.kubeconfig.crashed
steps.metadata.outputs.success-->outputs.success
```
