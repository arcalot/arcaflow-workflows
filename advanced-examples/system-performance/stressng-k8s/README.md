# System Performance Tests With stress-ng on Kubernetes

***NOTE: This is an example only and may not work out-of-the-box.***

## Workflow Description

This workflow runs a [stress-ng](https://github.com/ColinIanKing/stress-ng) workload plugin on a Kubernetes target system.

In addition to the stress-ng workload, the workflow collects system metrics with [Performance Co-pilot](https://pcp.io/), and collects system metadata using Ansible [gather facts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/gather_facts_module.html).

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to run
  and their data relationships, and the output to present to the user
- [`input.yaml`](input-example.yaml) -- The input parameters that the user provides for running
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
This diagram shows the complete end-to-end workflow logic.

```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
input-->steps.kubeconfig.starting
input-->steps.pcp.deploy
input-->steps.pcp.starting
input-->outputs.success
input-->steps.stressng.deploy
input-->steps.stressng.starting
input-->steps.metadata.deploy
steps.metadata.starting-->steps.metadata.running
steps.metadata.starting-->steps.metadata.starting.started
steps.pcp.running-->steps.pcp.outputs
steps.pcp.outputs.success-->outputs.success
steps.stressng.deploy-->steps.stressng.starting
steps.stressng.outputs-->steps.stressng.outputs.success
steps.stressng.outputs-->steps.pcp.cancelled
steps.stressng.outputs.success-->outputs.success
steps.pcp.deploy-->steps.pcp.starting
steps.stressng.running-->steps.stressng.outputs
steps.uuidgen.running-->steps.uuidgen.outputs
steps.uuidgen.starting-->steps.uuidgen.starting.started
steps.uuidgen.starting-->steps.uuidgen.running
steps.metadata.deploy-->steps.metadata.starting
steps.metadata.cancelled-->steps.metadata.outputs
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.kubeconfig.deploy-->steps.kubeconfig.starting
steps.metadata.outputs.success-->outputs.success
steps.stressng.cancelled-->steps.stressng.outputs
steps.kubeconfig.outputs.success-->steps.metadata.deploy
steps.kubeconfig.outputs.success-->steps.pcp.deploy
steps.kubeconfig.outputs.success-->steps.stressng.deploy
steps.pcp.outputs-->steps.pcp.outputs.success
steps.kubeconfig.running-->steps.kubeconfig.outputs
steps.pcp.cancelled-->steps.pcp.outputs
steps.metadata.running-->steps.metadata.outputs
steps.metadata.outputs-->steps.metadata.outputs.success
steps.kubeconfig.outputs-->steps.kubeconfig.outputs.success
steps.pcp.starting-->steps.pcp.starting.started
steps.pcp.starting-->steps.pcp.running
steps.stressng.starting-->steps.stressng.running
steps.stressng.starting-->steps.stressng.starting.started
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.uuidgen.outputs.success-->outputs.success
steps.kubeconfig.cancelled-->steps.kubeconfig.outputs
steps.kubeconfig.starting-->steps.kubeconfig.starting.started
steps.kubeconfig.starting-->steps.kubeconfig.running
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.pcp.starting.started-->steps.stressng.starting
```
