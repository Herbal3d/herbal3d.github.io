---
layout: page
style: herbal3d
title: "RaguOS: Design Discussion"
---
<div id="herbal-toc">
  <ol>
    <li><a href="#raguos-module-based-spaceserver-frontend-for-opensimulator">Introduction</a></li>
      <ol>
        <li><a href="#choice-of-region-modules">Choice of Region Modules</a></li>
      </ol>
    <li><a href="#loden">Loden</a></li>
      <ol>
        <li><a href="#region-content-layers">Region Content Layers</a></li>
          <ol>
            <li><a href="#static">Static Layer</a></li>
            <li><a href="#dynamic">Dynamic Layer</a></li>
            <li><a href="#actors">Actors Layer</a></li>
            <li><a href="#editing">Editing Layer</a></li>
          </ol>
        <li><a href="#level-of-detail-computation">Level of Detail Computation</a></li>
        <li><a href="#division-hash">Division Hash</a></li>
        <li><a href="#texture-simplification">Texture Simplificiation</a></li>
        <li><a href="#asset-storage-and-access">Asset Storage and Access</a></li>
        <li><a href="#region-description-assets">Region Description Assets</a></li>
      </ol>
    <li><a href="#raguos">RaguOS</a></li>
      <ol>
        <li><a href="#editing-mode">Editing Mode</a></li>
      </ol>
    <li><a href="#revision-history">Revision History</a></li>
    <li><a href="#legal-stuff">Legal Stuff</a></li>
  </ol>
</div>

# RaguOS: Module Based SpaceServer Frontend for OpenSimulator

"Ragu" is the code name for a SpaceServer that makes an [OpenSimulator] grid/region available
to a [Basil Viewer].
This document describes the implementation of [RaguOS] which is a set of [OpenSimulator] region
modules that convert and presents region contents so it fits into the [Herbal System].

Because of the limitations of the OpenSimulator region content format, the region must
be converted before use. This conversion provides:

* Formats that can be used by simplified viewers (convert to GLTF, JPEG, ...);
* Reduces triangle and draw count (for better performance in browser and mobile [Basil Viewer]s);
* Converts content into "drawable" form for sending to a viewer
  (as opposed to sending the original, editable content);
* Provides level-of-detail ("lod") versions of region content (for more efficient display)

This conversion is done by the [Loden] region module.

Once the region contents have been converted, [RaguOS] acts as a SpaceServer to a [Basil Viewer]
to display and interact with the region's contents.

## Choice of Region Modules

These routines ([RaguOS], [Loden], etc.) were created as [OpenSimulator] region modules
because:

* ease of implmentation;
* [Loden] is essentially a "copy-bot" and thus creating it as a separate program seemed questionable;
* modules are the extensibility system for [OpenSimulator];
* [RaguOS] requires object update events to implement editing, avatar movement, object movement,
  and all other region update operations

Two options for making them separate programs would be to either:

* make them a proxy that talks LLLP[^1] and acts on all the content changes and events as presented by the legacy viewer protocol, or
* create a region module (probably based on [Dispatcher]) that presents a new protocol

The major advantage of separate programs is not requiring the rebuilding of the regions
simulator with the [Loden] and [RaguOS] modules. This is a great advantage for acceptance of
the [Basil Viewer] by the [OpenSimulator] community. 

# Loden

[Loden] [^2] converts an OpenSimulator's region's contents into formats useful to a Basil viewer
([GLTF], JPEG, ...) organized into a hiearchical spacial structure of increasing displayable detail.
This does not replace the OpenSimulator's asset store but is a displayable version
of the region assets.

[Loden] organizes the region assets into four different "layers". These are displayed together by a
[Basil Viewer] to present a complete view of the region. Each of the layers is organized into a tree
of spacial areas with increasing level-of-detail. The tree representation is modeled after the
[CesiumJS] project's [3DTiles]. [3DTiles] defines bounding boxes of objects of some level-of-detail
that also has "children" of sub-bounding boxes that define objects of increased detail. The process
is for a viewer to start at the top of the tree and work down the tree to find the appropriate level-of-detail
for the viewer resolution, view distance, and camera location.

To that end, it is envisioned that [Loden] will convert a region into a tree that includes a top
level of the whole OpenSimulator region as one mesh (for distance viewing) and then sub-areas of the
region containing ever increasing level-of-detail of the assets.
The construction of the tree will depend on the type of scene objects.

## Region Content Layers

As mentioned above, [Loden] separates the region scene objects into four classes that make up
layers for the [Basil Viewer]. These layers are:

* Static -- Primitive objects that are non-physical and do not contain scripts;
* Dynamic -- Any object that is either physical (can move) or contains scripts (can move or modify itself);
* Actors -- Avatars in the scene;
* Editing -- Object in the process of being editted

### Static Layer

The initial implementation will replace the whole region, single mesh with a single GLTF that describes
the whole region's contents. The second level will be a quad division of the region. Initially, the
divisions will be of equal size but future versions will split the region until there is an equal
number of scene objects in each division.

The quad divisions will each be increasing level-of-detail until the lowest level will have scene
object versions that are visually nearly identical to the underlying [OpenSimulator] primitives.
Depending on ease of implementation, each level may split into other quad divisions.

Top level: region as one GLTF with reduced mesh detail and texture
Top-1: region split into four sub-regions each as one GLTF
Top-2: quads with linksets merged into one mesh with texture atlases
Top-3: quads with meshes with reduced complexity and textures reduced to 100ppm
Top-4: quads with meshes at full prim detail and textures full size

Each level and quad will have a unique, content-based hash supplied with the GLTF file that is
used to decide whether to build that level/quad after any content changes.

### Dynamic Layer

### Actors Layer

### Editing Layer

## Level of Detail Computation

As stated in the [3DTiles] documentation:

> A tile's geometric error defines the selection metric for that tile. Its value is a nonnegative number that specifies the error, in meters, of the tile's simplified representation of its source geometry. The root tile, being the most simplified version of the source geometry, will have the greatest geometric error. Then each successive level of children will have a lower geometric error than its parent, with leaf tiles having a geometric error of or close to 0.

((Description of the level-of-detail specification.))

## Division Hash

((SHA256 hash of object characteristics and location of all objects in ?? order))

## Texture Simplification

((Textures reduced in resolution based on in-world size))

((Texture types being JPEG (compression factor??) or PNG if texture contains transparancy))

((Texture atlas??))

## Editing Mode

## Asset Storage and Access

((How assets are stored and how the HTTP server is set up to allow access to them))

((Access control))

## Region Description Assets

((Description of the JSON files created for region root information))

# RaguOS

## Editing Mode

Description of RaguOS operation

# Revision History

# Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].

Since every idea in  the world is covered by a patent somewhere, I make
no claims as to the ownership or availability of any design or concept
described above.



[^1] LLLP: "Linden Lab Legacy Protocol". The combination of TCP and UDP communications used by
all [OpenSimulator] grids and Third Party Viewers.

[^2]: The "loden" name is a play on "LOD" or level-of-detail as to level-of-detail'en a region.

[OpenSimulator]: http://opensimulator.org/
[RaguOS]: https://github.com/Herbal3d/RaguOS
[Loden]: https://github.com/Herbal3d/Loden
[Dispatcher]: https://github.com/cmickeyb/dispatcher
[GLTF]: https://www.khronos.org/gltf/
[CesiumJS]: https://cesiumjs.org/
[3DTiles]: https://github.com/AnalyticalGraphicsInc/3d-tiles
[WGS 1984]: http://earth-info.nga.mil/GandG/publications/tr8350.2/tr8350_2.html
[Herbal System]: http://herbal3d.org/
[Basil Viewer]: http://basilviewer.org/
[Pesto]: http://misterblue.github.io/pesto/
[BSD License]: http://opensource.org/licenses/BSD-3-Clause
[MIT License]: http://opensource.org/licenses/MIT
[Apache License]: http://opensource.org/licenses/Apache-2.0
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 et ai
-->
