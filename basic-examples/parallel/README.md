# Parallel Workflow

## Workflow Description

By default, multiple steps in a workflow will be run in parallel if there is no data passing relationship between the steps. 

This workflow runs a metadata collection plugin step and an example plugin step in parallel. All steps are run via the default deployer (defined in `config.yaml` as podman) and their success outputs are reported.

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
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.metadata.starting-->steps.metadata.starting.started
steps.metadata.starting-->steps.metadata.running
steps.example.outputs-->steps.example.outputs.success
steps.example.cancelled-->steps.example.outputs
steps.example.deploy-->steps.example.starting
steps.metadata.running-->steps.metadata.outputs
steps.metadata.cancelled-->steps.metadata.outputs
steps.example.running-->steps.example.outputs
input-->steps.example.starting
steps.example.outputs.success-->outputs.success
steps.metadata.outputs-->steps.metadata.outputs.success
steps.example.starting-->steps.example.starting.started
steps.example.starting-->steps.example.running
steps.metadata.deploy-->steps.metadata.starting
```
