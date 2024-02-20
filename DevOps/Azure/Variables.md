For each pipeline you can define variables after `pool` in
```
variables:
- name: Variable1
  value: Value1
```

You can enter secret variables in the "Variables" button top right.

You can create a group from the left menu under Pipelines > Library > New Variable Group and reference that with
```
- group: VariableGroupName
```

This group can be connected to an Azure key vault too.