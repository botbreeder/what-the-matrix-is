# What the matrix is
A 3D world engine, with semantic textures applied to meshes, lights and sounds. 

![The Matrix](https://github.com/botbreeder/what-the-matrix-is/raw/main/bm.jpg)

If an AI is connected to a text-world, capable of showing plausible cause-consequence chains, the percepts we feed it with would be like textual descriptions floating dynamically in a 3D space. For visual and audio senses, a typical 3d game engine, running headless, could provide the space-time skeleton on which "semantic textures", made of qualitative/quantitive descriptions (potentially expressed in a subset of a natural language), could be applied to form this world.

## Percepts

This is literally about semantic information embedded in a 3D scene. Say you have a red sphere in front of you. Having an RGB value of 255,0,0 doesn’t carry the “redness” of the sphere, but having a description like “this is red” does. But that’s not all. The agent doesn’t receive an objective description of a 3D scene, but a view, as delivered by a “semantic camera”. If you’re looking towards north, and the sphere is located east, you’ll see it on your right.

It’s hard to imagine the input as received by the agent, because we humans are obviously not able to combine semantic (propositional) data with 3D data. The image above was an attempt to express what is otherwise unthinkable. So I’ll try to describe it better.

What I have in mind is not like [X3D](https://en.wikipedia.org/wiki/X3D) for instance. It’s not meant to be rendered visually. Instead, it is a description of the situation, embedded in a 3D space, and moving as time passes. Take a dog for instance. From far away, it could be described as a box, labelled “dog”. If you get closer, you see that inside that bounding box there are 4 cylinders for legs, and maybe a line for the tail, and a sphere for the head. These basic shapes contain more precise (smaller) shapes, along with semantic descriptions of what they are, and what are their relations to their parents’ shapes. There can be several relations on the same nodes, like `is-part-of` relations, ...etc. But I don’t want to give the impression that it’s a single hierarchy, because it’s not. It’s a graph rather than a tree.

A given node has a 3D shape, and is related to (is located in/contains) several other 3D shapes by relations. A relation-type is identified primarily by a natural language sentence with holes. Relations are themselves located in space-time, and after all, nodes are just like 0-arity relations. By default, relations spatially cover what they relate to. Relations are in fact functions of what comes next: it's like a never satisfied spreadsheet.

Let's give a name to these nodes/relations. Let's call them [Narratives](https://github.com/botbreeder/mudbasic#narratives-as-reusable-blocks). Just replace the numbers by global `$names` and local `%names` from [OpenDDL](http://openddl.org/).

It is also a choreographic languege. [Jolie](https://docs.jolie-lang.org/v1.10.x/introduction/index.html) is pretty.

I need a syntax.

What OpenDDL lacks is a functional, or procedural, notation. I can't see a better candidate than prefixed s-expressions.

```
a(b c)(d e f)
```

meaning `((abc) d e f)`.

Assignment / equivalence can prove useful.

```
a: b(c)
```

Grouping things as ordered lists feels basic too, although this one feels controversial. OpenDDL doesn't know single values. If you give an integer, you might as well give a thousand integers. So, it knows about arrays because it doesn't know anything else. Everything is an array. And it knows about "subarrays", obviously because 3D stuff is often regularly organized.

Still, there's no way to distinguish a thing from a list of things. So if we're not going quantic, it needs a `List` type, and will benefit from a special (although familiar) notation.

```
[a b c]
```

That's about it, on the syntax level. For now. OpenDDL provides almost everything else. We stay very declarative.

