# Basic Workflow

## Workflow Description

This workflow simply runs a single step of an example plugin via the default deployer (defined in `config.yaml` as Docker) and reports its success output.

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
steps.example.outputs.success-->outputs.success
steps.example.deploy_failed-->steps.example.deploy_failed.error
steps.example.deploy-->steps.example.starting
steps.example.deploy-->steps.example.deploy_failed
steps.example.cancelled-->steps.example.crashed
steps.example.cancelled-->steps.example.deploy_failed
steps.example.cancelled-->steps.example.outputs
steps.example.outputs-->steps.example.outputs.success
steps.example.outputs-->steps.example.outputs.error
steps.example.crashed-->steps.example.crashed.error
steps.example.starting-->steps.example.crashed
steps.example.starting-->steps.example.running
steps.example.running-->steps.example.outputs
steps.example.running-->steps.example.crashed
```
