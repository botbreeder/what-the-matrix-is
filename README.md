# What the matrix is
A 3D world engine, with semantic textures applied to meshes, lights and sounds. 

![The Matrix](https://github.com/botbreeder/what-the-matrix-is/raw/main/bm.jpg)

If an AI is connected to a text-world, capable of showing plausible cause-consequence chains, the percepts we feed it with would be like textual descriptions floating dynamically in a 3D space. For visual and audio senses, a typical 3d game engine, running headless, could provide the space-time skeleton on which "semantic textures", made of qualitative/quantitive descriptions (potentially expressed in a subset of a natural language), could be applied to form this world.

## Percepts

This is literally about semantic information embedded in a 3D scene. Say you have a red sphere in front of you. Having an RGB value of 255,0,0 doesn’t carry the “redness” of the sphere, but having a description like “this is red” does. But that’s not all. The agent doesn’t receive an objective description of a 3D scene, but a view, as delivered by a “semantic camera”. If you’re looking towards north, and the sphere is located east, you’ll see it on your right.

It’s hard to imagine the input as received by the agent, because we humans are obviously not able to combine semantic (propositional) data with 3D data. The image above was an attempt to express what is otherwise unthinkable. So I’ll try to describe it better.

What I have in mind is not like [X3D](https://en.wikipedia.org/wiki/X3D) for instance. It’s not meant to be rendered visually. Instead, it is a description of the situation, embedded in a 3D space, and moving as time passes. Take a dog for instance. From far away, it could be described as a box, labelled “dog”. If you get closer, you see that inside that bounding box there are 4 cylinders for legs, and maybe a line for the tail, and a sphere for the head. These basic shapes contain more precise (smaller) shapes, along with semantic descriptions of what they are, and what are their relations to their parents’ shapes. There can be several relations on the same nodes, like `is-part-of` relations, ...etc. But I don’t want to give the impression that it’s a single hierarchy, because it’s not. It’s a graph rather than a tree.

A given node has a 3D shape, and is related to (located in) several other 3D shapes by relations. A relation-type is identified primarily by a natural language sentence with holes.


