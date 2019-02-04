---
layout: post
title: Many Clickable Points on a Leaflet Map With Leaflet.glify
date: 2016-01-07T09:29:50+01:00
categories: [leaflet]
---
# Problem
As seen in the [post before]({{< ref "build-a-coverage-map-with-mongodb.md" >}})
we collected some 12.000 points which we want to display on a leaflet map.
Every attempt to put that many points on one map failed. Firefox completely stopped working, chrome did
a  little bit better and stayed responsive, but that was not a solution at all.

# Solution
I pretty much tried everything `L.geoJSON` layer with points and polygons.
Where polygons would have been a solution maybe, but you can not tell leaflet to just draw the corners of the polygons.
So I needed something else. In the `#leaflet` on freenode i was pointed to the
[Leaflet.glify](http://robertleeplummerjr.github.io/Leaflet.glify/) project.

As I understood, there is a leaflet layer which provides a HTML5 canvas. The location of the points on the canvas is
calculated based on the GPS coordinates (points2pixel) and then are rendered on the canvas with some WebGL point shapes.

So far so good. The best part is, that the lib holds an index about every rendered point and so can respond to clicks.
As it is not easy to really hit a special point there is even a 'find nearest point' algorithm in place so that you almost
always get a point clicked.

The actual code to use the plugin is a one-liner.

{{< gist efwe 683e7db0c4c22f5a52d4 "add_coverage.coffee" >}}

Here's the callback I use when a point is clicked. Interesting detail. You should call `L.DomEvent.stopPropagation(event)` to
stop the event handling if you have other click-backs registered on your map.

{{< gist efwe 28c03b9c10c15cfda165 "point_clicked.coffee" >}}

You can see the result at [https://123k.org/coverage](https://123k.org/coverage).