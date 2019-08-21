UFEP#4 The extended for-each
============================

**Status:** Proposed

Abstract
--------

The proposal indroduces a new features for the for-each component definition,
such as prepended components, appended components, separator components, and so
on.

Rationale
---------

UFL is not a general purpose language. It never was and it never will. The only
purpose of UFL is to define logic of the UML .FRI metamodel. In this situation
the only result UFL can have is some (most of the time composite) object structure.

Whole syntax of UFL is focused on this principle. We can introduce some parts
to the language, that can help building composite object structure, even if they
would not have any reason to exist in other languages.

The extended for-each is that kind of feature. It introduces possibility to define
multiple sections into the for-each loop component to be included before, after,
between or instead of the loop iterations.

Syntax
------

The for-each componets would get a new syntax:

```xml
<ForEach collection="source-collection" item="item-variable">
    <Before>
        components, that should be included right before the loop iterations.
    </Before>
    <Loop>
        original loop body
    </Loop>
    <Separator>
        components, that should be included between loop iterations.
    </Separator>
    <After>
        components, that should be included right after the loop iterations.
    </After>
    <Else>
        Components, that should be included, when the source collection is empty.
    </Else>
</ForEach>
```

All sections but the `Loop` section are optional. When only the loop section
is present, the simpler syntax can be used:

```xml
<ForEach collection="source-collection" item="item-variable">
    original loop body
</ForEach>
```

The simpler syntax is the same as the current one, so the backward compatibility
of the syntax would be preserved.

Components in the `Before` and `After` sections would be included right before
iterating the first item, or after iterating the last item of the collection.
If the collection is empty, those sections would not be included.

Components in the `Separator` section would be included between iteration of two
items from the collection. If the collection has lower then two items, this
section is skipped too.

If an author want to include some components in a case of empty collection, no
conditions are needed. They can use new `Else` body, that would be included for
an empty collections.

Current alternative
-------------------

The proposal introduces only a new syntax for something, that can be achieved
by a current means too. Whole extended for-each can be rewritten to something
like this:

```xml
<If condition="#source-collection->empty()">
    <Then>
        the new *Else* section
    </Then>
    <Else>
        the new *Before* section
        <ForEach collection="source-collection" item="item-variable">
            the new *Loop* section
            <If condition="#@item-variable.is_last">
                the new *Separator* section
            </If>
        </ForEach>
        the new *After* section
    </Else>
</If>
```

This pattern is common in a element appearance definition and will be
even more common when code generation macros will be introduced into
UML .FRI.

The sole purpose of the new syntax is to make reading/writing ufl easier for
a metamodel creator.

Example
-------

Generating new-style class header for python 2.x would be this easy using the
new extended for-each syntax.

```xml
<TextBox>
    class<Space />
    <Text value="#self.name" />
    <ForEach collection="#self.bases" item="base">
        <Before>
            (
        </Before>
        <Loop>
            <Text value="#base" />
        </Loop>
        <Separator>
            ,
        </Separator>
        <After>
            )
        </After>
        <Else>
            (object)
        </Else>
    </ForEach>
</TextBox>
```
