---
layout: page
style: herbal3d
title: "Herbal3D: Overview of End-to-End Virtual World Infrastructure"
---

**WARNING TO ALL READING THIS:**
As of August 2016, this is an in-progress draft and is growing over time.
Expected completion is in a month or two.
Direct questions to [the author](mailto:herbal3d@misterblue.com).

The text and ideas expressed in this document are Copyright 2016, by Robert Adams
and can be used only with permission.

# Herbal3D: Infrastructure for Virtual Worlds

This document describes an architecture for
storing and displaying three dimensional objects and scenes.
The system is partitioned and defined to enable
display and interaction of several existing virtual world systems
as well as supporting different display hardware implementations.
The Herbal3D system can  support
immersive virtual reality or
can be mapped onto the real world for augmented reality.

# Summary

# Introduction

Some high level goals of the system are:
be open source,
be secure,
be scalable,
and to be generally extensible.

There are many proprietary systems built for virtual worlds
and for augmented reality.
The systems are proprietary and they have their silo of implementation.
This tends to create a closed ecosystem of libraries and expertise that
does not extend across the open space of the Internet.
This project is to be open source so it can be modified, tweaked,
and added to by the Internet community at large rather than just by the implementers of one company.

This project is intended to be secure.
Secure in the fact that it's designed with security from the beginning rather than
having security added later.
This means passing around authentication, continually checking for access level,
and otherwise enforcing access identity and useage controls at all levels.

The system is to use the latest Internet technologies for scalability.
This means things like containers, multiple processors, the APIs,
micro services than anything else which is hip and new but also usable to
create a system that can grow from one user two million.

The system also designed to be modular and extensible.
We expect to grow and it will gather functionality over time.

There are three target audiences for this: 1) individual hobbyists
(requires easy installation and configuration); 2) the small grid or
game that wants a smallish but extensible operation (requires robust
implementation); and 3) a production environment where millions of
users and bots are interacting.

# Background

This project has grown out of my general interest in virtual worlds.
After working with many existing systems which are client server based
(that is there is a world simulation and the server at a client that receives
some representation of that world to display it) I have noticed
requirements
for scalability, security, and content mixing.
This has led me to start thinking about what it would take to build a
3D viewer where one could see into the world as well as the
needed infrastructure behind that viewer to
store, convert, interact and simulate the virtual world.

That is, how can I build a system where an [OpenSimulator] avatar can
stand next to a [HighFidelity] avatar.

Many virtual world implementations are oriented toward specific use cases.
Whether making a multi-user game, or making a very precise real-time
interaction between users, the use case pushes the implementation of existing systems.

Additionally, I've seen several virtual worlds where unbelievably talented creators
have created content in that world but that content was not shareable anywhere else.
Content within one world is locked into that world and the interaction of people
there is locked into that world.
This made me think about how to build a virtual worlds system where an avatar
from one implementation of virtual world could stand next to an avatar from a
different virtual world.
What would such an implementation look like?
How would you build the pieces so that that one viewer allowed the two
simulated avatars to interact from the different worlds?

These lines of thought have lead to the this initial architecture for what
I am calling Herbal3D.
The "herbal" name came from the fact that the first project was a viewer
called "Basil" and from there herb names were used for all the different parts.
From that grew "Herbal3D".

At a less idealistic level, one impetus for creating Herbal3D is the creation of a follow-on
to [OpenSimulator].

OpenSimulator was originally designed as a open source alternative to [SecondLife].
But that was 10 years ago.
In that time, software development workflow has changed,
the tools and workflow have changed,
and, in general, the whole model for implementing a scalabile and massively parallel
application has changed greatly.

OpenSimulator, since it is 10 years old, is also suffering from old age.
It's a centralized service that is broken into regions and thus has scaling problems.
There have been several efforts to make open simulator decentralized and to make it
more scalable in all of its dimensions (like physics and scripting) but, again, the
software is getting long-in-the-tooth and needs updating.

So one of the things Herbal3D is attempting to do is to be an open source alternative
to the many proprietary systems that are being developed for virtual reality
and virtual worlds.
The other solutions are not wrong. They are just not open.

As an open source solution, Herbal3D attempts to be the basis for building
scalable systems.
It is a basis for building virtual worlds infrastructure like OpenSimulator
but using newer technologies and newer design patterns

Refer to [Other Virtual Worlds] for a description of some of the alternatives.

# Architectural Overview

The Herbal3D system consists of several major componenets:

<dl>
  <dt>Session manager</dt>
  <dd>
    The session manager is the coordinating process for an interaction.
    When used for virtual world viewing, for instance, the session manager
    interacts with the user, handles authentication, detects the user's
    session requests, and instantiates instances of the following components
    for the user.
  </dd>
  <dt>Viewer</dt>
  <dd>
    The Viewer is the process that converts 3D Objects into a view for a user
    or a bot 
    <sup>[1](#footnote1)</sup>.
    That is the only job of the viewer.
    The viewer does not do user interface nor does it participate in the 
    simulation of any environment.
    The Viewer models a camera and is told about 3D objects that might be
    viewed. The Viewer does all the processing necessary to display those
    visual objects for the camera. 
    The camera can be anything -- a computer screen, a projection on a wall,
    a head mounted display. Whatever the actual display, the viewer's job
    is to render the 3D objects.
    Other processes around the viewer handle passing the 3D object descriptions
    to the viewer, selecting the sources of the 3D objects, and creating any
    user interface displays to show to the camera.
    The [Viewer]("#Viewer") section below describes the Viewer in detail.
  </dd>
  <dt>Space manager</dt>
  <dd>
    A space manager is the process that tells the Viewer what should appear
    in a specific 3D region. If a space manager is representing a simulated
    virtual world space, for instance, the space manager communicates to the
    virtual world to figure out what objects are in that space and then
    communicates the object location and representation to the viewer.
    There can be multiple space managers feeding the viewer and the space
    managers can be telling the Viewer about different types of objects.
    Again, for a virtual world, there could be several space managers telling
    a viewer about static objects in multiple spaces while other space
    managers are telling the viewer about fast changing objects (like avatars).
  </dd>
  <dt>Space Manager Registry</dt>
  <dd>
    The Viewer's camera is at some point and looking in some direction.
    What does it see?
    The Space Manager Registry is where object sources are looked up.
    The Space Manager Registry accepts queries of the form "I am at location
    X looking in direction R, what do I see?" and a list of Space Managers
    is returned. These are the Space Managers that are sourcing objects for that
    camera view.
  </dd>
  <dt>Asset manager</dt>
  <dd>
  </dd>
  <dt>Virtual world simulator</dt>
  <dd>
  </dd>
</dl>

## Session Manager (Pesto)

## Viewer (Basil)

## Space Managers (Ragu and others)

## Virtual World Simulators

### Asset Repositories

## Space Registry


# Security

<a name="footnote1">1</a>: Multiple viewers can be used to create different
views of a space. The user of the view could be a person or some program (or 'bot')
that wants an image.



[OpenSimulator]: http://opensimulator.org/
[SecondLife]: http://secondlife.com/
[YouTube]: http://youtube.com/
[Other Virtual Worlds]: {{ site.baseurl }}/virtualworlds/herbal3d/otherWorlds.html

<!-- vim: shiftwidth=2 tabstop=2 autoindent expandtab
-->
