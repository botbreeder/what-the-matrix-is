# What the matrix is
A 3D world engine, with semantic textures applied to meshes, lights and sounds. 

![The Matrix](https://github.com/botbreeder/what-the-matrix-is/raw/main/bm.jpg)

If an AI is connected to a text-world, capable of showing plausible cause-consequence chains, the percepts we feed it with would be like textual descriptions floating dynamically in a 3D space. For visual and audio senses, a typical 3d game engine, running headless, could provide the space-time skeleton on which "semantic textures", made of qualitative/quantitive descriptions (potentially expressed in a subset of a natural language), could be applied to form this world.

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

#### Derivative data-types

What could be useful is the possibility to define **derivative data-types**, that is, data-types that are syntactically expressed as a primitive type, but have other names. For example, an `EmailAddress` data-type could be built from the primitive `string` data-type. The derivative data-type puts additional constraints on an existing type, and makes its meaning explicit.

```
EmailAddress:string { "github@gmail.com" }
```

#### Primitive data-types

That's about it, on the syntax level. For now. OpenDDL provides almost everything else. We stay very declarative.

The list of available primitive data-types can be reduced to:
```
boolean         bool        b
number          num         n
string          str         s
reference       ref         r
type            type        t
atom            atom        a
```

where `atom` can be an identifier or a name, including the initial `$` or `%` as needed.

## Computation

As in ECS architectures, systems react to what's inside structures. They can react to values, substructures, local names, anything. But they only work when there's a change, to produce a new stable value. When one of the components of the structure changes, all systems _interested in this component_ will recalculate their value.

