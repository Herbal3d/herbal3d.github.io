---
layout: default
title: "Herbal3D: Virtual World Viewer Infrastructure"
---
> "What brings you here, my child?"
> <br>The scamp points and asks, "What is that?"
> <br>"Ah. Curiosity. The best reason."

NOTE: 20200118: Work in progress. Contact [the writer] with questions.

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

Currently, this umbrella project contains the projects:

* BasilJS: a browser based version of `Basil`;
* BasilG: a [Godot] based application that implements `Basil`;
* RaguOS: a `SpaceServer` for [OpenSimulator] using region modules;
* Dill: a `SpaceServer` for [Project Athena] (open source of [High Fidelity])

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

News items on progress and releases are listed in the sidebar.
Other places to go from here:

* [Architectural Overview]
* [News]
* [Basil Protocol Definition]
* [Basil Issues]
* [Basil Sources]

Other topics:

* [Licensing Philosophy]
* [The Herbal Names]

## Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].

Since every idea in  the world is covered by a patent somewhere, I make
no claims as to the ownership or availability of any design or concept
described above.

[the writer]: mailto:herbal3d-w@misterblue.com
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
[Licensing Philosophy]: ./LicensingPhilosophy.html
[The Herbal Names]: ./HerbalNames.html
