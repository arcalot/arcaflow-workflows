# Example Workflow

***NOTE: This is an example only and may not work out-of-the-box.***

## Workflow Description

This example workflow runs a [sysbench](https://github.com/akopytov/sysbench) CPU workload plugin on the local system.

In addition to the sysbench workload, the workflow collects system metrics with [Performance Co-pilot](https://pcp.io/), and collects system metadata using Ansible [gather facts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/gather_facts_module.html). Finally the outputs are all collected into a document and indexed to an [OpenSearch](https://opensearch.org/)-compatible service such as [Elasticsearch](https://www.elastic.co/).

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to run
  and their data relationships, and the output to present to the user
- [`input.yaml`](input-example.yaml) -- The input parameters that the user provides for running
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
This diagram shows the complete end-to-end workflow logic.

```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.sysbench.cancelled-->steps.sysbench.outputs
steps.sysbench.outputs-->steps.pcp.cancelled
steps.sysbench.outputs-->steps.sysbench.outputs.success
steps.opensearch.outputs-->steps.opensearch.outputs.success
input-->steps.pcp.starting
input-->steps.sysbench.starting
input-->steps.opensearch.starting
steps.sysbench.running-->steps.sysbench.outputs
steps.pcp.cancelled-->steps.pcp.outputs
steps.opensearch.outputs.success-->outputs.success
steps.opensearch.running-->steps.opensearch.outputs
steps.pcp.deploy-->steps.pcp.starting
steps.metadata.running-->steps.metadata.outputs
steps.opensearch.starting-->steps.opensearch.running
steps.opensearch.starting-->steps.opensearch.starting.started
steps.metadata.starting-->steps.metadata.starting.started
steps.metadata.starting-->steps.metadata.running
steps.sysbench.outputs.success-->outputs.no-indexing
steps.sysbench.outputs.success-->steps.opensearch.starting
steps.sysbench.outputs.success-->outputs.success
steps.pcp.outputs-->steps.pcp.outputs.success
steps.pcp.running-->steps.pcp.outputs
steps.opensearch.cancelled-->steps.opensearch.outputs
steps.pcp.starting-->steps.pcp.starting.started
steps.pcp.starting-->steps.pcp.running
steps.opensearch.deploy-->steps.opensearch.starting
steps.pcp.starting.started-->steps.sysbench.starting
steps.metadata.cancelled-->steps.metadata.outputs
steps.sysbench.deploy-->steps.sysbench.starting
steps.opensearch.outputs.error-->outputs.no-indexing
steps.metadata.deploy-->steps.metadata.starting
steps.sysbench.starting-->steps.sysbench.starting.started
steps.sysbench.starting-->steps.sysbench.running
steps.metadata.outputs-->steps.metadata.outputs.success
steps.pcp.outputs.success-->outputs.no-indexing
steps.pcp.outputs.success-->steps.opensearch.starting
steps.pcp.outputs.success-->outputs.success
steps.metadata.outputs.success-->steps.opensearch.starting
steps.metadata.outputs.success-->outputs.success
steps.metadata.outputs.success-->outputs.no-indexing
```
