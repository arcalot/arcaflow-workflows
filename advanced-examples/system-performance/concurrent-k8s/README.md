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

Install Python, at least `3.9`.

Add the path to your Python interpreter to `config.yaml` as the value for `pythonPath` as shown here.

```yaml
deployers:
  python:
    pythonPath: ...
```

Download a Go binary of the latest version of the Arcaflow engine from: https://github.com/arcalot/arcaflow-engine/releases
 
Run the workflow:
```
$ export WFPATH=<path to this workflow directory>
$ arcaflow -input ${WFPATH}/input.yaml -config ${WFPATH}/config.yaml -context ${WFPATH}
```

## Workflow Diagram
This diagram shows the complete end-to-end workflow logic.