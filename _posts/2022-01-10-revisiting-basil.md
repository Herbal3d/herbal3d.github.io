---
layout: post
title:  "Revisiting Basil"
date:   2022-01-19 20:24:33 -0700
categories: herbal3d basil update
status: publish
---
After a few years of wandering around in other open-source projects
like [Vircadia] and [ROS2], I've come back to do some work on Basil
and its components.

I've reworked the protocol into an entity/ability model and rewrote a
lot of the Javascript code with a transport/protocol/message architecture.
I am now re-writing the OpenSimulator plugins to work with the new
protocol and architecture.

Why revisit? With the "metaverse" becoming A Thing, I really felt
the idea of a viewer that is separate from world logic is more important
than ever.

Also, since I'm a virtual world old timer (been doing this stuff for over
20 years), I have the nagging feeling that virtual worlds are a nitch.
A big nitch for sure -- it will create billion dollar companies and
industries. But augmented reality is where the real life changing
application will happen. Think how the touch-screen, graphical, cell
phone has changed the world.

Thus, the Basil architecture is again important.

[Vircadia]: https://vircadia.com
[ROS2]: https://github.com/ros2
