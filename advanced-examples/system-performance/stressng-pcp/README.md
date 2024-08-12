# Stress-NG + PCP Workflow

## Workflow Description

The [primary workflow](workflow.yaml) is designed to collect system metadata using the
Arcaflow [metadata plugin](https://github.com/arcalot/arcaflow-plugin-metadata), and
then to run loops of a [stress-ng
plugin](https://github.com/arcalot/arcaflow-plugin-stressng) sub-workflow with various
parameters.

In addition to the stress-ng workload, the sub-workflow collects system metrics with
[Performance Co-pilot](https://pcp.io/) using the Arcaflow
[pcp plugin](https://github.com/arcalot/arcaflow-plugin-pcp).

This is a complex workflow that exercises many Arcaflow features, including
[sub-workflows](/basic-examples/sub-workflow-foreach),
[serial wait-for](/basic-examples/serial-wait_for),
[namespaced scopes](/basic-examples/namespaced-scopes),
and [bind constants](/basic-examples/bind-constants).

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the outer workflow input schema, the
plugins and sub-workflow to run and their data relationships, and the output to present
to the user
- [`stressng-workflow.yaml`](stressng-workflow.yaml) -- Defines the inner workflow input
schema, plugins, and output for stress-ng tests. This workflow is looped over by the
outer workflow, but it can also be used stand-alone
- [`input.yaml`](sample-input.yaml) -- Example input parameters that the user provides
for running the outer workflow
- [`sample-input-stressng-cpu.yaml`](sample-input-stressng-cpu.yaml) -- Example CPU test
input parameters that can be used to run the `stressng-workflow.yaml` workflow as a
stand-alone workflow
- [`sample-input-stressng-memory.yaml`](sample-input-stressng-memory.yaml) -- Example
memory test input parameters that can be used to run the `stressng-workflow.yaml`
workflow as a stand-alone workflow
                     
## Running the Workflow

## Running the Workflow

Download the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```bash
arcaflow --input input.yaml 
```

Run the sub-workflow as a stand-alone workflow:
```bash
arcaflow --input sample-input-stressng-cpu.yaml --workflow stressng-workflow.yaml
```

## Workflow Diagrams    
These diagrams show the end-to-end workflow success-path logic.

### Parent Workflow
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.end_timestamp.starting-->steps.end_timestamp.running
steps.end_timestamp.starting-->steps.end_timestamp.starting.started
steps.stressng_loop.execute-->steps.stressng_loop.outputs
steps.start_timestamp.enabling-->steps.start_timestamp.enabling.resolved
steps.start_timestamp.enabling-->steps.start_timestamp.starting
steps.start_timestamp.enabling-->steps.start_timestamp.disabled
steps.uuidgen.running-->steps.uuidgen.outputs
input-->steps.stressng_loop.execute
input-->outputs.success
steps.start_timestamp.outputs.success-->outputs.success
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.uuidgen.outputs.success-->outputs.success
steps.end_timestamp.running-->steps.end_timestamp.outputs
steps.uuidgen.enabling-->steps.uuidgen.enabling.resolved
steps.uuidgen.enabling-->steps.uuidgen.starting
steps.uuidgen.enabling-->steps.uuidgen.disabled
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.stressng_loop.outputs-->steps.stressng_loop.outputs.success
steps.stressng_loop.outputs-->steps.end_timestamp.starting
steps.uuidgen.starting-->steps.uuidgen.starting.started
steps.uuidgen.starting-->steps.uuidgen.running
steps.ansible_facts.outputs.success-->outputs.success
steps.ansible_facts.cancelled-->steps.ansible_facts.outputs
steps.start_timestamp.running-->steps.start_timestamp.outputs
steps.ansible_facts.outputs-->steps.ansible_facts.outputs.success
steps.end_timestamp.outputs-->steps.end_timestamp.outputs.success
steps.start_timestamp.deploy-->steps.start_timestamp.starting
steps.end_timestamp.deploy-->steps.end_timestamp.starting
steps.ansible_facts.deploy-->steps.ansible_facts.starting
steps.start_timestamp.disabled-->steps.start_timestamp.disabled.output
steps.end_timestamp.disabled-->steps.end_timestamp.disabled.output
steps.end_timestamp.cancelled-->steps.end_timestamp.outputs
steps.uuidgen.disabled-->steps.uuidgen.disabled.output
steps.start_timestamp.outputs-->steps.stressng_loop.execute
steps.start_timestamp.outputs-->steps.start_timestamp.outputs.success
steps.ansible_facts.running-->steps.ansible_facts.outputs
steps.end_timestamp.outputs.success-->outputs.success
steps.stressng_loop.outputs.success-->outputs.success
steps.start_timestamp.cancelled-->steps.start_timestamp.outputs
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.ansible_facts.disabled-->steps.ansible_facts.disabled.output
steps.start_timestamp.starting-->steps.start_timestamp.starting.started
steps.start_timestamp.starting-->steps.start_timestamp.running
steps.end_timestamp.enabling-->steps.end_timestamp.starting
steps.end_timestamp.enabling-->steps.end_timestamp.disabled
steps.end_timestamp.enabling-->steps.end_timestamp.enabling.resolved
steps.ansible_facts.enabling-->steps.ansible_facts.enabling.resolved
steps.ansible_facts.enabling-->steps.ansible_facts.starting
steps.ansible_facts.enabling-->steps.ansible_facts.disabled
steps.ansible_facts.starting-->steps.ansible_facts.running
steps.ansible_facts.starting-->steps.ansible_facts.starting.started
```

### Sub-Workflow
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.pcp.starting.started-->steps.pre_wait.starting
steps.pre_wait.cancelled-->steps.pre_wait.outputs
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.pre_wait.outputs-->steps.pre_wait.outputs.success
steps.pre_wait.outputs-->steps.stressng.starting
steps.uuidgen.disabled-->steps.uuidgen.disabled.output
steps.uuidgen.starting-->steps.uuidgen.starting.started
steps.uuidgen.starting-->steps.uuidgen.running
steps.pcp.cancelled-->steps.pcp.outputs
steps.post_wait.enabling-->steps.post_wait.enabling.resolved
steps.post_wait.enabling-->steps.post_wait.starting
steps.post_wait.enabling-->steps.post_wait.disabled
steps.stressng.starting-->steps.stressng.starting.started
steps.stressng.starting-->steps.stressng.running
steps.stressng.disabled-->steps.stressng.disabled.output
steps.pre_wait.deploy-->steps.pre_wait.starting
steps.pre_wait.starting-->steps.pre_wait.starting.started
steps.pre_wait.starting-->steps.pre_wait.running
steps.pre_wait.disabled-->steps.pre_wait.disabled.output
steps.stressng.cancelled-->steps.stressng.outputs
steps.post_wait.outputs-->steps.post_wait.outputs.success
steps.post_wait.outputs-->steps.pcp.cancelled
steps.uuidgen.running-->steps.uuidgen.outputs
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.pcp.outputs-->steps.pcp.outputs.success
steps.post_wait.cancelled-->steps.post_wait.outputs
steps.uuidgen.outputs.success-->outputs.success
steps.pcp.enabling-->steps.pcp.starting
steps.pcp.enabling-->steps.pcp.disabled
steps.pcp.enabling-->steps.pcp.enabling.resolved
steps.stressng.outputs.success-->outputs.success
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.stressng.enabling-->steps.stressng.disabled
steps.stressng.enabling-->steps.stressng.enabling.resolved
steps.stressng.enabling-->steps.stressng.starting
steps.pcp.starting-->steps.pcp.starting.started
steps.pcp.starting-->steps.pcp.running
steps.post_wait.running-->steps.post_wait.outputs
steps.pre_wait.running-->steps.pre_wait.outputs
steps.pre_wait.enabling-->steps.pre_wait.enabling.resolved
steps.pre_wait.enabling-->steps.pre_wait.starting
steps.pre_wait.enabling-->steps.pre_wait.disabled
steps.post_wait.starting-->steps.post_wait.running
steps.post_wait.starting-->steps.post_wait.starting.started
steps.pcp.running-->steps.pcp.outputs
steps.pcp.deploy-->steps.pcp.starting
steps.stressng.outputs-->steps.post_wait.starting
steps.stressng.outputs-->steps.stressng.outputs.success
steps.stressng.deploy-->steps.stressng.starting
input-->steps.pcp.starting
input-->steps.stressng.starting
steps.pcp.outputs.success-->outputs.success
steps.stressng.running-->steps.stressng.outputs
steps.post_wait.disabled-->steps.post_wait.disabled.output
steps.uuidgen.enabling-->steps.uuidgen.starting
steps.uuidgen.enabling-->steps.uuidgen.disabled
steps.uuidgen.enabling-->steps.uuidgen.enabling.resolved
steps.pcp.disabled-->steps.pcp.disabled.output
steps.post_wait.deploy-->steps.post_wait.starting
```