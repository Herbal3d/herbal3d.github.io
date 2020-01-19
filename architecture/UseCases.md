---
layout: default
navigation: architecture
---

# Use Cases

## Virtual World Browser

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

## Virtual World Goggles

## Augmented Reality

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

## Multiple Virtual Worlds

<!-- vim: ts=2 sw=2 ai et spell
-->
