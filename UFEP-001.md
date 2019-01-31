UFEP#1 The type switch
======================

Abstract
--------

This document describes new type of ufl switch component usage for comparing
value types instead of values.

Rationale
---------

Current ufl switch component supports only simple value comparation. Sometimes,
metamodel creator needs to check for value type instead.

Traversing the model element tree might be taken as an example of such situation.
Each element can have other type and as that there is no way to access an element
data.

Syntax proposal
---------------

```xml
<Switch value="checked-object">
    <Case type="type1" into="varname">
        children, when the object has the first type.
    </Case>
    <Case type="type2" into="varname">
        children, when the object has the second type.
    </Case>
    ...
    <Default>
        default children.
    </Default>
</Switch>
```

The `Switch` element in the type switch definition should have the same syntax,
as the value switch. The difference is in the `Case` element. There are two
attributes `type` and `into` instead of just one `value` attribute. The `into`
attribute is not required.

Combining the value switch and the type switch into one `Switch` element should
not be possible. 

Execution
---------

Types will be checked from the first case on. All types should be tested for
`UflType::is_valid_value`. When the first type matches, the children of its case
will be included.
