# Example Foreach Loop Workflow

***NOTE: This is an example only and may not work out-of-the-box.***

## Workflow Description

This example workflow runs a series of [sysbench](https://github.com/akopytov/sysbench) CPU workloads on the local system.

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the parent workflow input schema and the plugin steps to run, including the sub-workflow
- [`sub-workflow.yaml`](sub-workflow.yaml) -- Defines the sub-workflow input schema and the plugin steps to run
- [`input.yaml`](input-example.yaml) -- The input parameters that the user provides for running
  the workflow
- [`config.yaml`](config.yaml) -- Global config parameters that are passed to the Arcaflow
  engine
                     
## Running the Workflow

### Workflow Execution

You will need a Golang runtime and Docker or Podman to run the containers.

Download the engine to your `$PATH` and ensure it is executable: https://github.com/arcalot/arcaflow-engine/releases

Clone this workflows repo, and set this directory to your workflow working directory (adjust as needed):
```
$ git clone https://github.com/arcalot/arcaflow-workflows.git
$ export WFPATH=$(pwd)/arcaflow-workflows/example-foreach-loop
```
 
Run the workflow:
```
$ arcaflow -input ${WFPATH}/input-example.yaml \
-config ${WFPATH}/config.yaml -context ${WFPATH}