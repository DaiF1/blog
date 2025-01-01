---
title: Merging Houses
summary: |
    Or "Can we make infinite assets for a merging game?"
date: 2024-12-22
tags: ["graphics", "procedural generation"]
authors:
    - "daif.en"
---

My mother really likes playing those mobile merging games.

For the ones who don't know, the goal of those games is to put together 2
objects of the same type to create another. In most cases, the game gives you
tasks to complete, which you need to make different objects for.

I noticed that there is always a limit to how much you can merge objects.
Mainly because it requires a lot of people to design and draw that many assets.

So the question is: can we automate that process? And can we create a
merging game with an infinite number of objects?

## The tools

For this project, I'll be using the *gmsh* python library, as well as
*pygmsh* for a simpler code syntax. Here are the 2 repositories:

{{< github repo="sasobadovinac/gmsh" >}}

<br/>

{{< github repo="nschloe/pygmsh" >}}

## Building a house

For this project, we'll try to make houses of infinite sizes. So first, we need
a house!

### The first cube

A good house starts with a cube. So we'll start by trying to make a cube using
*gmsh*. Here is the python script that does so:

```py
import gmsh
import pygmsh

# Starts describing the geometry
with pygmsh.occ.Geometry() as geom:
    # Add a cube to it
    mesh = geom.add_box(
        (0, 0, 0), # position
        (1, 1, 1)  # scale
    )

    # Convert the geometry to a real mesh
    geom.generate_mesh()

    # Write the generated mesh to the 'out.msh' file
    gmsh.write("out.msh")
    gmsh.clear()
```

When opening the generated `out.msh` with *gmsh*, we get this beautiful cube:

{{< figure src="./images/cube.png" alt="a yellow cube" >}}

{{< alert "circle-info" >}}

By default, *gmsh* only shows the cube's triangles. You can go to `Tools > Options > Mesh`
and select here what you want to see.

{{< /alert >}}

### Better with a roof

Now our house needs a roof. Unfortunatly, we do not have any basic 3D form to
put on top of it.

That's why we'll use *extrusion*.

#### Extrusion

The goal of extrusion is to extend a 2D face into 3D. For example, turning
a square into a cube.

Here are the different steps of extrusion:
1. make a copy of the object you want to extrude, and move it to the desired position;
2. link each vertices from the 2 objects to create the missing faces.

{{< figure src="./images/extrude.gif" alt="extrusion on a triangle" caption="Extrusion on a triangle" >}}

#### Making the roof

To use extrusion, we first need our 2D object. We'll start by drawing a triangle
on top of the cube:

```py
triangle = geom.add_polygon([
    [-0.1, -0.1,   1], # Removed -0.1 from each side of the roof
    [-0.1,  1.1,   1], # for a little offset with the main cube
    [-0.1,  0.5, 1.5],
], mesh_size=1)
```

{{< figure src="./images/roof_triangle.png" alt="a cube with a triangle on top of it" >}}

Now we need to make the extrusion. Thankfully, *pygmsh* has a method to do just
that:

```py
# The copy is moved by 1.2 on the x axis
geom.extrude(triangle, (1.2, 0, 0))
```

This will give us our final roof:

{{< figure src="./images/cube_with_roof.png" alt="a cube with a roof" >}}

### How to get in

To make it seem like there is an entrance to the house, we need to carve a
rectangle inside the house cube to imply its presence.

To do that, we'll need *boolean operations*.

#### Boolean operations

There is 2 types of boolean operations we need for this project:
- union, which combines 3D objects;
- difference, which carves a object into the other.

It is identical to union and difference between 2 mathematical sets:

{{< figure src="./images/boolean.gif" alt="schema of union and difference operations" caption="Union and difference boolean operations" >}}

#### Making the door

To create our door, we'll use a boolean diffenrece.
We first need to highlight the space we want to remove:

```py
door = geom.add_box(
    (0.4,   0,   0),
    (0.4, 0.1, 0.6)
)
```

Here is the result with only edges for more visibility:

{{< figure src="./images/house_wireframe.png" alt="A house" >}}

Once the space is delimited properly, we simply remove it with the
boolean difference.

```py
geom.boolean_difference(mesh, door)
```

{{< figure src="./images/house.png" alt="A house" >}}

### A complete house

Here is the final script for our building house:

```py
import gmsh
import pygmsh

# Starts describing the geometry
with pygmsh.occ.Geometry() as geom:
    # Add a cube to it
    mesh = geom.add_box(
        (0, 0, 0), # position
        (1, 1, 1)  # scale
    )

    # Create the door box
    door = geom.add_box(
        (0.4,   0,   0),
        (0.4, 0.1, 0.6)
    )

    # Remove it from the body
    geom.boolean_difference(mesh, door)

    # Create the triangle
    triangle = geom.add_polygon([
        [-0.1, -0.1,   1],
        [-0.1,  1.1,   1],
        [-0.1,  0.5, 1.5],
    ], mesh_size=1)

    # Extrude it
    geom.extrude(triangle, (1.2, 0, 0))

    # Convert the geometry to a real mesh
    geom.generate_mesh()

    # Write the generated mesh to the 'out.msh' file
    gmsh.write("out.msh")
    gmsh.clear()
```

## Define a merge pattern

Now that we are able to build a house, let's define a pattern that will allow
us to describe any type of house.

We'll consider the house built earlier to be of *level 0*.

Here is the pattern we will use:
- a level 1 house has a larger base;
- a level 2 house has a flower pot;
- a level 3 house has a little balcony.

On level 4, we create a new level 0 floor for the house. Then the cycle
continues: a level 5 house is composed of a level 3 base and a level 1 floor,
a level 6 house is composed of a level 3 base and a level 2 floor, etc.

### A larger house

To make a larger house, we simply to transform our base cube into a cuboid.

We also need to account for the change in the door placement.

{{< figure src="./images/house_lvl_1.png" alt="A level 1 house" >}}

### Adding some plants

For the plants, we'll add a cuboid at the front of the house to act as a pot,
and 3 random ellipsoids to fill it. You can see the 3 different variations below:

{{< figure src="./images/house_lvl_2.png" alt="A level 2 house" >}}

### A little balcony

For the balcony, same principle: we'll combine cuboids with a boolean union:

{{< figure src="./images/house_lvl_3.png" alt="A level 3 house" >}}

### New floor

This is where things gets fun.

New floors follow the same rules, with one small detail: we can't have a door.

To keep a consistent level of details, we'll add a window with the same method
used for the door.

We create a window shaped form:

{{< figure src="./images/window.png" >}}

And remove it with boolean difference:

{{< figure src="./images/carved_window.png" >}}

Here is the code used to do so:

```py
# Add a cube for the floor
mesh = geom.add_box(
    (0, 0, 0), # position
    (1, 1, 1)  # scale
)

# Create the window frame
window_frame = geom.add_box((0.2, -0.1, 0.25), (0.6, 0.15, 0.5))

# Window glasses
glasslb = geom.add_box((0.225, -0.1, 0.275), (0.25, 0.2, 0.2))
glasslt = geom.add_box((0.525, -0.1, 0.275), (0.25, 0.2, 0.2))
glassrb = geom.add_box((0.225, -0.1, 0.525), (0.25, 0.2, 0.2))
glassrt = geom.add_box((0.525, -0.1, 0.525), (0.25, 0.2, 0.2))

# Combine the window with boolean union
window = geom.boolean_union([window_frame, glasslb, glasslt, glassrb, glassrt])

# Remove it with a boolean difference
geom.boolean_difference(mesh, window)
```

We're missing one final detail to make the house truly complete.

Currently, we have this big empty space next to the last floor:

{{< figure src="./images/missing_roof.png" >}}

To fix this problem, we simply need to add a little roof section next to it.

{{< figure src="./images/house_lvl_4.png" alt="A level 4 house" caption="The final level 4 house" >}}

### Adding some variations

As a final touch, we'll add some variations to the details.
We'll alternate the side of the window and the balcony on each floor.

Here is a level 8 house to illustrate:

{{< figure src="./images/house_lvl_8.png" alt="A level 8 house" >}}

## Finishing notes

The script works well, but it isn't perfect. You may have noticed when opening
a file with *gmsh*, but the generated house is composed of way too many
triangles.

The script also does not have great performances due to the many boolean
operations in play:

| House Level | Time (in sec) |
|-------------|---------------|
| 0           | 0.77          |
| 1           | 1.06          |
| 2           | 1.36          |
| 3           | 1.53          |
| 4           | 2.39          |
| 16          | 8.98          |
| 100         | 111.55        |

If you want to tackle those problems, you can find the final of the script here:

<style>
/* https://github.com/lonekorean/gist-syntax-themes */
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/d49b91b3/stylesheets/idle-fingers.css');

@import url('https://fonts.googleapis.com/css?family=Open+Sans');
body {
  font: 16px 'Open Sans', sans-serif;
}
body .gist .gist-file {
  border-color: #555 #555 #444
}
body .gist .gist-data {
  border-color: #555
}
body .gist .gist-meta {
  color: #ffffff;
  background: #373737; 
}
body .gist .gist-meta a {
  color: #ffffff
}
body .gist .gist-data .pl-s .pl-s1 {
  color: #a5c261
}
</style>
<script src="your-gist-embed"></script>

{{< gist DaiF1 e2d84b194dc2c893ada48777c9187b5b >}}
