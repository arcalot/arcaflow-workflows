# Serial Workflow

## Workflow Description

By default, multiple steps in a workflow will be run in parallel if there is no data passing relationship between the steps. In some cases, you may want to run steps in a serial manner when there is no such relationship between the steps. For this case, there is the `wait_for` option, which tells a step explicitly to wait for a condition from another step.

This workflow runs a metadata collection plugin step, and then an example plugin that waits for the success of the metadata collection before starting. All steps are run via the default deployer (defined in `config.yaml` as Docker) and their success outputs are reported.

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
steps.example.running-->steps.example.outputs
steps.example.running-->steps.example.crashed
steps.metadata.starting-->steps.metadata.running
steps.metadata.starting-->steps.metadata.crashed
steps.example.starting-->steps.example.running
steps.example.starting-->steps.example.crashed
steps.metadata.running-->steps.metadata.crashed
steps.metadata.running-->steps.metadata.outputs
steps.metadata.deploy-->steps.metadata.starting
steps.metadata.deploy-->steps.metadata.deploy_failed
steps.example.crashed-->steps.example.crashed.error
steps.metadata.deploy_failed-->steps.metadata.deploy_failed.error
steps.example.outputs-->steps.example.outputs.error
steps.example.outputs-->steps.example.outputs.success
steps.example.deploy-->steps.example.deploy_failed
steps.example.deploy-->steps.example.starting
input-->steps.example.starting
steps.metadata.outputs.success-->steps.example.starting
steps.metadata.outputs.success-->outputs.success
steps.metadata.cancelled-->steps.metadata.outputs
steps.metadata.cancelled-->steps.metadata.crashed
steps.metadata.cancelled-->steps.metadata.deploy_failed
steps.metadata.crashed-->steps.metadata.crashed.error
steps.example.deploy_failed-->steps.example.deploy_failed.error
steps.metadata.outputs-->steps.metadata.outputs.error
steps.metadata.outputs-->steps.metadata.outputs.success
steps.example.cancelled-->steps.example.outputs
steps.example.cancelled-->steps.example.crashed
steps.example.cancelled-->steps.example.deploy_failed

```
