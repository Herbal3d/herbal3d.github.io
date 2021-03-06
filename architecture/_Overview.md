--
layout: default
title: "Herbal3D: Overview of End-to-End Virtual World Display Infrastructure"
---
> "What brings you here, my child?"
> The scamp points and asks, "What is that?"
> "Ah. Curiosity. The best reason."

Herbal3d is an umbrella project for the building of a virtual world
and augmented reality viewer that is usable with different virtual
world and augmented reality systems.
That is, the building of the one viewer to rule them all.
Well, not "rule" them. But certainly display the 3d world represented
by the virtual world simulation and/or the augmented reality system.

The dream is to have a 3d viewer that can display avatars from
different virtual world systems in the same view or display the data
from multiple augmented reality systems in the same view.

`Basil` is the viewer portion of this project to build a a system of
3D object discovery, manipulation, and display.
Basil can be used to view virtual worlds, games, or augmented reality.
The display may be a computer screen, a tablet, head mounted goggles, or
a heads up display.

An instance of `Basil` talks to multiple `SpaceServers` which act as
the intermediaries between the `Basil` renderer and the virual world
or augmented reality system. `Basil` handles the problem of display
of the 3d objects while not having any "world" logic embedded in it.
The `SpaceServers` present to Basil the objects to display in space.

The Basil viewer works in concert with several other components:

* Basil Viewer itself displays 3D objects.
  This is an X11-like render client specialized for 3D objects;
* [Pesto] session manager manages connections to virtual worlds.
    It finds and instantiates connection modules to virtual world or augmented reality object sources;
* [Loc-Loc] is a univeral space server registration and lookup service.
* [Ragu] connects the Basil viewer to an [OpenSimulator] world to display
  the objects and avatars from there.
* other `SpaceServer` modules to connect to other virtual world systems.

`SpaceServers` are connection modules which connect `Basil` components to object repositories.
If `Basil` is being used to inhabit a virtual world,
the repositories hold the world to view.
If the viewer is being used for augmented reality, the repositories contain
locations and representations of objects in the real world.

Architecturally, the interface between the modules is defined by their APIs
and associated protocols.
This way, modules can have different implementations, can be distributed
(run in more than one process or computer), and can evolve independently.

===============================================================

---
layout: page
style: herbal3d
title: "Herbal3D: Overview of End-to-End Virtual World Infrastructure"
---
<div id="herbal-toc">
  <ol>
    <li><a href="#3d-virtual-world-and-augmented-reality-viewer-architecture">Introduction</a></li>
      <ol>
        <li><a href="#the-pieces">The Pieces</a></li>
        <li><a href="#finding-space-servers">Finding Space Servers</a></li>
        <li><a href="#the-basil-viewer">The Basil Viewer</a></li>
        <li><a href="#coordinate-system">Coordinate System</a></li>
        <li><a href="#space-servers">Space Servers</a></li>
        <li><a href="#making-a-system">Making a System</a></li>
        <li><a href="#the-virtual-worlds">The Virtual Worlds</a></li>
      </ol>
    <li><a href="#use-cases">Use Cases</a></li>
      <ol>
        <li><a href="#use-case:-virtual-worlds">Virtual Worlds</a></li>
        <li><a href="#use-case:-augmented-reality">Augmented Reality</a></li>
      </ol>
    <li>Other Stuff</li>
      <ol>
        <li><a href="#some-licensing-philosophy">Some Licensing Philosophy</a></li>
        <li><a href="#herbal-names">Herbal Names</a></li>
        <li><a href="#legal-stuff">Legal Stuff</a></li>
      </ol>
  </ol>
</div>

# 3D Virtual World and Augmented Reality Viewer Architecture

This article describes a system for
viewing virtual worlds and augmented reality.
The 'herbal' system is not only healthy
but it adapts many different
existing virtual world and augmented reality
systems to a single view.
This creates a single 3D view where an
[OpenSimulator] avatar can stand next to and interact with a
[High Fidelity] avatar.

For augmented reality, one can look out into the
real world and have a combined view of information from
different sources all merged into one enhanced view.

This viewer architecture works for everything from 3D headsets to
phones to desktops.
It is useful for education, entertainment, training, and gaming.

After reading this document you will understand
how existing world simulations ([OpenSimulator], [High Fidelity], [Sirikata],
to name a few) as well as existing augmented reality systems (ADD SOME HERE)
can be connected to a single 3D view.

General concepts and architecture are described here.
More detailed and focused documents will be created for the
implementation details.

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

The view process is imagined to be:

* the observer is at a location and looking in a direction
  (the view camera has a location and a direction);
* the world is queried as to what can be seen from here in that direction;
* the response to this query is multiple handles to various information and
  3D space servers that will handle portions of the 3D space being viewed.
  This fills the view space with smaller 3D spaces and their associated
  servers who have the information to fill the spaces;
* the viewer then queries the servers for contents for the smaller view spaces;
* the viewer displays the received information and objects relative to
  the viewers camera location;
* as the viewers camera moves, the display of information and objects is
  adjusted and, potentially, additional world queries are made to find more
  information and content servers to fill the viewed space.

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
The [Herbal System] attempts to solve this by using a 
known coordinate system that all applications can fit into.
That coordinate system is [WGS 1984] which is the world coordinate
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
Although not an early part of the [Herbal System] design, it is expected that,
in the future, users will always be online and their connection to the
shared world will be through multiple viewers.
[Pesto] has the job of connecting the user to viewers and the necessary
services in these different environments.

The following diagram shows the major parts of a complete virtual world
session that implements a unified, merged view of multiple virtual worlds.

<img src="HerbalHighLevelBlock-20151205.png" alt="Herbal block diagram" width="800"/>


# Use Cases

## Use Case: Virtual Worlds

Putting on a headset, you enter a 3D virtual world. As you look around
you see a rich landscape under the foreign sun with animals and avatars
wandering around. As you turn your head and walk, you move in the
landscape where you interact with the other inhabitants of this
virtual world.

To create this view, the "viewer" -- in this case, the program collecting
and displaying the 3D view in the headset -- queries the location service
to find the space servers for the direction the headset is "looking".
What is returned is a set of space servers that can fill the viewed
area.

It might also receive multiple 'layers' within a space.

## Use Case: Augmented Reality

Imagine a manually driven car (radical, I know) that has an enhanced
front wind shield that can display text and images. The wind shield could
be a transparent OLED screen where images can appear anywhere on the glass.

Further imagine a eye tracking system that tracks the location of the driver's
eyes. Using this information, the display system can compute where on the
wind shield to display something so it would overlay the view of something
ahead of the car. For instance, a stop light hanging over the road could
have text counting down the seconds until the light turns green
semi-transparently appearing on the wind shield where it would appear to
the driver as being on the stop light. As the driver moves her head, the
text on the wind shield would move around so it stayed overlaid on the
stoplight.

In addition to the stoplight, there is information about the road (speed
limits, lane information, ...), information about the weather, information
about the traffic, and information about the businesses and homes
around the car. So, in addition to all the technical problems of deciding
what and how to display information on this wind shield, there is the
problem of connecting to the multiplicity of information services needed
to create this view.

Thinking of the Herbal architecture, the car would query the location service
requesting connections to the space servers ahead of the car.
The response would be city servers (for intersection information and
construction alerts), servers for the buildings within the view, and more.
The "viewer" -- in this case the program displaying the information on
the wind shield -- would then query the multitude of space servers and
receive information on displayable information. All this would appear
on the wind shield.

You can easily see how this would also work for augmented reality glasses
or anything that wants to visually map information onto the real world.

# Extra Stuff

* Challenges
  * Latency
  * Control plane
  * Managing distribution
* Docker
* Scalability
* Mobile
* Bearer Certificates

## Some Licensing Philosophy

I believe that business and innovation is advanced by common infrastructures.
One real world expression of this is the public road system.
The world could consist of independently owned roads with tolls.
In this world, each owner supports their piece of road.
Imagine such a road system.
Different pieces of road would conform to different design standards.
Toll collection system would be different with different payment and membership
requirements.
Traveling any distance would require constantly stopping and paying and
navigating different lane sizes and pavement qualities.
Some destinations just wouldn't have roads.

But if an entity creates a common infrastructure of uniform roads, travel
and shipment becomes easy. The roads still aren't free (taxes through
a government or a single company) but now commerce and mobility explodes.

My assertion is that overall prosperity is greater when there are many
uses and businesses built upon the common infrastructure vs the world
where there are businesses making money off creating the infrastructure.
Some economists probably have opinions here.

I see the [Herbal System] as a common infrastructure that enables
shared and extensible viewing of images in the real and virtual worlds.
The overall ecosystem will be larger, more innovative, and more
prosperous if this common infrastructure is created and made available
without barriers.

This philosophy means the sources will be "open source" licensed with
some core  technologies licensed with GPLv3 so the feature is always
available and in the open.
Some of the modules could have instances
created for specific uses and, since the glue between components is
the protocols, these instances could be proprietary.
Additionally, some reference designs of certain modules could
have a more flexible license ([BSD License], [Apache License], [MIT License], ...) 
but, in general, the core development should happen in a larger,
public community.

## Herbal Names

Just for fun, the various components have herbal names.
This started with the [Basil Viewer] and 
spilled over into [Pesto] and [Ragu].
Green, leafy herbs are good for you and taste good too.

## Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].

Since every idea in  the world is covered by a patent somewhere, I make
no claims as to the ownership or availability of any design or concept
described above.

[^1]: see work in [Sirikata] for querying space servers by pixel size.
[^2]: a term coined from [Sirikata]

[WGS 1984]: http://earth-info.nga.mil/GandG/publications/tr8350.2/tr8350_2.html
[double]: https://en.wikipedia.org/wiki/Double-precision_floating-point_format
[X11 Server]: https://en.wikipedia.org/wiki/X_Window_System
[OpenSimulator]: http://opensimulator.org/
[High Fidelity]: http://highfidelity.io/
[Oculus]: http://www.oculus.com/
[View Service]: http://loc-loc.net/
[Herbal System]: http://herbal3d.org/
[Basil Viewer]: http://basilviewer.org/
[Pesto]: http://misterblue.github.io/pesto/
[Ragu]: http://misterblue.github.io/ragu/
[Sirikata]: http://sirikata.org/
[BSD License]: http://opensource.org/licenses/BSD-3-Clause
[MIT License]: http://opensource.org/licenses/MIT
[Apache License]: http://opensource.org/licenses/Apache-2.0
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 et ai
-->
===============================================================

<ul class="post-list">
    {% for post in site.posts %}
        {% if post.status == "publish" %}
            {% include one-post.html thepost=post thecontent=post.content ownpage="no" %}
        {% endif %}
    {% endfor %}
</ul>

<p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

[OpenSimulator]: http://opensimulator.org/
[Pesto]:  http://herbal3d.github.io/pesto/
[Loc-Loc]: http://herbal3d.github.io/loc-loc/
[Ragu]: http://herbal3d.github.io/ragu/
===============================================================
`Basil` is the general name for the "viewer". Basil contains no
"world logic" -- its only job is displaying the 3d objects for
a viewer. So, if the viewing implementation is a head mounted display,
that version of Basil will worry about the correct display as
the viewer's head moves, etc. If the viewing implementation is
a desktop computer screen, that Basil version would implement
the screen display.

All versions of Basil are controlled through a common protocol API.
Basil is essentually a slave process that is told what to display
by multiple `SpaceServers`.
Basil essentually merges the view from multiple SpaceServers.
To do this, SpaceServers must share a coordinate space.
The model used by Herbal3d is a planetary system that defaults
to Earth and is based on the World Geodetic System [WGS84].

The merging of the contents of multiple SpaceServers is loosely
called merging `layers`. So if the Basil viewer is an augmented reality
headset viewing a street scene, the "layers" could be
the street location information from the municipal servers,
the traffic information from the city's traffic control,
the information from the surrounding businesses.
These "layers" are merged into a single view of the scene.

See the [Use Cases] for more complete descriptions of possible
configurations of systems that could use the Herbal3d architecture.
The use cases described include
augmented reality goggles,
virtual world 3d goggles,
3d views of virtual worlds in browsers,
and 
a merged 3d view into multiple virtual worlds.

Since the Basil viewer has no world logic, the user interface
is implemented by one or more "UI Servers".
Additionally, the whole session is managed by a "Session Manager".
These run externally to the Basil viewer and present more
layers to the viewer's view.

## Block Diagram

This block  diagram is an idealized view of a complete renderer.

# Philosophy

The Basil viewer system presents a rendered view of objects in a 3D space.
The objects can represent things in the real world or in a virtual world.
The viewer system is extensible and can accommodate new object spaces by the
addition of modules.

The modules are defined by their functionality, interface, and, optionally, by
the protocol to communicate with the module. The relationship between modules
and where they run (some computer, different computer, same address space, ...)
is not defined so many operational configurations are possible -- it depends on
what is best for the specific function provided by the module.

Basil tries to keep functionality separate preferring to define single function
modules that communicate through an API rather than conflating functionality into
one module. Thought should always be given to where new functionality should be
added. For instance, animation functionality is often added to a renderer because
of needed interactions between the animation computation and the frame update
timing. Architecturally, though, it would be better to add to the renderer
the ability to add inter-frame computation modules and then add animation, bones,
etc as separate functional modules. This extended the renderer with its needed
functionality and doesn't embed specific additional functions into the renderer.

The initial implementation is in JavaScript but that is not required.
Future module implementations can be in any programming language as long as
the API interfaces are kept.
