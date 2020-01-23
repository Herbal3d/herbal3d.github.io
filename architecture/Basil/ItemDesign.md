---
layout: page
title: "Herbal3D: Basil Item Design
---
# Basil Item Design

A thing that is created and managed within Basil is called an "item".
An item can be most anything from a mesh representation to a UI element
to a camera view.

Items are just identified containers that can be anything: meshes,
animations, cameras.
To the container, functionality is added as "able"`s. That is, a mesh item can
be positionable, or animatable, or whatever-able.

This is a riff on the [entity/component model] found in many game engines.

Access to the item's functionality and the functionality of the "able"s
for the item is through "properties" which are simple name/value pairs.
Each "able" adds some properties to the item and the
item is discovered and operated on by reading and changing the values of the properties.

Every item is an instance of `BItem` which provides the base properties,
item collection management, and the "able" addition, removal, and management.
The base properties are:

| Property | Gettable | Settable | Desc |
|--------- | -------- | -------- |----- |
| _Type    | yes      | no       | One of ?? |
| _Id      | yes      | no       | Unique string identifier for this BItem |
| _OwnerId | yes      | no       | Connection/service that created this BItem |
| _State   | yes      | no       | One of "UNINITIALIZED", "LOADING", "FAILED", "READY", "SHUTDOWN" |
| _Layer   | yes      | no       | String name of group this BItem belongs to |
| _Unique  | yes      | no       | A unique item key supplied by the creator or generated if not |

# ABLEs

## Displayable

This defines a displayable item. It usually is a MeshSet.

| Property | Gettable | Settable | Desc |
|--------- | -------- | -------- |----- |
| DType    | yes      | no       | One of ?? |
| AABB     | yes      | no       | axis aligned bounding box as "x,y,z,x,y,z" |

Other properties depend on the value of "_DisplayableType":

### MeshSet

| URL      | yes      | no       | URL needed for loader |
| LoaderType | yes    | no       | One of "GLTF", ?? |
| AssetAuth  | yes    | yes      | token to use for fetching the asset |

## Instanceable

This ability represents an instance of a displayable in the 3d space.
Often, there is a Displayable and Instanceable on a single item --
the case where there is a single instance of the displayable asset
in the 3d space. There could be a single Displayable item with
many Instancable items that reference that displayable.
In this latter case, there will not be Displayable properties
in this item but, instead, a "DisplayableRef" property will be present.

| Property | Gettable | Settable | Desc |
|--------- | -------- | -------- |----- |
| Pos      | yes      | yes      | Position as "x,y,z" |
| Rot      | yes      | yes      | Rotation as "x,y,z,w" |
| PosCoord | yes      | yes      | Coordinate system code for "Position" |
| RotCoord | yes      | yes      | Coordinate system code for "Rotation" |
| DisplayRef | yes    | no       | ID of item with Displayability if not present here |

## Trackingable

## Animatable

## UIable

[entity/component model]: https://en.wikipedia.org/wiki/Entity_component_system
[JWT]: https://jwt.io/
[JWT RFC]: https://tools.ietf.org/html/rfc7519
[RFC7519]: https://tools.ietf.org/html/rfc7519
[RFC3339]: https://tools.ietf.org/html/rfc3339
[OAuth2]: https://oauth.net/2/
[WGS 1984]: http://earth-info.nga.mil/GandG/publications/tr8350.2/tr8350_2.html
[OpenSimulator]: http://opensimulator.org/
[View Service]: http://loc-loc.net/
[Herbal System]: http://herbal3d.org/
[Basil Viewer]: http://basilviewer.org/
[Pesto]: http://misterblue.github.io/pesto/
[Ragu]: http://misterblue.github.io/ragu/
[BSD License]: http://opensource.org/licenses/BSD-3-Clause
[MIT License]: http://opensource.org/licenses/MIT
[Apache License]: http://opensource.org/licenses/Apache-2.0
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 et ai
-->
