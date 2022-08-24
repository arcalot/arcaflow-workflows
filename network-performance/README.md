**Key:**
```mermaid
graph TD
    schema[schema parameters]
    engine(engine tasks)
    plugin([plugin tasks])
```

## Workflow and schema breakdown
Global parameters define metadata items that will persist through the entire workflow, as well as flags for whether specific workflow features, and therefore plugins, will be used.

```yaml
global_params:
    es_server: str
    metadata_collection: bool
    metadata_targeted: bool
    system_metrics_collection: bool
    uuid: str
    cluster_name: str
    prom_token: str
```
```mermaid
graph TD
    global_params[global params] --> metrics_plugin([SUT metrics collection])
    global_params --> metadata_plugin([SUT metadata collection])
    global_params --> pod_config(k8s pod/job config)
```

SUT parameters affect k8s settings for pods and potentially other objects.
```yaml
sut_params:
    network_policy: bool
    multi_az: bool
    hostnetwork: bool
    serviceip: bool
    test_timeout: int
    run_id: str
    servicetype: str
    metallb:
        addresspool: str
        service_etp: str 
    multus:
        enabled: bool
```
```mermaid
graph TD
    sut_params[SUT params] --> net_config(pod network config) --> pod_config(k8s pod/job config)
```

The parameters for the uperf workloads should be available as defaults that will apply to all listed workloads unless overridden.
```yaml
uperf_workloads:
    defaults:
        samples: int
        pairs: ListType[int]
        nthrs: ListType[int]
        protos:  ListType[enum]
        test_types:  ListType[enum]
        sizes:  ListType[int]
        runtime: int
```

Individual workloads should be provided as lists of dicts, where parameters override the defaults.
```yaml
uperf_workloads:
    workload: ListType[dict]
        - <dict of params>  <-- This needs to accept all params from the defaults as optional
        - ...
```
```mermaid
graph TD
    workloads_list[workloads list] --> test_g(workloads test graph) --> scheduling(k8s scheduling)
```

## Complete workflow diagram
```mermaid
graph TD
    global_params[global params] --> metrics_plugin
    global_params --> metadata_plugin
    global_params --> pod_config

    sut_params[SUT params] --> net_config

    workloads_list[workloads list] --> test_g
    
    test_g(workloads test graph) --> scheduling

    net_config(pod network config) --> pod_config
    
    metrics_plugin([SUT metrics collection]) --> es
    metadata_plugin([SUT metadata collection]) --> es
    
    pod_config(k8s pod/job config) --> scheduling
    scheduling(k8s scheduling) --> uperf_servers
    scheduling --> uperf_clients

    uperf_servers([uperf servers])
    uperf_clients([uperf clients]) --> es
    es([elasticsearch])
```
