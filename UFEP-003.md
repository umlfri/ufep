UFEP#3 Enhanced Text Components
===============================

Abstract
--------

The UFEP proposes new ufl components for text building.

Rationale
---------

If the ufl text components should be used to automatically generate source
code from a model, they need power up. Simplified text components, line
endings, whitespace and indentation support is the absolute minimum.

Simplified text component
-------------------------

Basic ufl text component is `Text`. It is used so many times just to
add text constant to a resulting string.

Take the following source as an example:

```xml
<TextBox>
    <Text text="def " />
    <Text text="#method.name" />
    <Text text="(self" />
    <ForEach collection="#method.parameters" item="param">
        <Text text="#param.name" />
        <If condition="#!@param.is_last">
            <Text text=", " />
        </If>
    </ForEach>
    <Text text="):" />
</TextBox>
``` 

Only two text components contains dynamic expression, almost all of them
are constants. Nicer and more elegant way to write the same code would be
using xml mixed content as in following example:

```xml
<TextBox>
    def <Text text="#method.name" />(self
    <ForEach collection="#method.parameters" item="param">
        <Text text="#param.name" />
        <If condition="#!@param.is_last">
            ,
        </If>
    </ForEach>
    ):
</TextBox>
```

This possibility was not yet implemented in UML .FRI but it would simplify
text components code in the future.

All text literals should be stripped from leading and tailing whitespaces
to make structuring the xml document possible. Multiple following whitespaces
should be collapsed as they are in html.

White space components
----------------------

For a simpler writing more complex text components code, separate
components for whitespaces are needed, as writing whitespaces are easy
to forget when writing or miss when reading the code.

At least two new components are needed:

- `NewLine` component for breaking a line.
  It could be a synonym for a simple text component:
  
  ```xml
  <Text text="&#13;&#10;" />
  ```
  
  Line ending character sequence used by the component can depend
  on the platform.
- `Space` component for adding a space.
  It could be a synonym for a simple text component too:
  
  ```xml
  <Text text=" " />
  ```

Indent component
----------------

When producing multiline text output such as source code in some
language, indenting each line in a block by some number of spaces or tabs
would by very helpful. Indentation size could be set to 4 spaces
by default for example, but it has to be customizable.

```xml
<TextBlock>
    Unindented:<NewLine />
    <Indent size="1" type="tab">
        - Hello<NewLine />
        - World<NewLine />
    </Indent>
    Unindented again.
</TextBlock>
```

Inner `Indent` component should inherit its type and size from the parent
one if not defined to be other.

`Indent` component has to introduce new line if no line break was added
right before it.

Complex example
---------------

This `TextBlock` will generate python skeleton of a clas for the Python
language.

```xml
<TextBox>
    class<Space />
    <Text text="#self.name" />
    :
    <Indent>
        <ForEach collection="#self.methods" item="method">
            def<Space />
            <Text text="#method.name" />
            (self
            <ForEach collection="#method.parameters" item="param">
                <Text text="#param.name" />
                <If condition="#!@param.is_last">
                    ,
                </If>
            </ForEach>
            ):
            <Indent>
                pass
            </Indent>
        </ForEach>
    </Indent>
</TextBox>
```
