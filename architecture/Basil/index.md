---
layout: page
title: "Herbal3D: Basil Architecture"
---

# Basil Architecture

The overall design of Basil is described in the
[Basil section of the Architecture document]( {{ "/architecture/index.html.html#the-basil-viewer" | prepend: site.baseurl }} ).
The general architectural direction is a 3D version of the 2D [X Window System].
An X11 Server takes connections from multiple clients and presents a
model of 2d windows that contain text or graphics.
There are different windows managers that connect to the service
and presents controls for the user to move and manage the
windows.

Basil follows this model for 3D. Multiple object sources
connect to Basil and instruct Basil on the coordinates
of objects that are defined as meshes, textures, and shaders.
Basil handles the job of efficiently displaying the objects in the view.

A user interface manager can connect to Basil and present controls
to the user and otherwise manage the appearance and management of
the 3D view.

Basil has the concept of a camera and its view frustum which
defines the view that is displayed.
The camera is moved by external commands and can follow the user's
view of the world.
For augmented reality applications, for instance, the camera would be
moved to correspond to the user's head movements.
Basil would just display the objects in the current camera's view.

Objects locations are either relative to the current coordinate space
or are relative to the camera/frustum position.
By making objects relative to the camera, user interface objects can
be created that are always rendered in the same location in the user's view.

Basil tries to focus only on the tools needed for rendering the 3D scene.
These tools should be the minimal and most general functional implementations
needed to present general features.
For instance, animations added to skeletal models is a way of reducing
latency between movement updates and scene display.
This makes this a generally usable feature that should be considered for Basil.

ADD MORE

Basil's internal data model is described in the [Basil Item Design] document.

Basil's authentication model and protocol is described in the [Basil Authentication] document.
In general, the authentication/authorization system is opaque to Basil.
Basil merely passes around bearer tokens which usually take the form of [JWT] or [OAuth2] tokens.

The common coordinate system is large enough for a planet.
The default coordinate system is [WGS84] which is earth.
For convenience, virtual worlds are mapped to some location on the planet earth.

[X Window System]: https://en.wikipedia.org/wiki/X_Window_System
[Basil Item Design]: {{ "/architecture/Basil/ItemDesign.html" | prepend: site.baseurl }}
[Basil Authentication]: {{ "/architecture/Basil/Authentication.html" | prepend: site.baseurl }}
[JWT]: https://jwt.io/
[OAuth2]: https://oauth.net/2/
[JWT RFC]: https://tools.ietf.org/html/rfc7519

[RFC7519]: https://tools.ietf.org/html/rfc7519
[RFC3339]: https://tools.ietf.org/html/rfc3339
[WGS84]: https://en.wikipedia.org/wiki/World_Geodetic_System 
[OpenSimulator]: http://opensimulator.org/
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 et ai
-->
