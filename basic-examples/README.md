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
