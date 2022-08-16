---
layout: post
title:  "Basil Sleeping"
date:   2022-08-16 20:24:33 -0700
categories: herbal3d basil update
status: publish
---
Over the last few months I've done a lot of work on Basil --
both the [browser client] and the [OpenSimulator] region modules.
I've gotten it to the point where an avatar representation
can be navigated around a dedicated region.

Then I've stumbled over more little problems. Like needing to
have the connection to [OSGrid] login server be HTTPS.
[OpenSimulator] grid installations often do not have TLS
connections to the grid services
so both the login XMLRPC request and the connections
to the WebSockets are problematic. Browsers require
TLS once anything is loaded via HTTPS.

Then there is the project of converting [OpenSimulator] content
which requires a lot of work optimizing and reformatting.
I'd decided to use the [Cesium 3D-tiles] format to contain
the multiple levels of LOD and layered data.
The format is straight forward but it would be a lot of work
to do the mesh decimation and the texture atlasing.

Then there are the avatars and animations. And what to do about
the new avatar formats that are being used by everyone else
in the new virtual world universe?

That brings me to the largest barrier -- [OpenSimulator] is
just not made for the modern VR environment.
Not only is the content format not what the rest of the 
world is using, but avatar movement is not two way so anything
other than gesture animations is not possible.

And there is a world of new virtual worlds with active
and innovative communities. This project cannot keep up.
If I had completed Basil a decade ago (when I started)
it might be adaptable. The general concept of a universal
viewer might be adaptable but that's a different project.

So, for the moment, Basil is sleeping. Maybe it will awake
someday.

[Cesium 3D-tiles]: https://cesium.com/why-cesium/3d-tiles/
[OSGrid]: https://osgrid.org
[OpenSimulator]: http://opensimulator.org
[browser client]: https://github.com/Herbal3d/basil
