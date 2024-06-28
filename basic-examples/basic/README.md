# Basic Workflow

## Workflow Description

This workflow simply runs a single step of an example plugin and reports its success output.

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to run
  and their data relationships, and the output to present to the user
- [`input.yaml`](input.yaml) -- The input parameters that the user provides for running
  the workflow
                     
## Running the Workflow

Download the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```bash
arcaflow --input input.yaml 
```

Example output:
```yaml
output_data:
    example:
        message: Hello, Arcalot!
output_id: success
```

## Workflow Diagram
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.example.running-->steps.example.outputs
steps.example.outputs.success-->outputs.success
steps.example.enabling-->steps.example.starting
steps.example.enabling-->steps.example.disabled
steps.example.enabling-->steps.example.enabling.resolved
steps.example.outputs-->steps.example.outputs.success
steps.example.cancelled-->steps.example.outputs
steps.example.disabled-->steps.example.disabled.output
input-->steps.example.starting
steps.example.deploy-->steps.example.starting
steps.example.starting-->steps.example.starting.started
steps.example.starting-->steps.example.running
```
