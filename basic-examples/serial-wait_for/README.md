# Explicit Serial Workflow

## Workflow Description

By default, multiple steps in a workflow will be run in parallel if there is no data
dependency between the steps. In some cases, you may want to run steps in a serial
manner when there is no such relationship between the steps. For this case, there is the
`wait_for` option, which tells a step explicitly to wait for a condition from another
step.

This workflow runs a metadata collection plugin step, and then an example plugin that
waits for the success of the metadata collection before starting. All steps are run via
the default deployer (defined in `config.yaml`), and their success outputs are reported.

## Files

- [`workflow.yaml`](workflow.yaml) -- Defines the workflow input schema, the plugins to
  run and their data relationships, and the output to present to the user
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
    metadata:
        architecture: aarch64
        env:
            _: /usr/bin/python
            AD_HOC_COMMAND_ID: "1"
            ANSIBLE_CACHE_PLUGIN: jsonfile
            ANSIBLE_CACHE_PLUGIN_CONNECTION: /tmp/artifacts/0c733cd4-fa58-4a46-9b72-b95c55435377/fact_cache
            ANSIBLE_CALLBACK_PLUGINS: /usr/local/lib/python3.9/site-packages/ansible_runner/display_callback/callback
            ANSIBLE_HOST_KEY_CHECKING: "False"
            ANSIBLE_LOAD_CALLBACK_PLUGINS: "1"
            ANSIBLE_RETRY_FILES_ENABLED: "False"
            ANSIBLE_STDOUT_CALLBACK: awx_display
            AWX_ISOLATED_DATA_DIR: /tmp/artifacts/0c733cd4-fa58-4a46-9b72-b95c55435377
            HOME: /root
            HOSTNAME: fe1ff9f0a11c
            LC_CTYPE: C.UTF-8
            PATH: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
            PWD: /tmp
            RUNNER_OMIT_EVENTS: "False"
            RUNNER_ONLY_FAILED_EVENTS: "False"
            SHLVL: "2"
            container: podman
        fqdn: fe1ff9f0a11c
        kernel: 6.8.10-400.asahi.fc40.aarch64+16k
        memtotal_mb: 31621
        processor:
            - "0"
            - "1"
            - "2"
            - "3"
            - "4"
            - "5"
            - "6"
            - "7"
            - "8"
            - "9"
            - "10"
            - "11"
        processor_cores: 1
        processor_count: 12
        processor_threads_per_core: 1
        product_name: NA
        product_version: NA
        swaptotal_mb: 8191
        system_vendor: NA
        uptime_seconds: 610189
output_id: success
```

## Workflow Diagram
```mermaid
%% Mermaid markdown workflow
flowchart LR
%% Success path
steps.example.enabling-->steps.example.enabling.resolved
steps.example.enabling-->steps.example.starting
steps.example.enabling-->steps.example.disabled
steps.metadata.cancelled-->steps.metadata.outputs
steps.example.starting-->steps.example.starting.started
steps.example.starting-->steps.example.running
steps.metadata.running-->steps.metadata.outputs
steps.example.deploy-->steps.example.starting
steps.metadata.outputs-->steps.metadata.outputs.success
steps.metadata.starting-->steps.metadata.starting.started
steps.metadata.starting-->steps.metadata.running
steps.example.disabled-->steps.example.disabled.output
steps.metadata.disabled-->steps.metadata.disabled.output
steps.metadata.outputs.success-->outputs.success
steps.metadata.outputs.success-->steps.example.starting
steps.example.running-->steps.example.outputs
steps.metadata.deploy-->steps.metadata.starting
steps.example.outputs.success-->outputs.success
steps.metadata.enabling-->steps.metadata.starting
steps.metadata.enabling-->steps.metadata.disabled
steps.metadata.enabling-->steps.metadata.enabling.resolved
steps.example.outputs-->steps.example.outputs.success
input-->steps.example.starting
steps.example.cancelled-->steps.example.outputs
```
