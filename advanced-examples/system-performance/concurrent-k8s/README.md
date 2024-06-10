# OpenShift Shenanigans

***NOTE: This is an example only and may not work out-of-the-box.***

## Workflow Description

Given a target OpenShift cluster, this workflow executes a [kube-burner plugin](https://github.com/redhat-performance/arcaflow-plugin-kube-burner) workflow to place a load on the cluster, repeatedly removes a targeted pod at a given time frequency with the [kill-pod plugin](https://github.com/krkn-chaos/arcaflow-plugin-kill-pod), and runs a [stress-ng](https://github.com/ColinIanKing/stress-ng) CPU workload on the cluster.


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