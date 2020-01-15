---
layout: default
title: "Herbal3D: Virtual World Viewer Infrastructure"
---
> "What brings you here, my child?"
> <br>The scamp points and asks, "What is that?"
> <br>"Ah. Curiosity. The best reason."

Herbal3d is an umbrella project for the building of a virtual world
and augmented reality viewer.
The viewer is usable with different virtual
world and augmented reality systems.
That is, the building of the one viewer to rule them all.
Well, not "rule" them. But certainly display the 3d world represented
by the virtual world simulation and/or the augmented reality system.

The dream is to have a 3d viewer that can display avatars from
different virtual world systems in the same view or display the data
from multiple augmented reality systems in the same view.

`Basil` is the viewer portion of this project.
It simply displays 3D entities and UI features as instructed and
it handles all device dependent considerations.
Basil can be used to view virtual worlds, games, or augmented reality.
The display may be a computer screen, a tablet, head mounted goggles, or
a heads up display.

An instance of `Basil` talks to multiple `SpaceServers` which act as
the intermediaries between the `Basil` renderer and the virual world
or augmented reality system. `Basil` handles the problem of display
of the 3d objects while not having any "world" logic embedded in it.
The `SpaceServers` present to Basil the objects to display in space.

Currently, this umbrella project contains the projects:

* BasilJS: a browser based version of `Basil`;
* BasilG: a [Godot] based application that implements `Basil`;
* RaguOS: a `SpaceServer` for [OpenSimulator] using region modules;
* Espazote: a `SpaceServer` for [Project Athena] (open source of [High Fidelity])

News items on progress and releases are listed in the sidebar.
Other places to go from here:

* [Architectural Overview]
* [News]
* [Basil Protocol Definition]
* [Basil Issues]
* [Basil Sources]

## Some Licensing Philosophy

This project started as the personal project of [Robert Adams].

I believe that business and innovation is advanced by common infrastructures.
One real world expression of this is the public road system.
The nation could have independently owned roads with tolls.
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

I see the Herbal System as a common infrastructure that enables
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
This started with the Basil Viewer and 
spilled over into [Pesto] and [Ragu].
Green, leafy herbs are good for you and taste good too.

## Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].

Since every idea in  the world is covered by a patent somewhere, I make
no claims as to the ownership or availability of any design or concept
described above.

[OpenSimulator]: http://opensimulator.org/
[BasilJS]: https://github.com/Herbal3d/basil/tree/master/Basiljs 
[BasilG]: https://github.com/Herbal3d/basil/tree/master/Basilg 
[Loc-Loc]: https://herbal3d.github.io/loc-loc/
[RaguOS]: https://herbal3d.github.io/ragu/
[Ragu]: https://herbal3d.github.io/ragu/
[Espazote]: https://herbal3d.github.io/espazote/
[Project Athena]: https://projectathena.io/
[High Fidelity]: https://www.highfidelity.com/
[Architectural Overview]: https://herbal3d.github.io/architecture/Overview.html
[News]: https://herbal3d.github.io/News.html
[Basil Issues]: https://github.com/Herbal3d/basil/issues
[Basil Sources]: https://github.com/Herbal3d/basil
[Robert Adams]: https://misterblue.com/
[BSD License]: http://opensource.org/licenses/BSD-3-Clause
[MIT License]: http://opensource.org/licenses/MIT
[Apache License]: http://opensource.org/licenses/Apache-2.0
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/
