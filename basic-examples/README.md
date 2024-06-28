# Basic Workflow Examples
- [Basic](basic) - Runs a single plugin step
- [Config File](config-file) - Demonstrates the use of an Arcaflow config file
- [Parallel](parallel) - Demonstrates the default parallel step behavior
- [Serial Data Passing](serial-data_passing) - Demonstrates implicit step serialization
through data passing between steps
- [Serial Wait-For](serial-wait_for) - Demonstrates explicit step serialization through
the `wait_for` operative
- [Quoting](quoting) - Demonstrates various syntax for quoting strings in expressions
- [Namespaced Scopes](namespaced-scopes) - Demonstrates the use of a namespaced scope in
order to refer to the workflow input schema from the plugin step
- [Sub-Workflow For-Each](sub-workflow-foreach) - Demonstrates iterating over a list of
input items, executing the sub-workflow for each one
- [Binding Repeated Values](bind-constants) - Demonstrates the use of the builtin
function `bindConstants()` to repeat a set of input values with each item in your input
list using a foreach loop

> [!NOTE]
> The Arcaflow engine executes the steps of a workflow via *deployers* for one or more
> container platorms, such as
> [Podman](https://github.com/arcalot/arcaflow-engine-deployer-podman),
> [Docker](https://github.com/arcalot/arcaflow-engine-deployer-docker),
> or [Kubernetes](https://github.com/arcalot/arcaflow-engine-deployer-kubernetes).
> Arcaflow's default deployer is Podman. To override this default, a
> [configuration file](https://arcalot.io/arcaflow/running/setup/#configuration) must be
> used with the `arcaflow` command. A basic [Docker config file](/docker-config.yaml) is
> included in this repo's root directory and can be used with any of these example
> workflows via the `--config` parameter.
