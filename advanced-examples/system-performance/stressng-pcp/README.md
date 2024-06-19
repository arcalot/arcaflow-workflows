# Stress-NG + PCP Workflow

## Workflow Description

The primary [`workflow.yaml`](workflow.yaml) is designed to collect system metadata with
Ansible [gather facts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/gather_facts_module.html)
using the Arcaflow
[metadata plugin](https://github.com/arcalot/arcaflow-plugin-metadata), and then to run
loops of a stress-ng sub-workflow with various parameters.

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
`workflow.yaml`, but it can also be used stand-alone
- [`input.yaml`](sample-input.yaml) -- Example input parameters that the user provides
for running the outer workflow
- [`sample-input-stressng-cpu.yaml`](sample-input-stressng-cpu.yaml) -- Example CPU test
input parameters that can be used to run the `stressng-workflow.yaml` workflow as a
stand-alone workflow
- [`sample-input-stressng-memory.yaml`](sample-input-stressng-memory.yaml) -- Example
memory test input parameters that can be used to run the `stressng-workflow.yaml`
workflow as a stand-alone workflow
- [`config.yaml`](config.yaml) -- Global config parameters that are passed to the
Arcaflow engine
                     
## Running the Workflow

### Workflow Execution

Download the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```
$ arcaflow --context <workflow directory> --input input.yaml --config config.yaml
```

Run the sub-workflow as a stand-alone workflow:
```
$ arcaflow --context ${WFPATH} --input input-stressng-cpu.yaml --workflow stressng-workflow.yaml --config config.yaml
```

## Workflow Diagrams    
These diagrams show the end-to-end workflow success-path logic.

### Parent Workflow
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.uuidgen.outputs.success-->outputs.success
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.stressng_loop.outputs-->steps.stressng_loop.outputs.success
steps.ansible_facts.starting-->steps.ansible_facts.starting.started
steps.ansible_facts.starting-->steps.ansible_facts.running
steps.stressng_loop.outputs.success-->outputs.success
steps.stressng_loop.outputs.success-->steps.end_timestamp.starting
steps.stressng_loop.execute-->steps.stressng_loop.outputs
input-->steps.stressng_loop.execute
input-->outputs.success
steps.start_timestamp.disabled-->steps.start_timestamp.disabled.output
steps.end_timestamp.deploy-->steps.end_timestamp.starting
steps.end_timestamp.disabled-->steps.end_timestamp.disabled.output
steps.end_timestamp.starting-->steps.end_timestamp.starting.started
steps.end_timestamp.starting-->steps.end_timestamp.running
steps.end_timestamp.outputs.success-->outputs.success
steps.ansible_facts.running-->steps.ansible_facts.outputs
steps.ansible_facts.enabling-->steps.ansible_facts.enabling.resolved
steps.ansible_facts.enabling-->steps.ansible_facts.starting
steps.ansible_facts.enabling-->steps.ansible_facts.disabled
steps.end_timestamp.cancelled-->steps.end_timestamp.outputs
steps.end_timestamp.running-->steps.end_timestamp.outputs
steps.start_timestamp.enabling-->steps.start_timestamp.disabled
steps.start_timestamp.enabling-->steps.start_timestamp.enabling.resolved
steps.start_timestamp.enabling-->steps.start_timestamp.starting
steps.ansible_facts.disabled-->steps.ansible_facts.disabled.output
steps.start_timestamp.starting-->steps.start_timestamp.starting.started
steps.start_timestamp.starting-->steps.start_timestamp.running
steps.uuidgen.starting-->steps.uuidgen.starting.started
steps.uuidgen.starting-->steps.uuidgen.running
steps.uuidgen.disabled-->steps.uuidgen.disabled.output
steps.ansible_facts.deploy-->steps.ansible_facts.starting
steps.start_timestamp.cancelled-->steps.start_timestamp.outputs
steps.start_timestamp.outputs.success-->steps.stressng_loop.execute
steps.start_timestamp.outputs.success-->outputs.success
steps.ansible_facts.cancelled-->steps.ansible_facts.outputs
steps.ansible_facts.outputs-->steps.ansible_facts.outputs.success
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.end_timestamp.outputs-->steps.end_timestamp.outputs.success
steps.start_timestamp.deploy-->steps.start_timestamp.starting
steps.start_timestamp.running-->steps.start_timestamp.outputs
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.end_timestamp.enabling-->steps.end_timestamp.enabling.resolved
steps.end_timestamp.enabling-->steps.end_timestamp.starting
steps.end_timestamp.enabling-->steps.end_timestamp.disabled
steps.uuidgen.enabling-->steps.uuidgen.disabled
steps.uuidgen.enabling-->steps.uuidgen.enabling.resolved
steps.uuidgen.enabling-->steps.uuidgen.starting
steps.start_timestamp.outputs-->steps.start_timestamp.outputs.success
steps.ansible_facts.outputs.success-->outputs.success
steps.uuidgen.running-->steps.uuidgen.outputs
```

### Sub-Workflow
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.pre_wait.outputs-->steps.pre_wait.outputs.success
steps.uuidgen.disabled-->steps.uuidgen.disabled.output
steps.uuidgen.cancelled-->steps.uuidgen.outputs
steps.post_wait.disabled-->steps.post_wait.disabled.output
steps.stressng.starting-->steps.stressng.starting.started
steps.stressng.starting-->steps.stressng.running
steps.pre_wait.running-->steps.pre_wait.outputs
steps.pcp.starting.started-->steps.pre_wait.starting
steps.pre_wait.cancelled-->steps.pre_wait.outputs
steps.pre_wait.disabled-->steps.pre_wait.disabled.output
steps.pre_wait.outputs.success-->steps.stressng.starting
steps.stressng.running-->steps.stressng.outputs
steps.pcp.starting-->steps.pcp.starting.started
steps.pcp.starting-->steps.pcp.running
steps.uuidgen.running-->steps.uuidgen.outputs
input-->steps.pcp.starting
input-->steps.stressng.starting
steps.pre_wait.enabling-->steps.pre_wait.disabled
steps.pre_wait.enabling-->steps.pre_wait.enabling.resolved
steps.pre_wait.enabling-->steps.pre_wait.starting
steps.pcp.disabled-->steps.pcp.disabled.output
steps.post_wait.deploy-->steps.post_wait.starting
steps.stressng.cancelled-->steps.stressng.outputs
steps.post_wait.enabling-->steps.post_wait.enabling.resolved
steps.post_wait.enabling-->steps.post_wait.starting
steps.post_wait.enabling-->steps.post_wait.disabled
steps.post_wait.outputs-->steps.post_wait.outputs.success
steps.pre_wait.starting-->steps.pre_wait.starting.started
steps.pre_wait.starting-->steps.pre_wait.running
steps.stressng.outputs-->steps.stressng.outputs.success
steps.post_wait.running-->steps.post_wait.outputs
steps.pcp.running-->steps.pcp.outputs
steps.pcp.deploy-->steps.pcp.starting
steps.pcp.cancelled-->steps.pcp.outputs
steps.uuidgen.deploy-->steps.uuidgen.starting
steps.stressng.enabling-->steps.stressng.enabling.resolved
steps.stressng.enabling-->steps.stressng.starting
steps.stressng.enabling-->steps.stressng.disabled
steps.pcp.outputs.success-->outputs.success
steps.post_wait.outputs.success-->steps.pcp.cancelled
steps.uuidgen.outputs.success-->outputs.success
steps.pre_wait.deploy-->steps.pre_wait.starting
steps.uuidgen.outputs-->steps.uuidgen.outputs.success
steps.uuidgen.starting-->steps.uuidgen.starting.started
steps.uuidgen.starting-->steps.uuidgen.running
steps.pcp.enabling-->steps.pcp.starting
steps.pcp.enabling-->steps.pcp.disabled
steps.pcp.enabling-->steps.pcp.enabling.resolved
steps.pcp.outputs-->steps.pcp.outputs.success
steps.stressng.outputs.success-->steps.post_wait.starting
steps.stressng.outputs.success-->outputs.success
steps.post_wait.starting-->steps.post_wait.running
steps.post_wait.starting-->steps.post_wait.starting.started
steps.stressng.deploy-->steps.stressng.starting
steps.post_wait.cancelled-->steps.post_wait.outputs
steps.uuidgen.enabling-->steps.uuidgen.disabled
steps.uuidgen.enabling-->steps.uuidgen.enabling.resolved
steps.uuidgen.enabling-->steps.uuidgen.starting
steps.stressng.disabled-->steps.stressng.disabled.output
```