# Expressions

## Workflow Description

This workflow demonstrates example expressions as detailed in our [expression documentation](https://arcalot.io/arcaflow/workflows/expressions/).

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
### Output

Each input expression to the `name` parameter outputs as the same string.

```shell
output_data:
  message:
    data:
      - name: Hello, Here's an apostrophe and "embedded quotes".!
      - name: Hello, Here's an apostrophe and "embedded quotes".!
      - name: Hello, Here's an apostrophe and "embedded quotes".!
      - name: Hello, Here's an apostrophe and "embedded quotes".!
      - name: Hello, Here's an apostrophe and "embedded quotes".!
      - name: Hello, Here's an apostrophe and "embedded quotes".!
output_id: success

```