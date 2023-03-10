# What the matrix is
A 3D world engine, with semantic textures applied to meshes, lights and sounds. 

![The Matrix](https://github.com/botbreeder/what-the-matrix-is/raw/main/bm.jpg)

If an AI is connected to a text-world, capable of showing plausible cause-consequence chains, the percepts we feed it with would be like textual descriptions floating dynamically in a 3D space. For visual and audio senses, a typical 3d game engine, running headless, could provide the space-time skeleton on which "semantic textures", made of qualitative/quantitive descriptions (potentially expressed in a subset of a natural language), could be applied to form this world.

The name of the game is still [Open City](https://github.com/botbreeder/open-city). Evolving.

## Percepts

This is literally about semantic information embedded in a 3D scene. Say you have a red sphere in front of you. Having an RGB value of 255,0,0 doesn’t carry the “redness” of the sphere, but having a description like “this is red” does. But that’s not all. The agent doesn’t receive an objective description of a 3D scene, but a view, as delivered by a “semantic camera”. If you’re looking towards north, and the sphere is located east, you’ll see it on your right.

It’s hard to imagine the input as received by the agent, because we humans are obviously not able to combine semantic (propositional) data with 3D data. The image above was an attempt to express what is otherwise unthinkable. So I’ll try to describe it better.

What I have in mind is not like [X3D](https://en.wikipedia.org/wiki/X3D) for instance. It’s not meant to be rendered visually. Instead, it is a description of the situation, embedded in a 3D space, and moving as time passes. Take a dog for instance. From far away, it could be described as a box, labelled “dog”. If you get closer, you see that inside that bounding box there are 4 cylinders for legs, and maybe a line for the tail, and a sphere for the head. These basic shapes contain more precise (smaller) shapes, along with semantic descriptions of what they are, and what are their relations to their parents’ shapes. There can be several relations on the same nodes, like `is-part-of` relations, ...etc. But I don’t want to give the impression that it’s a single hierarchy, because it’s not. It’s a graph rather than a tree.

A given node has a 3D shape, and is related to (is located in/contains) several other 3D shapes by relations. A relation-type is identified primarily by a natural language sentence with holes. Relations are themselves located in space-time, and after all, nodes are just like 0-arity relations. By default, relations spatially cover what they relate. Relations are in fact functions of what comes next: it's like a never satisfied spreadsheet.

Let's give a name to these nodes/relations. Let's call them [Narratives](https://github.com/botbreeder/mudbasic#narratives-as-reusable-blocks). Just replace the numbers by global `$names` and local `%names` from [OpenDDL](http://openddl.org/).

## Syntax

I like OpenDDL's way to emphasize the type of everything. Every value has to be written in its type-container, even simple things like `string{"foo"}`.

> Each unit of data in an OpenDDL file has an explicitly specified type, and this eliminates ambiguity and fragile inferencing methods that can impact the integrity of the data.

There's an exception though, in properties:

> The type of the property’s value must be specified by some external source of information such as a schema or the implementation of the derivative format.

### Building on it

#### Subarrays

The `[a b c]` syntax seems appropriate for subarrays.

```
VertexArray {

    float[3] {

        [1.0, 2.0, 3.0],
        [0.5, 0.0, 0.5],
        [0.0, -1.0, 4.0],
        [0.0, 1.0, -4.0]
    }
}
```

#### Tensors

We can also expand this syntax to several dimensions.

```

Tensor {

    float[3, 2] {

        [
            [1.0, 2.0],
            [3.0, 4.0],
            [5.0, 6.0]
        ],
        [
            [1.1, 2.1],
            [3.1, 4.1],
            [5.1, 6.1]
        ],
        [
            [1.2, 2.2],
            [3.2, 4.2],
            [5.2, 6.2]
        ],
        [
            [1.3, 2.3],
            [3.3, 4.3],
            [5.3, 6.3]
        ]
    }
}

```

#### Specific data types

What could be useful is the possibility to define **specific** data types, that is, a data type that is syntactically expressed as an existing type, but has another name. For example, an `EmailAddress` data type could be built from the primitive `string` data type. The specific data type puts additional constraints on an existing type, and makes its meaning explicit.

```
EmailAddress:string { "github@gmail.com" }
```

#### Primitive data types

That's about it, on the syntax level. For now. OpenDDL provides almost everything else. We stay very declarative.

The list of available primitive data types can be reduced to:
```
boolean         bool        b
number          num         n
string          str         s
reference       ref         r
type            type        t
word            word        w
```

where `word` can be an identifier or a name, including the initial `$` or `%` as needed.

## Computation

As in [ECS](https://en.wikipedia.org/wiki/Entity_component_system) architectures, systems react to what's inside structures. They can react to values, substructures, local names, anything. But they only work when there's a change, to produce a new stable value. When one of the components of the structure changes, all systems _interested in this component_ will recalculate their value.

In case of natural language values, [chatbot](https://www.rivescript.com/docs/tutorial) & [interactive fiction](https://github.com/inkle/ink/blob/master/Documentation/WritingWithInk.md) techniques can be used to produce a textual output value, along with relevant numerical values produced from classical math expressions. Textual and numerical merge in composite formulae. Searching is just a function. Values are often texts about (giving meaning to) explicit values of any type. Values are a structure's components. Loop.

### Semantic camera

while feasible, I would recommend against a semantic camera that would just spit out a text-adventure-like monolithic string. It has to produce a changing network of values (most of which are refs) linked by natural language. The right way to read it is to listen to its changes.

### Selectors

In the original OpenDDL, `$`/`%` chains (references) can be seen as a CSS derivative. You select nodes through selectors. But it needs more power (we're going 3D & Fiction). The [syntax](https://github.com/botbreeder/what-the-matrix-is/blob/main/Peggy.txt) above can be terse if needed. It just needs a good path syntax and solver.

In fact path is everything. If you take a typical file-system syntax for example, like `../stuff/foo.txt`, you'll see that the names are just constants. A path is fundamentally a sequence of selectors, that from one node lead you to other nodes. These selectors might be constants, but in our case, we'd rather use functions. So, a path is simply a sequence of selecting functions, each of which take 1 entity as input, and output 0+ entities.

We'll follow an approach similar to that of [CSS Selectors](https://www.w3schools.com/cssref/css_selectors.php), while staying compatible with `$`/`%` chains.

First the basic syntax:

- We have global `$names` and local `%names`, which we can use just like CSS uses IDs and classes.
- There's an "everything here" selector `*`.
- The CSS "and" is the comma.
- Juxtaposition (space) means "all inside" or "under here".
- `>` means immediately inside (direct child).
- `<` means immediate parents (what links here).
- `[prop]` means "having a (possibly empty) property called 'prop'." (can be used as CSS classes)
- `[prop = value]` means those where prop = value.
- `[prop != value]` means those where prop != value.
- `[prop >= value]` means those where prop >= value.
- `[prop <= value]` means those where prop <= value.
- `[prop > value]` means those where prop > value.
- `[prop < value]` means those where prop < value.
- `[prop ^= value]` means those where prop begins with value.
- `[prop $= value]` means those where prop ends with value.
- `[prop *= value]` means those where prop contains value.

Then we have the following filters (below, `?` is one of `=` `!=` `>=` `<=` `>` `<` `^=` `$=` `*=`):

- `type(type)` keep only structures of a given primitive/specific/derived type.
- `not(selector)` exclude from the selection.
- `where(%name ? value)` compares `%name` to a literal value. All of `%name`'s values must match.
- `wheresome(%name ? value)` compares `%name` to a literal value. At least one of `%name`'s values must match.
- `wherenone(%name ? value)` compares `%name` to a literal value. None of `%name`'s values must match.
- `enter(selector)` triggers once for each appearing item.
- `update(selector)` triggers everytime there's a change.
- `exit(selector)` triggers once for each disappearing item.
- `select(function call)` calls a user-defined function on the current selection.

Careful, you can't have a simple `div`-like selector, you have to put it inside a `type()` filter, as in `type(EmailAddress:string)`.

Then you can have user-defined functions, written in the host language and registered properly. This is where you'd make complex tests on complex values (i.e. tensors?), and so on.

Like, `ref{ $Ferrari %model where(%year < 1990) }`

is

```javascript
[
    {
        specific: null,
        type: 'reference',
        format: 'primitive',
        name: null,
        properties: null,
        content: [
            {
                type: 'reference',
                value: [
                    {
                        type: 'selector',
                        selector: 'GlobalName',
                        value: '$Ferrari'
                    },
                    {
                        type: 'selector',
                        selector: 'LocalName',
                        value: '%model'
                    },
                    {
                        type: 'selector',
                        selector: 'Where',
                        name: '%year',
                        comparison: '<',
                        value: {
                            type: 'number',
                            value: 1990,
                            format: 'literal'
                        }
                    }
                ],
                format: 'literal'
            }
        ]
    }
]
```

There can be 3D selectors. They come in the so-called user-defined functions. 

### Meaning of names

OpenDDL's global and local names are awesome. Global `$names` are unique, end of story. Unique things are unique. Local `%names` are another story. They mean things get (quantically?) accumulated by name (which simplifies the observer pattern), because local names are locally unique. Types are another sort of accumulation (without local uniqueness), and of course properties too.

- The selection rules of systems observe the accumulations.
- When triggered, they launch formulae.
- The formulae feed the accumulations (entering, updating, exiting).

ECS architectures _love_ accumulations.

About local `%names`, I'll put an additional constraint. An item with a local name cannot have the same name as the item that contains it. That's because we want to be able to refer to the parent easily, in Narratives. If you mention the `%name` of the container, there's no ambiguity: you're refering to it.

### Relations

Relations are mainly made of natural language and spatial data: lights, materials, textures, meshes, sounds, cameras, etc. They contain strings that link names. We call these strings Narratives. The names can be global or local, and may refer to any value, including other (named) narratives and references.

```
Relation
{
    Narrative:string
    {
        "%Jackie is a $Cat",
        "%Jackie believes that %WhoOwnsWho"
    },
    Narrative:string %WhoOwnsWho
    {
        "%Jane belongs to %Jackie"
    }
}
```

The whole point is to put the _narrative_ into the _spatial_.

Lights, materials, textures, meshes, sounds, etc, are _the vectors of_ the narrative elements. They are the way the narrative elements get in touch and interact. These vectors describe the propagation of the influence of things. The syntax above can do that. It can take [OpenGEX](http://opengex.org/).

It seems to mean that, this kind of code has to be inside the 3D engine's code. Of course it would be better to stay outside and follow the updates of the 3D engine. A method is needed to hook it up in unusual places.

THREEjs and BabylonJS are viable. Like, rebuilding them starting from the per-frame rendering entry point (the Smalltalk way). Pragmatically, using a 3D math library like [math.gl](https://math.gl/docs) feels like a good option too.

Using colors as IDs might prove useful.

### Object entities

Object entities need models and init functions. It's like OOrientedness, but it's about entities of an ECS architecture. Oentities can be tangible 3D objects or narrative artefacts in 3D (i.e. events). They are usually made of several structures working together. This concept is not a runtime necessity, but a dev tool, a way to author stuff. Once running, it's all structures and components. Nothing new here, we still build by composition.

We can follow a _prototype_ approach though. A prototype is like an Oentity instance, but with holes. We can clone it and fill in the blanks. If we want delegation, that would mean having `prototype:ref{ }` references that point to one or more prototypes (composing it), and resolving `%names` against the prototypes chains.









