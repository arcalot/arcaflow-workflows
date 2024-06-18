# Basic Workflow Examples
- [Basic](basic) - Runs a single plugin step
- [Parallel](parallel) - Demonstrates the default parallel step behavior
- [Serial Data Passing](serial-data_passing) - Demonstrates implicit step parallelization through data passing between steps
- [Serial Wait-For](serial-wait_for) - Demonstrates explicit step parallelization through the `wait_for` operative
- [Quoting](quoting) - Demonstrates the expression language using example quote formats
- [Namespaced Scopes](namespaced-scopes) -  Demonstrates the use of a namespaced scope in order to infer the workflow input schema from the plugin step
- [Sub-Workflow For-Each](sub-workflow-foreach) - Demonstrates iterating over a sub-workflow in a for-each loop
- [Binding Repeated Values](bind-constants) - Demonstrates the use of the builtin function `bindConstants()` to repeat a set of input values