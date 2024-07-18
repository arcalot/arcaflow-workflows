# Binding Repeated Values

## Workflow Description

This workflow demonstrates how to use the Arcaflow builtin function `bindConstants()` to repeat a set of input values with each item in your input list using a foreach loop.

### Subworkflow Description

The subworkflow uses a plugin to print the value of the `name` input.  The `loop_id` and `ratio` input values are bundled with this plugin output as this subworkflow's output. The template plugin is used to simulate an operation on input data and produce an output. In a real world scenario, the subworkflow will likely use more complex plugins with more complex schemas.

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema and the steps to execute.
  and their data relationships, and the output to present to the user
- [`input.yaml`](input.yaml) -- The input parameters that the user provides for running
  the workflow
- [`config.yaml`](config.yaml) -- Global config parameters that are passed to the Arcaflow
  engine
- [`subworkflow.yaml](subworkflow.yaml) -- Defines the subworkflow input schema and the plugins to execute.
                     
## Running the Workflow

### Workflow Execution

Download a Go binary of the latest version of the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```
$ export WFPATH=<path to this workflow directory>
$ arcaflow --context ${WFPATH} --input input.yaml --config config.yaml
```

## Workflow Diagram 

### Parent workflow

```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
input-->steps.foreach_loop.execute
steps.foreach_loop.execute-->steps.foreach_loop.outputs
steps.foreach_loop.outputs-->steps.foreach_loop.outputs.success
steps.foreach_loop.outputs.success-->outputs.success
```
### Sub-Workflow
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
input-->steps.example.starting
input-->outputs.success
steps.example.running-->steps.example.outputs
steps.example.outputs.success-->outputs.success
steps.example.outputs-->steps.example.outputs.success
steps.example.deploy-->steps.example.starting
steps.example.enabling-->steps.example.enabling.resolved
steps.example.enabling-->steps.example.starting
steps.example.enabling-->steps.example.disabled
steps.example.cancelled-->steps.example.outputs
steps.example.starting-->steps.example.running
steps.example.starting-->steps.example.starting.started
steps.example.disabled-->steps.example.disabled.output
```