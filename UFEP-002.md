UFEP#2 Tree traversal
=====================

Abstract
--------

This UFEP proposes the possibility to use restricted recursion
for object tree walking in the ufl.

Rationale
---------

There is only one recursive object structure in UML .FRI - the project
element tree. But sometimes it is worth having a possibility to
traverse it in the ufl. In the future, more recursive structures might
be added.

Syntax
------

```xml
<Walk into="tree-expression" node="variable-name">
    ...
    <Walk into="node-child-expression" />
    ...
</Walk>
```

`Walk` element is used in two different contexts. The first element represents
the whole recursivly executed script part. The `into` attribute contains
the tree that should be traversed. The `node` attribute contains name of the
variable to assign each node to.

The second `Walk` element represents the point, where the recursion happens.
The `into` attribute contains expression that is used to execute traversal
for.

Execution
---------

When the walk command is executed, a tree root node is assigned into the
node variable. Walk command content is executed then. When the inner walk
command is being executed, traversal continues on the given node. After
a node traversal is done, execution continues when it stopped before.

Node variable metadata
----------------------

TODO

Example
-------

TODO
