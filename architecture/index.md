---
layout: default
navigation: architecture
---

# Architecture

The Herbal3d system is a rendering system for
viewing virtual worlds and augmented reality.
The 'herbal' system adapts many different
existing virtual world and augmented reality
systems to a single view.
This creates a single 3D view where an
[OpenSimulator] avatar can stand next to and interact with a
[Project Athena] avatar.

For augmented reality, one can look out into the
real world and have a combined view of information from
different sources all merged into one enhanced view.

This viewer architecture works for everything from 3D headsets to
phones to desktops.
It is useful for education, entertainment, training, and gaming.

## The Pieces

There are many virtual worlds to explore
and, for augmented reality, there are many companies all wanting
to own and control the sources of information.

So, any viewer has to first and foremost be able to merge these
multitude of data sources.

Additionally, a 3D viewer has to adapt to many configurations --
head mounted 3D displays are 'hot' at the moment but people still
have computer screens and then there are the mobile phones.
Augmented reality will have glasses, tablets, and anywhere there
is glass.

So, at the center of a view experience, is a viewer ("Basil")
which connects to a multitude of 3D object sources.
To get Basil hooked up, a session manager ("Pesto") 
sets up the connections and is the coordination hub of
the user's interactions.
Pesto gets login information from the user,
creates the Basil viewer,
sets the initial place in the world to view,
queries for the Space Servers, and
links the Space Servers to Basil.
The Space Servers then send Basil the items to display.

## Finding Space Servers

Looking into a virtual world
or looking out into the real world
is based on a 'camera' at some location that is pointing in some direction.
The problem is is figure out what that camera sees and how to access
representations for what should be in the camera view.

The 'view' process is imagined to be:

* the observer is at a location and looking in a direction
  (a view camera has a location and a direction);
* the world is queried as to what can be seen from here in that direction;
* the response to this query is multiple handles to various information and
  3d object servers that will handle portions of the 3d space being viewed.
  This fills the view space with smaller 3d spaces and their associated
  servers who have the information to fill the spaces;
* the viewer receives from the servers contents for the smaller view spaces;
* the viewer displays the received information and objects relative to
  the viewers camera location;
* as the viewers camera moves, the display of information and objects is
  adjusted and, potentially, additional contents is added from the
  information and object servers to fill the viewed space.

So, a view consists of the combination of multiple spaces.
For instance, looking down a street one would see multiple businesses
and each of those businesses would have something they want to display
for their 3D space that you see.
Similarity, for a virtual world, one would look out into a vista that
consists of the local village as well as the mountains in the distance.

This introduces the idea of an index service
that holds information about all the space servers for filling 
the world space.
All the space servers register with this "location service"
and the location service is queried by viewers to get the handlers
for all the space being viewed.
Think of it as the view being split into 3D
areas each of which has a different server supplying the objects to
display in that area.

In a virtual world, for instance, a query for a view might return a server who
can present a detailed display of the local village and another server
who will present a distant view of the mountains surrounding the village.
The query a viewer makes includes the distance and pixel size needed by
the viewer so a server can see that this view is far away an only requires
a low resolution view of the object[^1]. That the query includes the viewer
requirements (resolution, interaction requirements, ...) means that different
services could respond for different spaces in the viewed scene.

The viewer is then responsible for adding together the information from
the multiple space servers into one view. Since this is 3D space, addition
is easy.

## The Basil Viewer

This leads us to the viewer -- the piece of code that combines the objects
presented by the multiple space servers into a coordinated view for a user.
For ease of reference, the viewer is named "Basil".

Basil has the job of communicating with multiple space servers and combining
their objects into a consistent view for the user.

Think of the [X11 server] architecture.
The X11 server is a viewer service for 2D content.
Multiple 2D content providers (xterm consoles, xclocks, ...) connected to
one server that manages the display of multiple 2D object on one display.
The applications that presented 2D information were separate from the
display engine and the manager for the multiple 2D areas (the "windows manager")
was also separate from the 2D applications.

Basil aspires to that design but for 3D spaces.
So, consider multiple sources of 3D objects filling the 3D space viewed
while the manager of this coordinated view is separate from both the viewer
and the multiple sources.
This would solve many problems of existing viewers where the content connection
and the UI were intimately linked.
For Basil, the UI is a separate program that connects to the view service and
manages the multiple space servers.
The UI is just another service and thus can be replaced if needed.

Basil connects to the multiple space services and gets objects to display
in the viewed 3D space.
The interface between the space servers and Basil is a standard
API that allows the space servers to properly display its content.
The current state-of-the-art is represented in the multiple game
engines in the world.  
So, in the most abstract sense, Basil is *just* a common interface to most of
the features available in current game engines
(meshes, shaders, animations, boned-objects, textures, materials, etc).
That plus some coordination
functions so multiple object suppliers can present a unified view for the user.

One feature of Basil's additive nature is that multiple space servers can
fill the same space.
For instance, a space could be filled with both static and dynamic objects
(tables and walls along with a real-time avatar) and there would be multiple
space servers with one presenting the static objects and a different
one displaying the real-time avatar.
In this case, the two space servers could be using completely different
protocols, caching schemes, and update strategies for the display of their objects.

Basil considers the world made up of 'layers' that fill 'spaces' within
a 'view' and there may be multiple layers within a space.
Basil's job is adding together all the layers and managing the updates
and optimizations necessary for efficient display of all the 3D data.

An important point about 'viewers' -- viewers just create a view into a 3D world
and what defines the Basil viewer is the protocol connecting it to
the space servers.
Just like the X11 Server, there will be multiple implementations of the
Basil viewer for different applications.
What is rendered depends on the display and user requirements.
There can be viewers that display a stereoscopic view (for devices
like [Oculus]). There can be viewers that display on traditional 2D
computer screens.
There can also be viewers that describe what is seen for the sight impaired.

The 'viewer' concept is the idea of a programming
module that is able to get information about multiple spaces and then
render those spaces for a user.
Rendering a view for a machine is not precluded either.

## Coordinate System

One problem with merging multiple real world and virtual world
spaces is the coordinate system.
The Herbal System attempts to solve this by using a 
known coordinate system that all applications can fit into.
That coordinate system is [WGS84] which is the world coordinate
system used by the GPS system.
This supplies an <X,Y,Z> coordinate system for the planet Earth.
This puts all virtual world and augmented reality application into
the real world.
All measurements are in meters.

While real-world coordinates work for augmented reality, for
virtual worlds there just needs to be a mapping from the
virtual world coordinates to the real world.
There is no reason that an [OpenSimulator] virtual world can't
exist on a coffee table in some person's house.

It is expected that coordinates will be <x,y,z> with each coordinate
as a [double] (64 bit floating point number).
This gives about 15 significant digits of coordinate information
which should be sufficient for both large scale locations and
micro-scale worlds.
It is expected that there are transformations available to convert
planetary coordinates to localized spherical tangential plane coordinates
and back.

There will certainly be optimizations in the protocols to
transfer smaller or relative coordinate information.

## Space Servers

Basil finds and communicates with "space servers"[^2].
Space servers have the job of presenting to Basil the view of
the space they are managing.
So, remember that Basil sees multiple space servers with some
managing specific 3D spaces in the view and some overlapping and
presenting different layers in a 3D space in the view.
As mentioned earlier, the space server has some information from
Basil as to the requirements of the display.
For instance, the space being served could be in the distance and
thus a low resolution version of the world.
Or the display could require edit level access to the objects.

Space servers are the interface between the view and the simulated world.
The world could be stores along the road or complex physical
simulations of objects in a virtual world.
So the space servers have to solve the problems of synchronization and
latency between the simulated world and the view being presented.
This is the main job of the space servers that connect to Basil.

The space server is the adaptor between, for instance, a [OpenSimulator]
based region and Basil.
This has to include format conversion and event passing.
Coordination with user interface elements would enable
editing and movement of in-world objects but that is a feature
of the space server interface to Basil and not a feature of Basil --
Basil only enables the features of selection and pointing to be used
by user interface and space servers.

Consider a virtual world like [OpenSimulator].
The space server for this virtual world would be responsible for
giving Basil information about which objects are in view
(meshes and boned avatar representations)
and where to fetch representations of those objects for viewing.
To make the objects available to Basil, format conversion might be
necessary as well as coordinate conversion to make the
[OpenSimulator] regions appear in a reasonable place in the
real-world coordinates of Basil.

## Making Viewer System

The space server query service and the Basil viewer are just parts
of a system that creates a view of layers of a world. The following
diagram shows many of the parts.

There exists a session server ([Pesto]) that handles the user's ongoing
state and which viewer is interesting at the moment.
Although not an early part of the Herbal System design, it is expected that,
in the future, users will always be online and their connection to the
shared world will be through multiple viewers.
[Pesto] has the job of connecting the user to viewers and the necessary
services in these different environments.

The following diagram shows the major parts of a complete virtual world
session that implements a unified, merged view of multiple virtual worlds.

<img src="HerbalHighLevelBlock-20151205.png" alt="Herbal block diagram" width="800"/>

## Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].

Since every idea in  the world is covered by a patent somewhere, I make
no claims as to the ownership or availability of any design or concept
described above.


[^1]: see work in [Sirikata] for querying space servers by pixel size.
[^2]: a term coined from [Sirikata]

[OpenSimulator]: http://opensimulator.org/
[Project Athena]:
[X11 Server]: https://en.wikipedia.org/wiki/X_Window_System
[Oculus]: http://www.oculus.com/
[WGS84]: https://en.wikipedia.org/wiki/World_Geodetic_System
[double]: https://en.wikipedia.org/wiki/Double-precision_floating-point_format
[Use Cases]: {{ "/architecture/UseCases.html" | prepend: site.baseurl }}
[Pesto]: http://misterblue.github.io/pesto/
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 ai et spell
-->
